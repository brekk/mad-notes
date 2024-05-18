---
tags:
  - fundamentals
  - literals
  - logic
  - guide
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

Here are some simple expressions you can start to play with in the REPL before we set up a more serious [[05 - The March of IDEs|programming environment]].

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


##### Summary
In this document, we've discussed:
- [[01 - Hello mad, mad world#Installation|Installing]] Madlib
- Using the [[01 - Hello mad, mad world#REPL|REPL]]
- Basic [[01 - Hello mad, mad world#Basic arithmetic|arithmetic]]
- [[01 - Hello mad, mad world#Basic boolean logic|Boolean logic]]
- [[01 - Hello mad, mad world#Asserting equality|Expressions of equality]]
- [[01 - Hello mad, mad world|Importing]] and invoking functions from Prelude

