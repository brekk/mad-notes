In the [[01 - Hello mad, mad world|previous guide]] we talked through the basics of [[01 - Hello mad, mad world#Installation|installing]] Madlib and running the [[01 - Hello mad, mad world#REPL|REPL]].

In this document we'll talk through writing our own `main` function, which will allow us to save and execute code outside of the REPL.
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

## Summary
- Function [[02 - Writing functions, in the main#Defining functions|definitions]]
- Defining and running a [[02 - Writing functions, in the main#A main function|main]] function
- [[02 - Writing functions, in the main#Partial application and curry|Curried]] functions and their (partial) [[02 - Writing functions, in the main#Function application operator|applications]]
- [[02 - Writing functions, in the main#Composition|Composing]] functions
- Applying functions to a List with [[02 - Writing functions, in the main#Applying a function to a List|map]]
