    Title: Don't say "abstract" (instead say "general")
    Date: 2007-09-14T17:56:00
    Tags: John Clements, meta

The word "abstract" is common in computer science. An abstract thing is one
where some part of the whole is unspecified. For instance, the expression "3*x
+ 3" is an abstraction of the expression "3*4+3", because the "x" is
unspecified. Likewise, a function is an abstraction over some set of values,
supplied when the function is called.

The word "general" is not at all common in computer science. In
non-computer-science use, the word "general" is used to describe things that
may be applied to more than one thing or situation. For instance, a "more
general solution" is one that applies not just to the problem at hand, but
instead to a larger set of problems.

From a computer science perspective, things that are abstract are also general.
Things that are general are also abstract. Substituting the word "general" for
the word "abstract" would not be a terrible hurdle.

From a non-computer-science perspective, however, "general" and "abstract" have
very different implications. Something that is general is better: it is more
useful, it applies more frequently. Something that is abstract, though, is
worse: it is lacking detail, it is non-concrete.

This is one difference--the major difference?--between computer science (and of
course mathematics) and the real world: *the abstract is no less concrete*. We
can abstract over expressions using functions, and we can even abstract over
syntactic things, using hygienic macros. The result of such abstraction is a
perfectly well-defined element in our universe of expressions.

In computer science, then, the pejorative sense of the word "abstract" is
misleading, and the use of the terms "abstract" and "abstraction" merely
provides ammunition for those who wish that we could all still be writing
assembly language.

I suggest instead the use of the word "general."

John "purveyor of barbarous neologisms" Clements
