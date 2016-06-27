    Title: Small is Beautiful, Large is Useful, and Scheme is Both
    Date: 2007-06-14T14:05:00
    Tags: Matthias Felleisen

They say, Scheme is small and this is good.

Have you heard of X? No? It is the smallest computational basis. It is a single
function that can compute everything a Turing machine can compute; a Church
lambda calculus; a Post model; a RAM; a what-have-you model of computation.
Indeed, X is so simple that two equations suffice to specify it completely
[[Barendregt](http://www.andrew.cmu.edu/user/cebrown/notes/barendregt.html), [page 166](http://www.andrew.cmu.edu/user/cebrown/notes/barendregt.html#8)].
Imagine that: a complete language report in two lines;
a compiler that fits in a few K instead of Ms; no more arguments about
smallness.

Small alone can't be any good. If you used X alone, your programs would be the
size of the universe or something like that. That's what the theory of
computability teaches us [[Church and Turing](http://en.wikipedia.org/wiki/Church-Turing_thesis)]. Adding LAMBDA and a few
primitives to get a pure functional language isn't good enough either. That's
what the theory of expressiveness shows [[Felleisen](http://www.ccs.neu.edu/racket/pubs/#scp91-felleisen); [Mitchell](http://theory.stanford.edu/people/jcm/publications.htm); [Riecke](http://dl.acm.org/citation.cfm?id=99583.99617)]. And,
using an R5RS Scheme to build large systems with many people at a dozen sites
isn't doable either. That's what the PLT experience determined.

When we set out to construct DrScheme using MzScheme, we also conducted an experiment:

> Could we really build a graphical system that manages (shared) resources
> and that provides excellent error feedback with just plain Scheme? 

Could we just add enough libraries to do all this? Or would we have to change
the kernel of the language? As much as we tried to keep MzScheme small, it
became clear quickly that we needed exceptions, structures, module-like
features, a mechanism for concurrency, a way to manage resources such as
windows, tcp connections, and so on. The list isn't infinite but it is much
longer than I expected. Our "Revenge of the Son of the LISP machine" paper is a
good summary for the state of the art around 1999 [[Flatt and Son](http://www.cs.utah.edu/~mflatt/publications/index.html)].

As language designers we stepped back time and again to look at our monster.
What could we remove? What would we have to add in response? For some five
years, we had first-class modules (units) and first-class classes in the core
of the language. We had almost banned macros. They were so ugly I stopped
teaching about them because I did want to use our own dog food in my courses
but I couldn't stomach the macro system. It was such a step back from Eugene's
extend-syntax. But then Matthew figured out the next big step in macro and
module technology [[Flatt, You Want It When?](http://www.cs.utah.edu/~mflatt/publications/index.html)]. And with that out went units and
classes from the core and many other things. So we learned lessons, and we need
to keep building systems to learn more.

I have no question that the idea of Scheme is beautiful. At the same time, I
have also learned that if I wish to use this beautiful idea in practice, I need
to add the ingredients that it takes to build large systems. R6RS reflects this
insight, and I am happy about it.
