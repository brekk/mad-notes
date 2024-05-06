---
tags:
  - fundamentals
  - literals
  - curry
  - composition
---
## Installation

There are a few ways to [[Installation|install Madlib]], but the easiest is to install it via [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm).

```sh
npm i @madlib-lang/madlib -g
```

This should make the `madlib` binary globally accessible, so you should see a version number when you run `madlib --version`:

```sh
> madlib --version
madlib@0.23.13
```

## REPL

For lightweight or exploratory work, you can work within the Madlib REPL (Read-Eval-Print-Loop). This is a sandbox where you can write code as you learn more about Madlib.

The REPL can be accessed with the command `madlib repl`, which should bring up an interactive session that looks like this:

```
------ REPL - Madlib@0.23.13 -------------------------------
Available commands:
  :help           show help (alias :h)
  :exit           exit the REPL (alias :e)
  :multi          start multiline mode (alias :m)
  :type NAME      show the type of an assignment (alias :t)
  :reset          reset the state of the repl (alias :r)
-----------------------------------------------------------

>
```

Here are some simple expressions you can start to play with in the REPL before we set up a more serious programming environment.

### Basic arithmetic

```mad
> 5 + 5
10 :: Integer
> 10 - 100
-90 :: Integer
> 42 * 10
420 :: Integer
> 9 / 6
1.5 :: Float
```

As you can see, we're not doing anything complex here — but this illustrates the way we can write an expression and see the result, along with its type. The type is indicated by the double-colon / `::` , which can be read as "has type of".

These follow the usual precedence rules, and can be changed by adding parentheses:

```mad
> 3 * 4 / (5 - 6)
-12 :: Float
> 3 * 4 / 5 - 6
-3.6 :: Float
> 3 * (4 / 5) - 6
-3.5999999999999996 :: Float
```

## Basic boolean logic

Here are some simple boolean expressions.

```mad
> true && false
false :: Boolean
> true && true
true :: Boolean
> false || true
true :: Boolean
> !(false && false)
true :: Boolean
```

As you can see, we have the literals `true` and `false`, as well as the logical *and* (`&&`) and logical *or* (`||`) operators. Finally, we have a boolean negation operator (`!`) which is infixed.

## Asserting equality

Here we can assert the equality of things:

```mad
> 5 == 5
true :: Boolean
> 1 == 0
false :: Boolean
> 7 != 0
true :: Boolean
> "hello" == "world"
false :: Boolean
```

Here we've shown the equality operator ( ` == `) as well as the inequality operator (`!=`).

## Try mixing types

What happens if we try to do something like add `5 + "cool"`?

```mad
> 5 + "cool"
Instance not found

No instance for 'Number String' was found.

Hint: Verify that you imported the module where the Number
instance for 'String' is defined
Note: Remember that instance methods are automatically imported when the module is imported, directly, or indirectly.
```

Here Madlib is saying that "cool" is not a number and so it doesn't know how to add 5 to it. We'll discuss this kind of error more later on.

## Import functions from Prelude

In Madlib, we call our standard library _Prelude_ (a name taken from Haskell). We'll go over everything it has to offer in more detail shortly, but let's start by importing `Math`:

```mad
> import Math from "Math"
```

