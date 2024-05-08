---
aliases: 
tags:
  - development
  - javascript
---
For developers coming to Madlib from JavaScript, there are a few notable differences one should be aware of.

## Variable declaration

There is no specific keyword needed to declare a variable, like `let` or `const` or `var` in JS. Instead all values are declared with the assignment operator, e.g. `x = 5` . However, values cannot be mutated, [[Limit Mutation|except in limited circumstances]].

## Concatenating Strings has a different operator

In JavaScript, the same operator is used for adding numbers and concatenating strings (`+`), e.g. `5 + 4` / `"hello " + "world"`. In Madlib, addition is the same, but concatenating strings requires the concatenation operator (`++`) which is infixed / between operands. The this operator works to concatenate Lists: `[1,2,3] ++ [4,5,6]`. There is also a globally available function `mconcat` which affords the same behavior but as a function instead of an operator. `mconcat("hello ", "world") == "hello " ++ "world"`.

## Logging is done with IO

Unlike `console.log` in JavaScript, in Madlib you will want to use the [[IO|IO module]]. It has some specific edges which may feel unfamiliar at first. Here are some examples:

```mad
import IO from "IO"

main = () => {
  IO.put("this is a sentence with no trailing newline")
  IO.putLine("this is a sentence with a trailing newline")
  pipe(
    (x) => x + 5,
    IO.trace("two plus five"),
    (x) => ({x}),
    IO.pTrace("pretty trace the record with x in it!")
  )(2)
}
```

Additionally, you may need to call `show`, which is a built-in function that coerces values into strings, in order to pass a value to one of the IO functions.

```
import IO from "IO"

main = () => {
  IO.put(12345) // this results in an "Instance not found" error
}
```

This can be fixed by changing the logging line to: `IO.put(show(12345))` 
## Runnable files need a `main` function

Unlike JS, where all files are the same and have the same semantics: in Madlib if you want to run a file via `madlib run` (e.g. `madlib run ./src/Main.mad`) it expects that there is a `main` function. This function doesn't need any parameters and must have no return / return [[Types#Unit|Unit]].

## Copies over Mutation

In Madlib are [[Limit Mutation|limited circumstances where values can be mutated]] — by default most functions will instead return a modified copy of the the original value. For instance, `List.append` will yield a new list of values rather than in-place mutation:

```mad
import List from "List"
import IO from "IO"

list = ["a", "b", "c"]
list2 = List.append("d", list)
IO.pTrace("Note that list is unchanged!", #[list, list2])
```

**NB**: This doesn't hold for the specialized [[Coming From JavaScript#Madlib Arrays|Array type]] but it is an outlier in the language.

## Strings cannot be created with single-quotes

