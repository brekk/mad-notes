---
aliases: 
tags:
  - literals
---
##### Synopsis
- A tuple is an enumerated literal of specific length (two elements or more) containing possibly mixed types, delimited by square brackets beginning with a hash, e.g. `#["two", 2]`.

A tuple must have at least two types in it; an "empty" tuple or a tuple with a single type is logically and syntactically invalid.

## Comparison

Tuples of different lengths or constituent types are considered separate data types: `#[1, 'a', true]` is a different type than `#[1, "a", true]` and that is also different from `#[1, "a"]` — so some operations cannot be generalized across these different type boundaries. For instance, `Tuple.fst` grabs the first value of any 2-tuple, but it cannot operate on a 3-tuple.

## Deconstruction

Tuples can be deconstructed [[Pattern Matching|where]] syntax.