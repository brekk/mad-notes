---
aliases:
  - Functions
tags:
  - fundamentals
---
##### Synopsis
- Functions are one of the core abstractions used by Madlib. They have a type signature which helps frame the types that a function takes as parameters and the type which it returns. The compiler can then help developers write code correctly.

Examples:
```
add :: Integer -> Integer -> Integer
add = (x, y) => x + y

nth :: Integer -> List a -> Maybe a
export nth = (i, list) => where(list) {
  [] =>
    Nothing

  [a, ...xs] =>
    i == 0 ? Just(a) : nth(i - 1, xs)
}
```

## Type Signature

Functions _should_ be preceded with a Hindley-Milner style [[Type Signatures|type signature]]. This isn't _strictly_ required, since the compiler can usually infer correct types without this, but using type signatures will improve the quality and correctness of your code. This is especially true of code which makes use of [[The Fence|The Fence]] or [[Externally linked libraries|externally linked libraries]], or code which makes use of [[Interfaces#Type Constraints|type constraints]], either in order to be generic or take advantage of an interface.

## Consistent Return Type

Functions _must_ return a consistent type, e.g. unlike [[Coming From JavaScript|in JavaScript]], you cannot have a function which returns an integer _or_ a string. If you want to allow for a function to return "different" types, you'll need to wrap that value with an [[Algebraic Data Types|algebraic container type]]. For instance, if you want to model something that could otherwise result in failure, you could use the [[Data Types#Maybe|Maybe]] type to encapsulate and elide over that — using `Nothing` to model failure and `Just` to model success.

## Construction

Functions can be a lambda-style expression without braces and an implicit `return`  (e.g. `(x) => x`), or use braces build up logic over several expressions and need an explicit `return` keyword to produce a value. A function which uses braces and has no explicit `return` will be inferred as nullary, returning [[Types#Unit|Unit]]. (Definitionally, a nullary function is therefore either side-effectful or needless.)

## `do` Notation

Functions may use `do` notation to unwrap monadic computation.
## Composition

Functions may be composed using the `pipe` keyword, which takes a variadic number of functions as parameters, and returns a unary function.

Example:
```
timesFiveMinusTwo = pipe(
  (x) => x * 5,
  (x) => x - 2,
)

timesFiveMinusTwo(10) // 48
```

## Curried

In Madlib, functions with an arity greater than one are curried, so if a function is given fewer parameters than it needs to produce a value, it will instead return a function which expects the remaining parameters.

Example:
```
wave :: String -> String -> String
wave = (greeting, name) => greeting ++ name

hi = wave("Hello, ")

pipe(
  map(hi),
  IO.putLine
)(["Brekk", "Arnaud"]) // ["Hello, Brekk", "Hello, Arnaud"]
```