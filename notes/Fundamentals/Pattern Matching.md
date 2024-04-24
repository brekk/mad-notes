---
tags:
  - fundamentals
  - syntax
aliases:
  - where
---

##### Synopsis
- Madlib has powerful pattern matching using the `where` keyword to generalize across and deconstruct Data Types to access their encapsulated values.
### Explicit deconstruction
Say we have an [[Data Types#Either|Either]] value which we want to access the internal value of:
```
import { Right } from "Either"

x = Right("Brekk")
```
We can explicitly deconstruct this value (to access the string `"Brekk"` within it) with `where (x) { [...] }`:
```
import { Right, Left } from "Either"

x = Right("Brekk")

str = where (x) {
  Right(value) => "Name: " ++ value
  Left(error) => "Error: " ++ error
}
```
(For the purposes of this example we're assuming that this `x` value is of type `Either String String`. Note that if we omitted the `Left` deconstruction above, we would get an [[Warnings|incomplete pattern warning]])

### Implicit deconstruction
For convenience and composition, a `where` deconstruction without an explicit parameter is treated like a unary function which expects one parameter. Taking the example from above, the following code results in the same behavior:
```
import { Right, Left } from "Either"

x = Right("Brekk")

getString :: Either String String -> String
getString = where {
  Right(value) => "Name: " ++ value
  Left(error) => "Error: " ++ error
}

str = getString(x)
```