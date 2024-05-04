---
aliases: 
tags:
  - reference
  - prelude
  - data-type
---
##### Synopsis
- Madlib has a specialized Array type which is designed for performance, but at the cost of safety. It eschews several of the considerations that have been built into Lists, chiefly [[Limit Mutation|in-place mutation]] and [[Make Invalid States Unrepresentable|unsafe access]].

Madlib uses [[List|Lists]] as a collection type in most circumstances.

## Construction

Sets are most easily constructed using `Array.fromList`:

```mad
import Array from "Array"

a = Array.fromList(["a", "b", "c"])
```

## In-Place Mutation

Arrays in Madlib are unusual in that they have operations which mutate them in place, unlike [[Coming From JavaScript#Copies over Mutation|most functions in Madlib]].

```mad
import Array from "Array"
import IO from "IO"

alpha = Array.fromList(["a", "b", "c"])
Array.push("z", a)
IO.pTrace("mutant!", alpha)
```

## Unsafe Access

Additionally, unlike Lists, Arrays allow for unsafe index access:

```mad
alpha = Array.fromList(["a", "b", "c"])
IO.pTrace("...", alpha[0]) // ... a
```

However, there is **no safety net**. If you try to access an invalid index, your program will fail at runtime.