In JavaScript, strings can be created with single quotes (`'`), double quotes (`"`) or backticks (`` ` ``). In Madlib, single quotes are used to delimit [[Types#Char|Chars]]. Double quotes and backticks can both be used identically to their usage in JavaScript.

## Strict Equality

Madlib has a single equality operator <code>==</code> and the language itself is designed to be [[Explicitness|explicit]] where possible. Unlike JS, instead of forcing the developer to remember to use a different operator of increased strictness, <code>===</code> is not a valid operator and will throw a syntax error.

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

In Madlib logical conditions are an expression, exactly like ternary expressions in JS. So multiple returns here would be invalid (unless using [[Do Notation|do notation]], which we will discuss more [[Coming From JavaScript#Conditional `do`|below]]).

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

There is a logical case (where `x` is greater than or equal to 15) which we haven't accounted for. As this would implicitly return [[Types#Unit|Unit]], this results in an error.

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
Without the `do`, this is a grammar error because it's not a single expression. Without the `return` in the body of the `do`, that logical branch executes but returns `{}` / [[Types#Unit|Unit]], so it is an [[Coming From JavaScript#Function return types|inconsistent return type]].

## Working with Lists

Though Madlib has an [[Array|Array type]], unless you have a specific need for performance, you're more likely to be working with Lists.

```js
const list = ["a", "b", "c"]
list[1] // "b"
list[7] // undefined
```

In the above code, trying to access the seventh index of `list` results in `undefined`.

Unlike JavaScript, Madlib protects you by wrapping this unsafe operation in a [[Maybe|Maybe]].

An equivalent version of the same code in Madlib:

```mad
import List from "List"
list = ["a", "b", "c"]
List.nth(1, list) // Just("b")
List.nth(7, list) // Nothing
```

The above code may feel unwieldy coming from JS, but ultimately this safety helps [[Make Invalid States Unrepresentable|prevent invalid states]].

In order to get the raw value out from a Maybe, you can use `Maybe.fromMaybe` or [[Data Types#Deconstruction|deconstruct the container]] using `where`:

```mad
import { nth } from "List"
import { fromMaybe } from "Maybe"

list = ["a", "b", "c"]
raw = pipe(
  nth(7),
  fromMaybe("?")
)(list) // "?"
```

Note that the type passed to `fromMaybe` must [[Coming From JavaScript#Function return types|be consistent]] with the value being unwrapped here (so in both cases it will return a raw String).

## Madlib Arrays

Madlib has a specialized [[Array|Array type]] which you can use if you have a specific need for performance. (If you're not sure, you _probably_ don't.)

You can create an Array from a List like so:
```mad
import Array from "Array"

a = Array.fromList(["a", "b", "c"])
```

Arrays in Madlib are unusual in that they have operations which mutate them in place, unlike [[Coming From JavaScript#Copies over Mutation|most functions in Madlib]].

```mad
import Array from "Array"
import IO from "IO"

alpha = Array.fromList(["a", "b", "c"])
Array.push("z", a)
IO.pTrace("mutant!", alpha)
```

Additionally, unlike Lists, Arrays allow for unsafe index access:

```mad
alpha = Array.fromList(["a", "b", "c"])
IO.pTrace("...", alpha[0]) // ... a
```

However, there is **no safety net**. If you try to access an invalid index, your program will fail at runtime.

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

## Functions must return consistent types

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

## objects vs. Records vs. Dictionaries

In JavaScript one might create an object and then add values to it dynamically, e.g.
```javascript
const obj = {}
obj.j = true
obj.k = false
obj.l = "hello"
```

In Madlib, there are two literal container types and they have different properties which make them distinct from JS objects.

### Records

[[Record]] literals in Madlib look similar to JS objects syntactically, in that they are delimited by single curly braces: `{` ... `}` and have key-value pairs. These values do not need to be of the same type.

```mad
myRecord = {
  j: true,
  k: false,
  l: "hello"
}
```

Above, the `myRecord` has an explicit type that matches it shape (and this is most easily expressed as a type definition using the [[Aliases|alias]] keyword):

```mad
alias CoolRec = {
  j :: Boolean,
  k :: Boolean,
  l :: String
}
myRecord :: CoolRec
myRecord = {
  j: true,
  k: false,
  l: "hello"
}
```

The fields of this record follow the same rules as literal values, they can be alphanumeric characters and underscores, cannot include a space nor start with a number.

Unlike JS objects, you cannot build a record up piecewise; that is, a record with partial fields is a distinct type, and cannot have fields assigned directly, e.g.

```
myPartialRecord = {
  j: true
}
// invalid
myPartialRecord.k = false
```

However, like JS objects, you can spread records:
```
myPartialRecord = { j: true }
myRecord :: CoolRec
myRecord = { ...myPartialRecord, k: false, l: "cool" }
```

But you cannot currently spread more than one record together:

```mad
myPartialRecord = { j: true }
myPartialOtherRecord = { k: false }
myRecord :: CoolRec
// invalid syntax
myRecord = { ...myPartialRecord, ...myPartialOtherRecord, l: "cool" }
```

### Dictionaries

Dictionaries are a literal type — they are distinct from Records in a few ways. Firstly, they are delimited by double curly braces: `{{` ... `}}`, they have key-value pairs, but the keys _can_ be dynamic, and the values _must_ be of the same type.

```mad
myDict :: Dictionary String Boolean
myDict = {{
  
}}
```