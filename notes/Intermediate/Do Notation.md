---
aliases: 
tags:
  - syntax
  - keyword
---
##### Synopsis
- The `do` keyword allows for some specific semantics: it allows for the usage of the [[Do Notation#Single Assignment operator|single assignment operator]] and it allows for multiple expressions in a scope which otherwise expects a single expression, such as [[Conditionals|if ... then]] , [[while]] and [[Pattern Matching|pattern matching using where]].

### Single Assignment operator

The single assignment operator (`<-`) affords local assignment of internal monadic values. It is similar to the assignment operator ( <code>=</code> ), but exclusively available when in a `do` notation context.

Consider the following:

```mad
import IO from "IO"
import { Just, Nothing, fromMaybe } from "Maybe"



a = (m) => {
  IO.trace("a", m)
  return m
}

a2 = IO.trace("a2")

b = (m) => do {
  n <- m
  IO.trace("b", n)
  return Just(n)
}

b2 = map(IO.trace("b2"))

c = pipe(
  fromMaybe("???"),
  IO.trace("c"),
)

main = () => {
  x = Just("x")
  a(x)       // a Just("x")
  a2(x)      // a2 Just("x")
  b(x)       // b "x"
  b2(x)      // b2 "x"
  c(x)       // c "x"
  c(Nothing) // c "???" 
}
```

Note that `a` and `a2` have the same functionality. This is also true of `b` and `b2`. However, `c` is actually pulling the interior value of the Just wrapper out, as evidenced by the `c(Nothing)` outcome.

## Multiple expressions

The `do` keyword can also be used in some specific constructs to afford multiple expressions where normally only a single expression is allowed.

### if ... else

Consider the following code:

```mad
tier :: Integer -> String
tier = (x) => if (x < 5) {
  "a"
} else if (x >= 5 && x < 10) {
  "b"
} else {
  "c"
}
```

As discussed in [[Conditionals]], this syntactic construct is identical to a ternary expression, so it does not allow for multiple expressions and it is not allowed to short-circuit.

However, using `do` here we can invoke more than a single expression, but we must add the `return` statement explicitly:

```mad
tier :: Integer -> String
tier = (x) => if (x < 5) {
  "a"
} else if (x >= 5 && x < 10) do {
  IO.trace("the b case", x)
  return "b"
} else {
  "c"
}
```

### while

Similarly, `while` has a single-expression limitation. Consider the following code, which makes use of the [[Limit Mutation#Explicit Mutation|explicit mutation operator]] (`:=`) to make changes to values in the outer function's scope with a closured inner function:

```mad
whileDont :: Integer -> List Integer
whileDont = (x) => {
  i = 0
  stack = []
  invoke = (j) => {
    stack := [...stack, j]
    i := j + 1
  }
  while(i < x) {
    invoke(i)
  }
  return stack
}
```

Here's the same code using `do`, which allows for a slightly more convenient (and terse) implementation:

```mad
whileDo :: Integer -> List Integer
whileDo = (x) => {
  i = 0
  stack = []
  while(i < x) do {
    stack := [...stack, i]
    i := i + 1
  }
  return stack
}
```

### where

Normally [[Pattern Matching|pattern matching with `where`]] only allows for a single expression. Consider this function which returns a string based on a Maybe-wrapped Integer:

```mad
import type { Maybe } from "Maybe"
import { Just, Nothing } from "Maybe"

whereDont :: Maybe Integer -> String
whereDont = where {
  Just(_) =>
    `This number is valid!`

  Nothing =>
    `This number is invalid!`
}
```

The above function can only have a single per expression per pattern matched. However, if we use `do` we can augment this function to add some interior logging:

```mad
import type { Maybe } from "Maybe"
import { Just, Nothing } from "Maybe"

whereDo :: Maybe Integer -> String
whereDo = where {
  Just(x) =>
    do {
      IO.trace("The valid number is", x)
      return `This number is valid!`
    }

  Nothing =>
    `This number is invalid!`
}

```