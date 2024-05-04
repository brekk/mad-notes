---
aliases: 
tags:
  - reference
  - prelude
  - literals
---
##### Synopsis
- A List is a literal collection of the same type, delimited by square brackets.

```mad
listOfStrings = ["hey", "cool", "nice"]
listOfIntegers = [1,2,3,4,5,6]
emptyList = []
```

Though Madlib has an [[Array|Array type]], unless you have a specific need for performance, in all likelihood you ought to be working with Lists.
## Safe access

In order to [[Make Invalid States Unrepresentable|avoid illegal states]], some common List operations yield values wrapped in a [[Maybe|Maybe]]:
```mad
import List from "List"
list = ["a", "b", "c"]
List.nth(1, list) // Just("b")
List.nth(7, list) // Nothing
```