[Math](https://github.com/madlib-lang/madlib/blob/master/prelude/__internal__/Math.mad) has a few functions we can play with. We won't go over everything here, just enough to give a general sense of how it works.

```mad
> import Math from "Math"
> Math.max(100, 20)
100 :: Integer
> Math.min(100, 20)
20 :: Integer
> x = Math.divide(3, 4)
0.75 :: Float
> y = Math.sqrt(100)
10 :: Float
> z = Math.abs(-1000)
1000 :: Integer
> x
0.75 :: Float
```

As you can see, functions are invoked by passing them parameters within parentheses. We've also assigned some of these results to variables (`x`, `y`, `z`) and we've shown that the REPL will remember these values in memory.

We'll also try playing with a few of the functions defined in [List](https://github.com/madlib-lang/madlib/blob/master/prelude/__internal__/List.mad). Again, this isn't comprehensive but will give a sense of how they work:

```mad
> import List from "List"
> List.range(0, 10)
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9] :: List Integer
> nums = [1,2,3,4,500,7,92,30]
> List.reduce(Math.max, -1, nums)
500 :: Integer
> List.reduce((a, b) => a + b, 0, nums)
639 :: Integer
```
## Defining functions

Let's step through the process of defining functions in a standalone `main` file. Open up your favorite text editor and write the following

```mad
import IO from "IO"

say :: String -> String -> String
say = (word, subject) => word ++ " " ++ subject

main = () => {
  pipe(
    say("hello"),
    IO.putLine
  )("world")
}
```

Save this file as `Say.main.mad` or whatever you like, as long as it ends in `.mad`.

We can run this file standalone with the command `madlib run`:
```sh
> madlib run Say.main.mad
hello world
```

Let's talk through exactly what we've done here, as there's a few pieces.

### Type signatures

Firstly, we've defined a function named `say`. It has a [[Type Signatures|type signature]], `say :: String -> String -> String` — this means that it is a binary function, taking two parameters (of type `String`), and its return type is also a `String`. Learning to read these type signatures can take some time, but we'll continue to articulate what they mean as we go through this process.

Let's see a few examples of more signatures (we're omitting the function implementations here, but they'd normally be required in order to be syntactically valid):

```mad
func :: Integer -> String
```

The function above takes an [[Numeric Types#Integer|Integer]] and returns a [[String]].

```mad
otherFunc :: Float -> Float -> Float -> List Float
```

This `otherFunc` takes three [[Numeric Types#Float|floating point numbers]] and returns a [[List]] of floating point numbers.

```mad
thirdFunction :: Char -> String -> Boolean
```

This `thirdFunction` takes a [[Char]] and a [[String]] and returns a [[Boolean]].

### Function implementation

Coming back to the `say` function we defined earlier, let's talk through its actual implementation details:

```mad
say :: String -> String -> String
say = (word, subject) => word ++ " " ++ subject
```

This definition allows us to associate a concrete implementation with the types in the signature. So `word` here is a [[String]], as is `subject`. The return type is also a String, which works out nicely because the concatenation operator `++` works on Strings and Lists, so `word ++ " " ++ subject` is a String concatenated with two other Strings.

As written above, we're using the lambda form of a function, which has an implicit `return` value, after the ` =>`.

Let's see what happens if we define the function with curly braces:

```mad
say :: String -> String -> String
say = (word, subject) => {
  word ++ " " ++ subject
}
```

This causes the compiler to be unhappy:

```
[error]: Type error
     ╭──▶ /how-to/Say.main.mad@6:1-8:1
     │
   6 │ ╭┤ say :: String -> String -> String
   7 │ │  say = (verb, subject) => {
   8 │ ├┤   verb ++ " " ++ subject
     • │
     • ╰╸ expected:
     •      String -> String -> String
     •
     •    but found:
     •      String -> String -> {}
     •
─────╯
```

You can see from the error that this change has caused the function to no longer return a String, but instead this set of empty curly braces: `{}` — this is also known as the [[Unit]] type.

In order to fix this we need to add the explicit `return` keyword (or change the type signature so that it returns Unit instead: `String -> String -> {}`)

```mad
say :: String -> String -> String
say = (word, subject) => {
  return word ++ " " ++ subject
}
```

### Function invocation

To see this in action, we need to call the function:

```mad
main = () => {
  pipe(
    say("hello"),
    IO.putLine
  )("world")
}
```

This adds a few minor wrinkles, so let's talk through them.

### A main function

In order to call our function from the command line, we need to define a `main` function. This function is special in that it _must_ be named `main` and it needs to return [[Unit]] / `{}`.

### Partial application and curry

Recall earlier when we were showing `Math.max`, we passed in two parameters in the same invocation:

```mad
> Math.max(100, 20)
100 :: Integer
```

If we chose to, we could call our `say` function in this same manner

```mad
main = () => {
  say("hello", "world")
}
```

However, this will run but not print anything. That's because we've now omitted the `IO.putLine` function, which actually prints the input.

```mad
main = () => {
  IO.putLine(say("hello", "world"))
}
```

**Now** this function will print correctly.

_So why did the original version work?_

This is because we had _partially-applied_ the `say` function. Consider this bit of code:

```
main = () => {
  hi = say("hello")
  IO.putLine(hi("world")) // "hello world"
  IO.putLine(hi("there")) // "hello there"
  IO.putLine(hi("hey")) // "hello hey"
}
```

Recall that our `say` function takes two parameters. Unlike some other more imperative languages, if we invoke a function with fewer parameters than it needs, in Madlib we get back a function which expects the remaining parameters. This is called currying a function. _All functions_ in Madlib are curried.

If we come back to our definition, you can think of this as crossing off one of the values in the signature:

```
// this function is curried!
say :: String -> String -> String
say = (word, subject) => word ++ " " ++ subject

// partial application
hi :: String -> String
hi = say("hello")
```

(Since the Madlib compiler is smart and capable, we don't actually need to define the type definition for `hi`, but we've done so here for illustrative porpoises.)

### Composition

If you recall from our original example, we used the special `pipe` function:

```mad
main = () => {
  pipe(
    say("hello"),
    IO.putLine
  )("world")
}
```

This allows us to _compose_ functions together. When we discussed partial application above, we named the partially applied version of `say`:

```
hi = say("hello")
```

And we wrapped its invocation in `IO.putLine`:

```mad
IO.putLine(hi("world"))
```

The `pipe` function allows us to write things with fewer parentheses and compose operations from top to bottom:

```mad
pipe(
  say("hello"),
  IO.putLine
)("world")
```

This is the exact same as:

```mad
IO.putLine(say("hello")("world"))
```

This may seem confusing or needless at first, but as we add more complexity you'll start to see the utility of this alternative form.

### Function application operator

We've managed to articulate quite a few things with this simple example. Let's add two more things to our understanding before we present you with a challenge.

If you recall above, we used the `Math.divide` function like so:

```mad
> Math.divide(3, 4)
0.75 :: Float
```

This is the same as using the division operator (`/`): `3 / 4`

If we wanted to partially apply the `Math.divide` function, we'd always be passing in the numerator `3` before the denominator `4`. However, we can use the function application operator (`$`) to make this function much more valuable without changing its definition:

```
half = Math.divide($, 2)
half(100) // 50
```

### Applying a function to a List

When we were discussing partial application before, we had an example like this:

```mad
main = () => {
  hi = say("hello")
  IO.putLine(hi("world")) // "hello world"
  IO.putLine(hi("there")) // "hello there"
  IO.putLine(hi("hey")) // "hello hey"
}
```

Using the `map` function, we can re-use the same functionality while avoiding repetition:

```mad
main = () => {
  x = map(pipe(say("hello"), IO.putLine))(
    ["world", "there", "hey"]
  )
}
```

However, you'll note that as written, the resulting value `x` isn't a List of Strings, it's a List of [[Unit]]: `[{}, {}, {}]`. This is because `IO.putLine` prints a value but doesn't return it.

If we wanted to change that, we could do something like this instead, to capture the transformed map:

```mad
import IO from "IO"
import String from "String"

main = () => {
  x = map(
    say("hello"),
    ["world", "there", "hey"]
  )
  pipe(
    String.join(", "),
    IO.putLine
  )(x) // "hello world, hello there, hello hey"
}
```

NB: If you try to print `x` without turning it into a String first via `String.join`, you'll see an error similar to this:
```
[error]: Type error
     ╭──▶ /how-to/Say.main.mad@30:14-30:15
     │
  30 │   IO.putLine(x)
     •              ┬
     •              ╰╸ expected:
     •                   String
     •
     •                 but found:
     •                   List String
     •
─────╯
```

This is most easily worked around by calling `show` first, like `IO.putLine(show(x))` — We won't go into too much detail for the purposes of keeping this document reasonably short, but `show` is a useful and [[Show|semi-magical function]] which coerces values into Strings.
### Challenge:

In order to test our understanding and comprehension, here's a small challenge: [[Say Anything, Say Many Things]]
### Solution:

See a possible solution to the challenge [[Solution - Say Anything, Say Many Things|here]]