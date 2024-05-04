---
aliases: []
tags:
  - fundamentals
---
##### Synopsis
- Functions are one of the core abstractions used by Madlib. They have a type signature frames the types that a function takes as parameters and the type which it returns. The compiler can then help developers write code correctly.

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

Functions may use [[Do Notation|do notation]] to unwrap monadic computation or in [[Conditionals|conditionals]] to allow for multiple expressions 

## Curry 

Currying refers to the operation of translating a function that takes multiple arguments into a sequence of functions, each taking a single argument. Automatically curried functions, when passed fewer than the arguments they expect, yield a function that expects the remaining parameters. All functions in Madlib are automatically curried. This allows for generic functions which afford partial application and named morphisms of concrete functions.

### Example

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

This can also be used without naming the morphism and keeping it inline (here we're using [Functor](app://obsidian.md/Functor) to apply the partially applied `greet` to a List of Strings):

```mad
pipe(
  map(greet("hello")),
)(["world", "developer", "there"]) // ["hello world", "hello developer", "hello there"]
```

### Function Application Operator

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

This illustrates the way in which, especially in other languages, we need to be cognizant of the order of parameters. However, in Madlib we can take advantage of the function application operator to make the partially applied version of `div` much more useful _without_ re-writing `div`.

```mad
half = div($, 2)
```

This syntactic construct allows us to placehold the first parameter — this new function `half` has the denominator 2 partially applied, and it still expects the numerator:

```mad
half(100) // 50
```

The function application operator can be thought of as transforming the original function like so (but this all happens magically at compilation time):

```mad
// half = div($, 2)
half = (n) => div(n, 2)
```

The function application operator can be used multiple times, but if it is used to placehold the final parameter it does nothing / is needless.

## Pipe Composition

Pipe composition is a fundamental tool for combining unary functions. Instead of writing a function which takes the result of another function as its input ( `f(g(x))` ) we can instead compose functions together left-to-right with pipe: `pipe(g, f)(x)`. This aids algebraic logic, empowers equational reasoning and allows for succinct and expressive code. This feature, combined with automatic [[Functions#Curry|curry]], allows for tacit (sometimes called point-free) programs. Note: `pipe` does not need to be imported, it is a syntactic keyword.


### Usage

Here's a simple example using only unary functions (functions which take a single parameter):

```mad
import IO from "IO"

main = () => {
  pipe(
    (x) => x ++ x,
    (x) => "b" ++ x,
    (x) => x ++ "a",
    IO.put
  )("an")
}
```

Here's the same thing but written with the `mconcat` function which combines two Strings together (`String -> String -> String`), and leverages the [[Functions#Function Application Operator|function application operator]] (`$`):

```
import IO from "IO"

main = () => {
  pipe(
    (x) => x ++ x,
    mconcat("b"),
    mconcat($, "a")
    IO.put
  )("an")
}
```


### Pipeline operator

We support the pipeline operator (`|>`) in limited circumstances. It is useful for composing one or two operations in sequence, but it is not supported in all places (and we are considering phasing it out, so it may become deprecated):

```mad
import IO from "IO"
main = () => {
  "hello " ++ "world" |> IO.put
  // this is the same as
  IO.put("hello " ++ "world")
}