    Title: XML Transformation in Scheme
    Date: 2007-05-10T12:04:00
    Tags: Noel Welsh

Selenium is a tool for testing web applications, the core of which is a
Javascript library that controls a web browser. With the Selenium IDE you can
convert your actions in a web browser into tests, and with the Selenium Remote
Control you can control a web browser from code. I've recently been working on
adding Selenium Remote Control bindings to PLT Scheme, which has resulted in a
nice and hopefully instructional demonstration of XML transformation in PLT
Scheme

The Selenium Remote Control is controlled by sending simple messages over HTTP.
The format of the messages isn't important. What is, is that there are a lot of
them, and the API is specified in a file called iedoc.xml that comes with
Selenium. The Java/Python/Ruby bindings are generated using XSL. If I was to
use XSL I'd have a processing pipeline that uses three languages (XSL, Java,
Scheme) which is two more than I'd like. Hence I turned to WebIt!, an XML
transformation DSL written in Scheme, to create an all Scheme pipeline. The
rest of this post wshows the steps I used to transform the Selenium API into
Scheme code using WebIt! I think this is interesting in its own right, but also
serves as a nice demonstration of the power of macros, which WebIt! makes
extensive use of.

My first step is to get an idea of the structure of the XML. The bits I'm
interested in look like this:

```xml
<function name="click">
  <param name="locator">an element locator</param>
  <comment>Clicks on a link, button, checkbox or radio button.
  If the click action causes a new page to load (like a link usually
  does), call waitForPageToLoad.</comment>
</function>
```

Let's read in the XML file and extract all the function elements. For this I'll
use SSAX and SXPath:

```racket
(require
 (planet "ssax.ss" ("lizorkin" "ssax.plt" 1))
 (only (planet "sxml.ss" ("lizorkin" "sxml.plt" 1)) sxpath))

(define api
  (with-input-from-file "iedoc.xml"
    (lambda () (ssax:xml->sxml (current-input-port) '()))))

(define functions
  ((sxpath '(// function)) api))
```

Ok, so we have all the functions. Now let's parse them into a more useful
datastructure. Here's my first attempt:

```racket
(require (planet "xml.ss" ("jim" "webit.plt" 1 5)))

;; struct function : string (listof string)
(define-struct function (name params))

;; parse-function : sxml -> function
(define (parse-function fn)
  (xml-match fn
    [(function name: ,name
               (param name: ,param-name ,desc) ...
               (comment ,_ ...))
     (make-function name (list param-name ...))]))

(map parse-function functions)
```

The xml-match macro is a pattern matcher for SXML. You specify the “shape” of
the SXML, and if the input matches the pattern the following expressions are
evaluated:

```racket
(xml-match value
  [(pattern expression ...)]...)
```

The simplified form of a pattern is:

> (element ...) matches an element with the given name.
> name: value matches an attribute with the given name and value.
> ,binding binds the value of binding to the given name in the scope of the following expressions.
> ... matches zero or more of the preceeding patterns.

In our example the pattern is:

```racket
     (function name: ,name
               (param name: ,param-name ,desc) ...
               (comment ,_ ...))
```

So we're looking for an element called function with an attribute called name
the value of which is bound to name. Then follows zero or more param elements,
with attribute name, the value of which is bound to param-name. Finally we
expect a comment element which can contain any amount of data. The use of `_` as
the binding name is a common convention to indicate data we don't care about
but must still match to make our pattern complete.

I run the code in DrScheme and see the result:

```
xml-match: no matching clause found
```

Oops. So our pattern isn't complete. We've also seen one flaw of WebIt!: it
doesn't give very good error messages. However we can easily fix this by adding
a catch all pattern that raises an error telling us what we failed to match.
The code follows. Notice that I've also added pretty printing to make the
unmatched SXML easier to read.

```racket
(require (lib "pretty.ss"))

;; parse-function : sxml -> function
(define (parse-function fn)
  (xml-match fn
    [(function name: ,name
               (param name: ,param-name ,desc) ...
               (comment ,_ ...))
     (make-function name (list param-name ...))]
    [,err (let ([op (open-output-string)])
            (pretty-print err op)
            (error (format "Didn't match ~n~a~n" (get-output-string op))))]))
```

Run this code and you'll see the error occurs as we don't allow the description
to contain more than one element. This is easily fixed by extending the pattern
to `,desc ....` The next error is more interesting. The function element contains
a return element. The WebIt! pattern language doesn't allows us to express
optional patterns, so we have to duplicate our pattern and include the case of
return. This also requires we extend the defintion of the function structure.

```racket
;; struct function : string string (listof string)
(define-struct function (name return params))

;; parse-function : sxml -> function
(define (parse-function fn)
  (xml-match fn
    [(function name: ,name
               (param name: ,param-name ,desc ...) ...
               (comment ,_ ...))
     (make-function name "void" (list param-name ...))]
    [(function name: ,name
               (return type: ,type ,return-desc ...)
               (param name: ,param-name ,desc ...) ...
               (comment ,_ ...))
     (make-function name type (list param-name ...))]
    [,err (let ([op (open-output-string)])
            (pretty-print err op)
            (error (format "Didn't match ~n~a~n" (get-output-string op))))]))
```

This works! This is as far as I want to go in this article. We've seen how we
can use SSAX. SXPath, and WebIt! to create XML transforms in pure Scheme. There
is a lot more to all of these packages but what we've used is sufficient for
many uses. The rest of the code to create Scheme from the API is quite
straightforward and specific to Selenium. If you're curious read the source of
the Selenium PLaneT package, which will be released soon.

This post also appears on Untyping
