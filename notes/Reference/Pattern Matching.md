---
tags:
  - fundamentals
  - syntax
aliases:
  - where
---

##### Synopsis
- Madlib has powerful pattern matching using the `where` keyword to generalize across and deconstruct Data Types to access their encapsulated values.
## Explicit deconstruction

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

## Implicit deconstruction

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

## Wildcards

We can use the wildcard (`_`) in pattern matching to declare a variable that we don't want to do anything with.

For instance, the `Maybe` type looks like this:

```mad
type Maybe a = Just(a) | Nothing
```

If we want to pattern match against it, but we don't actually care about the value it represents, we can use the wildcard to tell the compiler that "a variable goes here, but I don't care about it":

```mad
isNothing = where {
  Nothing => true
  _ => false
}
```

Note the way in which we've skipped even declaring the `Just` constructor here. Used like this, the wildcard means "in all other cases".

We could easily also do the opposite case:

```mad
isJust = where {
  Just(_) => true
  _ => false
}
```

Note the way in which we've used the wildcard twice. The first time it represents the `a` value of our `Just` constructor. The second time it represents `Nothing`, but we're eschewing even declaring that constructor; "in all other cases: false".

Here's one more example with a custom type, for completeness:

```mad
type Mood = Happy | Sad | Indifferent

isHappy = where {
  Happy => true
  _ => false
}
```
