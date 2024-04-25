---
tags:
  - prelude
  - reference
---
##### Synopsis
- The Set data type provides a means of storing unique values using a binary tree (specifically, a [Red-black tree](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree))

## Construction

Sets are most easily constructed using `Set.fromList`; it applies uniqueness to the given List. The underlying constructors are not exported (in order to avoid footguns and make them simpler to use), but you can use `Set.empty` and `Set.singleton` to create Sets, with zero or one element, respectively.

```mad
import Set from "Set"

s = Set.fromList([1,3,2,4,5,2,3,5,2,2,1,2,3,4]) // Set([1,2,3,4,5])
s2 = Set.singleton(4) // Set([4])
e = Set.empty // Set([])
```

*NB*: `Set.singleton` is used to add a single element to a Set! This is a potential footgun if you're not careful: `Set.fromList` will construct a Set whose elements are taken from a list. `Set.singleton` will construct a set of one element — if you pass `Set.singleton` an array, it will create a Set with one element which _is_ an array. Subsequent operations will then expect elements in the Set to also be arrays:

```mad
s3 = Set.singleton([1,2,3]) // Set([[1,2,3]]) - note the two sets of square brackets
s3 |> Set.insert(4) |> IO.pTrace("invalid!")
```