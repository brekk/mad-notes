---
aliases:
  - Data Types
tags:
  - fundamentals
  - prelude
---
##### Synopsis
- The built-in data types that Madlib ships with (also called Prelude) will give you powerful tools to manipulate and encapsulate logic within applications.
## Prelude

Our standard library (the functionality available without a third-party library) is called **Prelude**, much like Haskell's. These documents may refer to the standard library and Prelude interchangeably.

## Usage

To make use of the core data types, there are two kinds of [[Imports|syntactic imports]], `import type` and concrete `import`s.

To use a data type in a function signature, you must import its type:

```
import type { Maybe } from "Maybe"
import { Just, Nothing } from "Maybe"

first :: List a -> Maybe a
export first = (list) => where (list) {
  [] => Nothing
  [x, ..._] => Just(x)
}
```

In order to make use of a type constructor, you must import its constructor:

```
import { Just } from "Maybe"
x = Just(5)
```

or reference it via a named import:

```
import Maybe from "Maybe"
x = Maybe.Just(5)
```

## Construction
* [[Maybe|Maybe]]
* [[Either|Either]]
* [[Wish|Wish]]
* Set
* Parser
* Stream?
* Comparison
* 
## Deconstruction
Often you will want to access the interior values encapsulated with a given constructor. This can easily be done with the `where` keyword.
This can be done explicitly or implicitly.
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