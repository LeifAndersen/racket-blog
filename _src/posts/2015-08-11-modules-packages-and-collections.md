    Title: Modules, Packages and Collections
    Date: 2015-08-11T14:33:00
    Authors: Vincent St-Amour
    Tags:

Racket, the Racket docs and Racketeers use a number of terms to refer
to various units of Racket code. Of those, *module*, *package* and
*collection* refer to related but distinct concepts. Their exact
relations and distinctions can be confusing for new users. This is an
attempt at explaining those concepts, what they are for, and how they
relate to each other.

To begin with the smallest of the three, a file that begins with
`#lang` and the name of a language is a module. There are also other
ways to construct modules, but let's not worry about those.

A module is the basic unit of functionality for Racket code.

Once your Racket programs get larger, though, you'll want to split them over multiple modules. This allows you to organize your source better, enables separate compilation, and makes it possible for you to mix and match modules written in different Racket languages ([Racket](http://docs.racket-lang.org/guide/index.html), [Typed Racket](http://docs.racket-lang.org/ts-guide/index.html), [Datalog](http://docs.racket-lang.org/datalog/index.html), [Scribble](http://docs.racket-lang.org/scribble/index.html), etc.).

That's where packages and collections come in. They help you organize your modules.

A *package* is an group of modules that you can install together, and
that usually provide one piece of functionality. To pick a random
example, take the
[pict3d](http://docs.racket-lang.org/pict3d/index.html) package from
[pkgs.racket-lang.org](http:°pkgs.racket-lang.org). That package is a
collection of modules which together implement a functional 3D
engine. You can install it using `raco pkg install pict3d`, or via the
graphical package manager in DrRacket.

So, to sum up, packages are units of code distribution.

A *collection* is a group of modules whose functionality is related to
the same topic, for example data structures (the `data` collection), or
wrapper libraries for use with Typed Racket (the `typed`
collection). Modules are referred to and required using collection
paths. For example, when you require `racket/class`, you're requiring
the `class` module from the `racket` collection.

Modules within a collection do not necessarily come from the same
package, and may not be developed together. For example, some data
structures in the data collection are provided as part of the core of
Racket, such as the integer sets in
[data/integer-set](http://docs.racket-lang.org/data/integer-set.html). Other
data structures are provided by additional packages which you may need
to install separately, such as the hash-array-mapped tries in
`data/hamt`, which are provided by the
[hamt](http://docs.racket-lang.org/hamt/index.html) package. Having
both of those in the data collection signals that they both provide
data structures. If you develop your own data structures, putting them
in the data collection is probably the right thing to do.

Many packages, however, provide functionality that does not fall under
existing categories, and provide their own, new collection. For
example, the `pict3d` package we discussed above puts its modules in the
`pict3d` collection. For that reason, the distinction between package
and collection is sometimes a bit blurred.

So, to sum up, collections are units of code classification.

The term *library* does not have a technical meaning in Racket. We
usually use it to refer to a package, or to a set of packages that are
developed together. For example, the
[Rackunit](http://docs.racket-lang.org/rackunit/index.html) library is
split across multiple packages: `rackunit`, `rackunit-lib`,
`rackunit-gui`, `rackunit-plugin-lib`, `rackunit-doc` and
`rackunit-test`. This allows packages to only depend on part of
Rackunit. For example, a package for a string-processing library
probably should not depend on the
[Racket GUI](http://docs.racket-lang.org/gui/index.html) library (to
be deployed on headless servers, for example), and so should depend on
the `rackunit-lib` package for its testing, instead of on the full
`rackunit` package, which brings in GUI support via the `rackunit-gui`
package, and would introduce a dependency to Racket's GUI library.

Hopefully, this clarifies the Racket code organization terminology a bit.
