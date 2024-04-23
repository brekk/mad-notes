---
aliases:
  - Type Signatures
tags:
  - fundamentals
---
A type signature is indicated by a double-colon `::` , which can be read as "has type of".

This can be done inline, e.g. `five = (5 :: Byte)` or prior to a declaration, especially [[Core - Functions|a function]]:
```
times :: Integer -> Integer -> Integer
export times = (a, b) => a * b 
```

## Reading a Function Type Signature

Function type signatures are written independently of their implementation.
For example, `nth :: Integer -> List a -> Maybe a` is a function type signature which indicates that there is a function, called "nth", which takes two parameters, one of type `Integer` and another parameterized list of any type `List a`. This will return that same type `a` wrapped in a `Maybe`.

If we expand this to the full definition of `nth` (taken from [[Core - Data Types#Prelude|Prelude]], [here](https://github.com/madlib-lang/madlib/blob/7b9f98c09c70a03a036b900bc7f08e9eb2302f12/prelude/__internal__/List.mad#L372-L387)) we see both the type signature and the function implementation:
```
nth :: Integer -> List a -> Maybe a
export nth = (i, list) => where(list) {
  [] =>
    Nothing

  [a, ...xs] =>
    i == 0 ? Just(a) : nth(i - 1, xs)
}
```

## Type Constraints
In order to make a function work generic

