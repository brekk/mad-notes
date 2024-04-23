---
aliases:
  - Tuple
tags:
  - fundamentals
  - literals
---
A specific length enumeration of possibly mixed types, delimited by square brackets beginning with a hash, e.g. `#["two", 2]`.

## Comparison

Tuples of different lengths or constituent types are considered separate data types: `#[1, 'a', true]` is a different type than `#[1, "a", true]` and that is also different from `#[1, "a"]` — so some operations cannot be generalized across these different type boundaries:

```mad
firstOfTuple :: #[a, Char] -> a
```

## Deconstruction

Tuples can