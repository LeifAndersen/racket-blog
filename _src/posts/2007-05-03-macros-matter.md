    Title: Macros Matter
    Date: 2007-05-03T17:35:00
    Tags: Matthias Felleisen

Thank you Jens for setting up this Blog.

PLT Scheme is a 12-year old project now and it is definitely time to open it up
to the world. The language and the project has contributed numerous ideas and
products to the world. This covers programming languages (units, mixins, an
implementation of cml-style concurrency, etc); programming tools (drscheme,
check-syntax, transparent repls, module browsers, etc), programming pedagogy
(htdp, htdc); program engineering (we resurrected the "expression" problem, web
programming and continuations); and some more.

Time and again, people have asked me what I consider the one 'feature' that
distinguishes us from the rest of the hordes of programming languages. I always
respond with a single word: 

> macros.

We have pushed macros hard, and we have accomplished a lot with them. I
conjecture that without macros, we would never have achieved the level of
productivity that this group displays.

Of course, everyone else in academia works on types. ML's module type system of
the third kind and Haskell's system-complete type system are serious challenges
to anyone. It is probably true that you shouldn't consider yourself a
programmer if you can't read and write some of those type-laden programs, and I
seriously believe that they are the next generation of influential languages.

For the generation-after-the-next then, I see "macros" as one of the big topics
(next to concurrency). A real programmer will have to know how Lisp and
Scheme-style macros can reduce labor by orders of magnitude, how macros provide
the tools for creating the "ultimate abstraction" in the form of
domain-specific and embedded languages (Hudak's words). And there is no better
place to start with than PLT Scheme's macro system.

So I would like to dedicate this blog to all things macros and everything else
that matters in (and to) PLT Scheme.
