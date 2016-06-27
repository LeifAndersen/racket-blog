    Title: Web Application Development in PLT Scheme
    Date: 2008-10-06T15:33:00
    Tags: Jay McCarthy, continuations, documentation, drracket, tutorials, web-server

Many users often post to the `plt-scheme` mailing list asking for introductions
to Web application development in PLT Scheme. They've heard of the
continuation-based PLT Web Server and want a gentle introduction.
Unfortunately, there has been a distinct lack of good documentation and
tutorials for the server. Taking the cue from two users:
[Jens Axel Soegaard](http://blog.scheme.dk/2007/01/introduction-to-web-development-with.html)
and [David Reynolds](http://alwaysmovefast.com/category/plt-scheme/), we've written a tutorial with Danny Yoo.

The tutorial is available at
[http://docs.plt-scheme.org/continue/index.html](http://docs.plt-scheme.org/continue/index.html).
It walks through the creation of a blog application, introducing features slowly
and culminates in an SQL-backed database for the posts. Of particular interest,
is the [fast start servlet setup](http://docs.racket-lang.org/web-server/run.html#%28part._insta%29)
based on the Instaservlet package from [Untyped](http://www.untyped.com/).

Please take a look and pass along this as a pointer to those who may be
interested in PLT Scheme.
