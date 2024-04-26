---
aliases:
  - autocurry
  - Curry
tags:
  - "#fundamentals"
  - partial-application
---
##### Synopsis
- Currying refers to the operation of translating a function that takes multiple arguments into a sequence of functions, each taking a single argument. Automatically curried functions, when passed fewer than the arguments they expect, yield a function that expects the remaining parameters. All functions in Madlib are automatically curried. This allows for generic functions which afford partial application and named morphisms of concrete functions.

## Example

Consider this simple function which takes two strings and returns them concatenated together with a space:

```mad
greet :: String -> String -> String
greet = (salutation, subject) => salutation ++ " " ++ subject
```

This function is made more reusable by the fact that it is curried, and thus it can have a named morphism that is a partially applied version of the original:

```mad
hello = greet("hello")
hello("world") // "hello world"
hello("developer") // "hello developer"
hello("there") // "hello there"
```

This can also be used without naming the morphism and keeping it inline (here we're using [[map]] to apply the partially applied `greet` to a List of Strings):
```mad
pipe(
  map(greet("hello")),
)(["world", "developer", "there"]) // ["hello world", "hello developer", "hello there"]
```

## Function Application Operator

The function application operator (`$`) can be thought of as a placeholder value which is valuable when partially applying curried functions:

Consider the following function a binary function for division:

```mad
div :: Float -> Float -> Float
div = (n, d) => n / d
```

If we were to use this in a partially applied form, by default we'd be partially applying the first parameter, the numerator in the division:

```mad
div(1, 3) // 1 / 3
div(4, 2) // 2

oneOver = div(1)
```

This illustrates the way in which, especially in other languages, we need to be cognizant of the order of parameters. However, in Madlib we can take advantage of the function application operator to make the partially applied version of `div` much more useful _without_ re-writing `div`.

```mad
half = div($, 2)
```

This syntactic construct allows us to placehold the first parameter — this new function `half` has the denominator 2 partially applied, and it still expects the numerator:

```mad
half(100) // 50
```

The function application operator can be thought of as transforming the original function like so (but this all happens magically at compilation time):

```mad
// half = div($, 2)
half = (n) => div(n, 2)
```

The function application operator can be used multiple times, but if it is used to placehold the final parameter it does nothing / is needless.