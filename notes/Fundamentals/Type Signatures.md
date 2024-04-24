---
aliases: []
tags:
  - fundamentals
---
##### Synopsis
- A type signature is indicated by a double-colon `::` , which can be read as "has type of". It is used by the Madlib compiler to provide logical correctness when writing applications.

This can be done inline, e.g. <code>five = (5 :: Byte)</code> or prior to a declaration, especially [[Functions|a function]]:
```
times :: Integer -> Integer -> Integer
export times = (a, b) => a * b 
```

## Reading a Function Type Signature

Function type signatures are written independently of their implementation.
For example, `nth :: Integer -> List a -> Maybe a` is a function type signature which indicates that there is a function, called "nth", which takes two parameters, one of type `Integer` and another parameterized list of any type `List a`. This will return that same type `a` wrapped in a `Maybe`. Note that `a` here can be any lowercase variable, we tend to use the beginning of the alphabet by default but this is not a hard requirement 

If we expand this to the full definition of `nth` (taken from [[Data Types#Prelude|Prelude]], [here](https://github.com/madlib-lang/madlib/blob/7b9f98c09c70a03a036b900bc7f08e9eb2302f12/prelude/__internal__/List.mad#L372-L387)) we see both the type signature and the function implementation:
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
Functions can be implemented so that they fulfill a known [[Interfaces|Interface]]. This allows for consistent and generic behavior across multiple implementations. 

For example, here’s a function which works with Integers:

```mad
square :: Integer -> Integer
square = (x) => x * x
```

if you want to have a function which works with _any_ kind of [[Numeric Types|Number]], you’d want to change the signature to something generic, like `square :: a -> a`  — however, the compiler will be unhappy with this because it will try to make these generic types concrete upon usage, so it would work if you pass in a Float for the `a` value, but you couldn’t then use the same function with Integers or another numeric type.

However, we can solve this by adding the Number constraint:

```mad
square :: Number a => a -> a
square = (x) => x * x
```

Note the location of the fat arrow ( <code>=></code> ) in the signature, this indicates that the `a` value here is constrained / pulled from the Number interface.