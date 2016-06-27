    Title: Getting rid of set-car! and set-cdr!
    Date: 2007-11-12T07:53:00
    Tags: Matthew Flatt, mutation, new-feature

Functional is Beautiful
-----------------------

Scheme is a “mostly functional” language. Although Schemers don’t hesitate to
use `set!` when mutation solves a problem best, Scheme programmers prefer to
think functionally. Purely functional programs are easier to test, they make
better and more reliable APIs, and our environments, compilers, and run-time
systems take advantage of functional style.

A Schemer’s functional bias is especially strong when writing programs that
process and produce lists. The `map` function, which does both, is a thing of
beauty:

```racket
(define (map f l)
 (cond
   [(null? l) '()]
   [else (cons (f (car l)) (map f (cdr l)))]))
```

The `map` function is most beautiful when the given f is functional. If f has
side-effects, the the above implementation over-specifies `map`, which is
traditionally allowed to process the list in any order that it wants (though
PLT Scheme guarantees left-to-right order, as above). Arguably, when some other
Schemer provides a non-functional `f`, then it’s their problem; they have to deal
with the consequences (which may well be minor compared to some benefits of
using mutation).

The `map` function might also receive a non-list, but the `map` implementor can
guard against such misuse of `map` by wrapping it with a check,

```racket
(define (checked-map f l)
  (if (list? l)
      (map f l)
      (error 'map "not a list")))
```

and then exporting `checked-map` instead of the raw `map`. This kind of checking
gives nicer error messages, and it helps hide implementation details of `map`. We
could further also imagine that the raw `map` is compiled without run-time checks
on `car` and `cdr`.

The Problem with Mutable Pairs
------------------------------

What if someone calls `checked-map` like this?:

```racket
(define l (list 1 2 3 4 5))
(checked-map (lambda (x)
               (set-cdr! (cddr l) 5))
             l)
```

The `f` provided to `map` in this case is not purely functional. Moreover, it uses
mutation in a particularly unfortunate way: the `list?` test in `checked-map`
succeeds, because the argument is *initially* a list, and the mutation is
ultimately discovered by a call to `cdr` --- but only if checks haven't been
disabled.

If you’re a Schemer, then unless you’ve seen this before, or unless you thought
a bit about the title of this section, then you probably didn't think of the
above test case for `map`. A Schemer’s view of lists is so deeply functional that
it's hard to make this particular leap.

Furthermore, this example is not contrived. If you have either Chez Scheme
version 6.1 or a pre-200 MzScheme sitting around, calling `map` as above leads to
a seg fault or an invalid memory access:

```
Chez Scheme Version 6.1
Copyright (c) 1998 Cadence Research Systems

> (define l (list 1 2 3 4 5))
> (map (lambda (x) (set-cdr! (cddr l) 5)) l)

Error: invalid memory reference.
Some debugging context may have been lost.
```

The `map` example illustrates how mutable pairs can break a Schemer’s natural and
ingrained model of programming. Of course, if optimizing and providing friendly
error messages for `map` were the only issues with mutable pairs, then it
wouldn’t matter; Scheme implementors are smart enough to (eventually) get this
right. Unfortunately, the underlying problem is more pervasive.

In the API for a typical Scheme library, lists can be used for many kinds of
input and output. Flags for options might be provided in a list. A function
might provide information about the current configuration (e.g., the current
items in a GUI list box) in a list. Procedures or methods that deal gracefully
with list mutation are few and far between. In most cases, the result of
unexpected mutation is merely a bad error message; sometimes, however,
unexpected mutation of a list can break the library’s internal invariants. In
the worst case, the library whose internal invariants are broken plays some
role in a system’s overall security.

Mutable lists also interfere with the language’s extensibility. The PLT Scheme
contract system, for example, offers a way to wrap an exported function with a
contract that constrains its input and outputs, which are optionally (in
principle) enforced by run-time checks. Higher-order contracts, such as “a list
of functions that consume and produce numbers”, require wrappers on sub-pieces,
and these wrappers can be installed only by copying the enclosing list. Copying
a mutable list changes the semantics of a program, however, whereas contracts
are supposed to enforce invariants without otherwise changing the program.
Copying an immutable list creates no such problem.

Finally, mutable lists make the language’s specification messy. The R6RS
editors spent considerable energy trying to pin down the exception-raising
guarantees of map; the possibility of mutable pairs made it difficult to
provide much of a guarantee. The standard says that implementations *should*
check that the lists provided to map are the same length, but it’s not worth
much to require that check, since an argument’s length as a list can change via
mutation to the list’s pairs.

Switching to Immutable Pairs
----------------------------

The designers of PLT Scheme long ago recognized the problems of mutable pairs,
and we introduced functions like `cons-immutable` and `list-immutable` to support
programming with immutable lists. These additions solved some problems --- but
only in the cases where we were careful to use immutable lists. The R6RS
editors also recognized the problems of mutable pairs, so that `set-car!` and
`set-cdr!` were banished to their own library --- but programmers are still free
to use that library.

While these are worthwhile steps for many reasons, they do not solve the
underlying problem. Library implementors who deal in lists must still either
set up elaborate guards against mutation, pretend that the problem doesn't
matter, or require the use of a special immutable-list datatype that is
incompatible with libraries whose authors set up elaborate guards or ignore the
problem.

Why all this hassle? If most Scheme code really does use and expect pairs in a
functional way, can't we just switch to immutable pair? Most Scheme code will
still work, untold security holes will have been closed, specifications will
become instantly tighter, and language extensions like contracts will work
better.

