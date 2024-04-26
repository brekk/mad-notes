---
aliases:
  - Pipe
  - Pipe Composition
tags:
  - fundamentals
  - syntax
---
##### Synopsis
- Pipe composition is a fundamental tool for combining unary functions. Instead of writing a function which takes the result of another function as its input ( `f(g(x))` ) we can instead compose functions together left-to-right with pipe: `pipe(g, f)(x)`. This aids algebraic logic, empowers equational reasoning and allows for succinct and expressive code. This feature, combined with automatic [[Functions - Curry|curry]], allows for tacit (sometimes called point-free) programs.


## Usage

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

Here's the same thing but written with the `mconcat` function which combines two Strings together (`String -> String -> String`), and leverages the [[Functions - Curry#Function Application Operator|function application operator]] (`$`):

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


## Pipeline operator

We support the pipeline operator (`|>`) in limited circumstances. It is useful for composing one or two operations in sequence, but it is not supported in all places (and we are considering phasing it out, so it may become deprecated):

```mad
import IO from "IO"
main = () => {
  "hello " ++ "world" |> IO.put
  // this is the same as
  IO.put("hello " ++ "world")
}
```