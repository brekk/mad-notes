---
aliases:
  - Conditionals
tags:
  - logic
  - fundamentals
---
##### Synopsis
- Conditionals in Madlib are designed to be logically equivalent to ternary expressions. They are single-expression by default, they have implicit returns, and they must be complete (they cannot short-circuit).

## Conditionals with `if` ... `else`

Here's an example of conditionals using `if` ... `else` syntax:

```mad
tier :: Integer -> String
tier = (x) => {
  return if (x < 5) {
    "a"
  } else if (x >= 5 && x < 10) {
    if (x == 7 || x == 9) {
      "b1"
    } else {
      "b"
    }
  } else {
    "c"
  }
}
```

Note the location of the `return`, and the way in which each branch is a single expression.

Additionally, if we modify our logic to have an incomplete logical case, Madlib will yell at us:

```mad
tier :: Integer -> String
tier = (x) => {
  return if (x < 5) {
    "a"
  } else if (x >= 5 && x < 10) {
    if (x == 7 || x == 9) {
      "b1"
    } else {
      "b"
    }
  } else if (x < 15) {
    "c"
  }
}
```

There is a logical case (where `x` is greater than or equal to 15) which we haven't accounted for. As this would implicitly return [[Types#Unit|Unit]], this results in an error.

### Conditionals with `do`

The `do` keyword can be used to allow for multiple expressions / effects in locations which are otherwise expecting single expressions.

To re-use the example from above, if we wanted to add a log in the logical case between 5 and 10, we must use a `do` statement and add an explicit `return`:

```mad
return if (x < 5) {
  "a"
} else if (x >= 5 && x < 10) do {
  IO.pTrace("b cases!", x)
  return if (x == 7 || x == 9) {
    "b1"
  } else {
    "b"
  }
} else if (x < 15) {
  "c"
}
```

Without the `do`, this is a grammar error because it's not a single expression. Without the `return` in the body of the `do`, that logical branch executes but returns `{}` / [[Types#Unit|Unit]], so it is an [[Coming From JavaScript#Function return types|inconsistent return type]].
## Conditionals with Ternary Expressions

The same code as a ternary expression would be:

```mad
tier :: Integer -> String
tier = (x) => (
  x < 5
  ? "a"
  : (
    x >= 5 && x < 10
    ? ( x == 7 || x == 9
      ? "b1"
      : "b" )
    : "c"
  )
)
```

