    Title: Experience Report: Scheme in Commercial Web Application Development
    Date: 2007-08-03T05:35:00
    Tags: Noel Welsh

Our paper [“Experience Report: Scheme in Commercial Web Application Development”](http://dl.acm.org/citation.cfm?id=1291175)
is online. As the title suggests, it describes our experiences over the past
year developing a number of web-based applications in PLT Scheme. If we'd
chosen a language like Java or Ruby we could have used a large number of
libraries developed for web apps, whereas PLT Scheme has relatively few
libraries in this area, and they haven't been tested under high load. So we
were gambling that Scheme would make us so productive we could develop our own
libraries and the applications we were contracted to produce in the same time
it would take to develop just the applications in another language. It was a
gamble that paid off. You'll have to read the paper for all the details, but
suffice to say we delivered the applications on time (and more are in
development) and our libraries already compare well against big names like Ruby
on Rails and J2EE.

On thing that got cut from the paper was our use of [Flapjax](http://www.flapjax-lang.org/) is parts of the
interface. If you write complicated Javascript to take a look at it. It really
does simplify event handling, and our code using Flapjax is half the size of
our original code without it.

**Update:** This is more or less the same post as on [Untyping](http://untyped.com/untyping/archives/2007/08/a_scheme_case_s_1.html)
