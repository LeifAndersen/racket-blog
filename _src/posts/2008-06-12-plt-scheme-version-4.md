    Title: PLT Scheme Version 4.0
    Date: 2008-06-12T02:11:00
    Tags: release

PLT Scheme version 4.0 is now available from:

  [http://plt-scheme.org/](http://plt-scheme.org/)

This major new release offers many improvements over version 372, and we
encourage everyone to upgrade.

* The PLT Scheme language now provides better syntax for modules, better
  support for optional and keyword arguments to functions, a more complete syntax
  for structure types, new syntax for list comprehensions and iterations, a more
  complete and consistent set of list operations, a more complete set of string
  operations, and streamlined hash-table operations.

* The documentation has been re-organized and re-written. New tutorials and
  overviews offer a clearer introduction to Scheme and PLT Scheme.

* New documentation tools help programmers create and install documentation
  for libraries and Planet packages. All installed documentation can be read
  though the user's web browser, and even searching within the browser works on
  local files.

* The language for writing documentation is an extension of Scheme, and
  document sources are linked to implementations through the module system. The
  module connection allows, for example, reliable automatic hyperlinking of
  identifiers mentioned in documentation to their specifications in other
  documentation.

* R6RS programs are supported in two ways: though the plt-r6rs executable and
  through the #!r6rs prefix. The latter allows an R6RS library or program to
  serve as a PLT Scheme module.

* Legacy R5RS support is improved, partly through a separate plt-r5rs
  executable.

* Pairs are immutable within the PLT Scheme language; mutable pairs (which
  are the same as R6RS and R5RS pairs) are provided as a separate datatype. For
  more information, see “[Getting rid of `set-car!` and `set-cdr!`](http://blog.racket-lang.org/2007/11/getting-rid-of-set-car-and-set-cdr.html)”

* ProfessorJ uses a new and improved parser, it evaluates programs faster,
  and it includes a Java-specific indenter.

* Testing frameworks for the HtDP and HtDC (ProfessorJ) teaching languages
  have been unified. Both support systematic unit testing in a comprehensive
  fashion. When programs lack tests, students are asked to add test cases. When
  all tests succeed, a simple message says so; otherwise, a pop-up window
  (dockable) displays URLs to the failed test cases and explains why the cases
  failed.

* Typed Scheme, a statically typed dialect of Scheme, is now included with
  PLT Scheme. While Typed Scheme is still in its early stages of development, it
  supports modular programming with types and full interaction with existing
  untyped code. Safe interactions between typed and untyped modules are enforced
  via contracts. Typed Scheme also features a novel type system designed to
  accommodate Scheme programming idioms. For more information, see the
  [Typed Scheme](http://docs.racket-lang.org/ts-guide/index.html) page.

Feedback Welcome.