Schemers have been reluctant to make this leap, because it has never been clear
just how much code relies on mutable pairs. We don’t know how much the switch
will cost in porting time and long-term incompatibility, and we don’t really
know how much we will gain. We won't know until we try it.

For PLT Scheme v4.0, we’re going to try it. In our main dialects of Scheme
(such as the mzscheme language), `cons` will create immutable pairs, and `pair?`
and `list?` will recognize only immutable pairs and lists. The `set-car!` and
`set-cdr` procedures will not exist. A new set of procedure `mcons`, `mcar`, `mcdr`,
`set-mcar!`, and `set-mcdr!` will support mutable pairs. (A related v4.0 change is
that `define-struct` by default creates immutable structure types.)

Of course, PLT Scheme v4.0 will support an R5RS language where `cons` is `mcons`,
and so on, so many old programs can still run easily in the new version. The
difference is that interoperability between R5RS libraries and PLT Scheme
libraries will be less direct than before.

Experience So Far
-----------------

PLT Scheme v3.99.0.2 exists already in a branch of our SVN repository, and it
will soon move to the SVN trunk. That is, we have already ported at least a
half million lines of Scheme code to a dialect without `set-car!` and `set-cdr!`.

The conversion took about eight hours. Obviously, relatively little code had to
change. The following are the typical porting scenarios:

* The `reverse!` and `append!` functions were frequently used for “linear
  updates” by performance-conscious implementors. As our underlying Scheme
  implementation has improved, however, the performance benefits of these
  functions has become less. All uses could be replaced with `reverse` and `append`.
* The `set-cdr!` operation was often used to implement an internal queue. Such
  internal queues were easily changed to use `mcons`, `mcar`, `mcdr`, and `set-mcdr!`.
* An association-list mapping was sometimes updated with `set-cdr!` when a
  mapping was present, otherwise the list was extended. Since the extension case
  was supported, it was easy to just update the list functionally. (The relevant
  lists were short; if the lists were long, the right change would be to use a
  hash table instead of a list.)
* A pair was sometime used for an updatable mapping where a distinct
  structure type is better. The quick solution was to throw in a mutable box in
  place of the value.

The PLT Scheme code might be better positioned for the switch than arbitrary
Scheme code. Most of it was written by a handful of people who understood the
problems of mutable pairs, and who might therefore shy away from them. However,
the PLT Scheme code base includes a lot of code that was not written
specifically for PLT Scheme, including Slatex, Tex2page, and many SRFI
reference implementations. With the exception of SRFI-9, which generalizes set!
to work with pairs, the SRFI implementations were remarkably trouble free.
(Thanks to Olin Shivers for making mutation optional in the “linear update”
functions like reverse! from SRFIs 1 and 32.)

In addition, we looked at a number of standard Scheme benchmarks, which can be
found here:

[http://svn.plt-scheme.org/plt/trunk/collects/tests/mzscheme/benchmarks/common/](http://svn.plt-scheme.org/plt/trunk/collects/tests/mzscheme/benchmarks/common/)

Of the 28 benchmarks, eight of them mutate pairs. Four of those are trivially
converted to functional programs, along the lines of the scenarios above. One,
`destruct`, is designed specifically to test mutation performance, so it makes no
sense to port. Another, `sort1`, is a sorting algorithm that inherently relies on
mutation; a functional sort is obviously possible, but that would be a
different benchmark. The conform benchmark uses mutable pairs for tables in a
relatively non-local way; as a modern Scheme program, it would probably be
written with structures, but it’s not trivial to port. The peval benchmark uses
pairs to represent Scheme programs, and it partially evaluates the program by
mutating it, so it is not trivial to port. To summarize, out of 28 old,
traditional benchmark programs, only two represent interesting programs that
are not easily adapted to immutable pairs. (They run in PLT Scheme’s R5RS
language, of course.)

Finally, we selected a useful third-party library that is not included with PLT
Scheme. We checked the generic SSAX implementation (not the PLT Scheme
version), and we found a couple of uses of `set-car!` and `set-cdr!`. Again, they
fall into the above queue and association-list categories that are easily and
locally converted.

Meanwhile, as we start to use v3.99 to run scripts in our day-to-day work,
immutable pairs have so far created no difficulty at all. So far, then, our
optimism in trying immutable pairs seems to be justified; it just might work.

But It’s Lisp Tradition!
------------------------

A typical response to news of the demise of mutable pairs is that it will
create lot of trouble, because mutable pairs are Scheme tradition, and surely
lots of useful old code exploits them in lots of places.

We’re eager to hear whether anyone has such code. Our initial hypothesis is
that practically all old code falls into one of two categories:

* The code is easily ported to immutable pairs, along the same lines as above
  (i.e., local queues and small association lists).
* The code so old and generic that it can be run as an R5RS program. It
  won’t call into the large PLT Scheme set of libraries that will expect
  immutable pairs, and it can easily be used as a library with wrappers that
  convert mutable pairs back and forth with immutable pairs.

Frankly, we’re not so eager to hear opinions based on guesswork about existing
code and how it might get used. Download v3.99 from SVN or as a nightly build
when it becomes available; let us know your guesses about how running your old
code would go, but then let us know what *actually* happens.

The immutable-pairs plan for v4.0 is not set in stone, but we won’t make the
decision based on guesswork. More libraries (other than R5RS) to aid
compatibility may be useful, but so far we don’t have a tangible need for them.
In any case, we’ll revert to mutable pairs only if significant experience with
the pre-release version demonstrates that it really won’t work.
