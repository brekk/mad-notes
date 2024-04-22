---
aliases:
  - Coming From JavaScript
---
For developers coming to Madlib from JavaScript, there are a few notable differences one should be aware of.

## Variable declaration

There is no specific keyword needed to declare a variable, like `let` or `const` or `var` in JS. Instead all values are declared with the assignment operator, e.g. `x = 5` . However, values cannot be mutated, [[Language Design - Limit Mutation|except in limited circumstances]].

## Strict Equality

Madlib has a single equality operator `==` and the [[Language Design - Explicitness|language itself is strict]], instead of forcing the developer to remember to use a different operator of increased strictness — `===` is not a valid operator and will throw a syntax error.
## Conditionals

Conditionals in Madlib are single-expression by default, they have implicit returns, and they must be complete (they cannot short-circuit).

Consider the following JS code:
```js
if (x < 5) {
  return "a"
} else if (x >= 5 && x < 10) {
  if (x == 7 || x == 9) return "b1"
  return "b"
} else {
  return "c"
}
```

In Madlib logical conditions are an expression, exactly like ternary expressions in JS. So multiple returns here would be invalid (unless using [[Syntax - Do Notation|do notation]], which we will discuss more [[Migration - Coming From JavaScript#Conditional `do`|below]]).

To reproduce the above code in Madlib:

```mad
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
```

Note the location of the `return`, and the way in which things have been rewritten to be expressions. The same code as a ternary expression would be:

```mad
return (
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

The above code is also identical (1:1) with JS code.

Additionally, if we modify our logic to have an incomplete logical case, Madlib will yell at us:

```mad
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
```

There is a logical case (where `x` is greater than or equal to 15) which we haven't accounted for. As this would implicitly return [[Core - Types#Unit|Unit]], this results in an error.

### Conditional `do`

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
Without the `do`, this is a grammar error because it's not a single expression. Without the `return` in the body of the `do`, that logical branch executes but returns `{}` / [[Core - Types#Unit|Unit]], so it is an [[Migration - Coming From JavaScript#Function return types|inconsistent return type]].
## Array / List access

Though Madlib has a [[specific Array|Array type]], unless you have a specific need for performance, you're more likely to be working with Lists.

```js
const list = ["a", "b", "c"]
list[1] // "b"
list[7] // undefined
```

In the above code, trying to access the seventh index of `list` results in `undefined`.

Unlike JavaScript, Madlib protects you by wrapping this unsafe operation in a [[Monad - Maybe Type|Maybe]].

An equivalent version of the same code in Madlib:

```mad
import List from "List"
list = ["a", "b", "c"]
List.nth(1, list) // Just("b")
List.nth(7, list) // Nothing
```

The above code may feel unwieldy coming from JS, but ultimately this safety helps prevent invalid states.

In order to get the raw value out from a Maybe, you can use `Maybe.fromMaybe` or [[Core - Data Types#Deconstruction|deconstruct the container]] using `where`:

```mad
import { nth } from "List"
import { fromMaybe } from "Maybe"

list = ["a", "b", "c"]
raw = pipe(
  nth(7),
  fromMaybe("?")
)(list) // "?"
```

Note that the type passed to `fromMaybe` must [[Migration - Coming From JavaScript#Function return types|be consistent]] with the value being unwrapped here (so in both cases it will return a raw String).

## Function parameters need parentheses

Madlib has a slight difference from JavaScript with regard to unary functions:
```js
const identity = x => x
```

This is not valid in Madlib, as all functions, regardless of arity, need parentheses around the parameters:

```
nullary = () => 5
unary = (x) => x
binary = (x, y) => x ++ y
```
## Function return types

Like other strongly typed languages, Madlib mandates consistent returns. A function _cannot_ return multiple types. If you had this impractical function in JS:

```js
const yesValue = (x) => x === "yes" ? true : x
```

It would be invalid in Madlib:
```mad
yesValue = (x) => x == "yes" ? true : x
```

The above function throws a `Type error: expected: Boolean but found: String`

In order to work around this, you could define a custom type (though this particular solution is far from practical):

```mad
type MatchValue = Matched | Unmatched(String)

yesValue :: String -> MatchValue
yesValue = (x) -> x == "yes" ? Matched : Unmatched(x)
```

The above function is valid because both `Matched` and `Unmatched(x)` are of type `MatchValue`