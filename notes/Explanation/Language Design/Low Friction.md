---
tags:
  - language-design
---
In designing Madlib we've tried to [[Familiarity|cleave to the familiar]], but we also want to make adopting the Madlib paradigms both [[Explicitness|explicit]] and involve as little friction / convenience as possible.

This affects both the general approach of the language as well as attempting to eschew behavior that can result in [bike-shedding](https://en.wikipedia.org/wiki/Law_of_triviality) — for instance, the [[madlib format|auto-formatter]] will ensure that imported statements and records have a consistent sorting.

Much of the language is designed to feel reasonably similar to JavaScript syntax, with a bit of Haskell thrown in.

To that end we have a few syntactic shorthands which are aimed at making interacting with the type system compelling and powerful. One such feature is the [[Record]], which allows for accessing the fields of [[Record|Records]] as simple as writing the field with a period before it.
## JavaScript
- [[The Fence]] allows for a JS escape hatch at the cost of automatic inference
## LLVM
- [[Externally linked libraries]] / the `extern` keyword allows for a LLVM escape hatch