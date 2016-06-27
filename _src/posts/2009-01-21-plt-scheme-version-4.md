    Title: PLT Scheme v4.1.4
    Date: 2009-01-21T02:20:00
    Tags: release

PLT Scheme version 4.1.4 is now available from

  [http://plt-scheme.org/](http://plt-scheme.org/)

* New libraries include `scheme/package' (for nestable static modules) and `ffi/objc' (support for Objective-C).
* New teaching support includes a "universe.ss" teachpack for connecting "worlds" over a network.
* Redex now supports automatic test-case generation. Specify a predicate that should hold of your reduction system, and Redex will attempt to falsify it. See 'redex-check' in the manual for more details.
* Improvements to the run-time system include better and more reliable memory-limit tracking, function contracts that preserve tail recursion in many cases, native debugging backtraces on x86_64, and performance improvements.
* Improved libraries include enhancements to `scheme/sandbox', better handling of zero-sized matches by `regexp-split' and friends, an `equal<%>' interface for specifying equality on class instances (and more general support for attaching properties to interfaces), equality (via `equal<%>') for image objects, and refinements to `scheme/foreign' to support atomic operations and function-pointer conversions.

[Note that mirror sites can take a while to catch up with the new downloads.] Feedback Welcome.
