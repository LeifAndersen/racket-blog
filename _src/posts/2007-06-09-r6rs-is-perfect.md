    Title: R6RS is "perfect"
    Date: 2007-06-09T13:15:00
    Tags: Matthias Felleisen

When I read the "side by side" and "head to head" descriptions of the
alternatives facing the Scheme community (see Comp.Lang.Scheme and the R6RS
mailing list), I am wondering which one is which and which one is better.

* Is it really good that Scheme (the spec) doesn't support a module system?
* Is it really good that almost all major implementations support their own version of a module system?
* Is it really good that programmers can't even leave the module structure intact when porting code? 

Imagine your own similar questions and add them here. We have lived in a
side-by-side universe for a long time, and there are quite a few programmers
who have suffered from this not-really-the-same-language problem. Besides the
module system, there are other not-quite-the-same-but-related features that
implementations have and programmers wish to use.

The R6RS process has pushed several major implementors/implementations to agree
on a design for module systems and other constructs. Their report declares that
they are ready to put a large amount of work in to get from r5rs to r6rs. I
believe that this step would help the community in several arenas, listed in
increasing order of relevance:

1. the academic publishing business
2. the fund raising business
3. adapting each others innovations
4. supporting programmers who learn on one and switch to another implementation
5. supporting commercial programmers who need reassurance that there is more than one implementation and implementor [ever attended Commercial Uses of Functional Programming?] 

Is the document perfect? Is every construct exactly the 'right thing'? Of
course not! Guy and Gerry revised their first Scheme report because they didn't
get it 'right'. R3RS and R4RS and R5RS revised flaws in R(n-1)RS because the
authors/editors didn't get it 'right'. It is extremely difficult, and usually
impossible, to get the design of a complex artifacts (such as a programming
language) 'right' the first time. In these cases, it's all about the feedback
loop and revising your design based on observations. (Remember the 'science'
part in the name of our discipline?) Indeed, 'right' doesn't exist; what exists
is 'most pragmatic and internally beautiful,' and nothing else.

Our choice is quite simple: move forward as a community with some amount of
convergence (r6rs) or split into dozens of mutually incompatible
sub-communities (status quo, including SRFIs).

*Also posted as ["R6RS is perfect"](http://lists.r6rs.org/pipermail/r6rs-discuss/2007-June/002538.html) at the R6RS discussion list.*
