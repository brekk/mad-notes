---
tags:
  - guide
---
In the last guide we went over the core of writing functions in Madlib. In this guide we’ll be going over common syntactic affordances.

## Comments

 Madlib has C-style comments which start with two slashes: (`//`) for single-lines or wrapped with (`/*` … `*/` ) for multi-line comments.

```mad
// this is a comment
/*
This is a
Multiline
Comment
*/
```

### Documentation comments

Additionally there is a special format for creating comments which can be automatically parsed and turned into structured JSON — and a [secondary tool](https://github.com/madlib-lang/maddoc) which can then turn this JSON into a website. 

```mad
/**
 * A function for division
 * @since 0.0.1
 * @example
 * third = div($, 3)
 */
div :: Integer -> Integer -> Float
div = (n, d) => n / d
```

 This is discussed more in [[06 - Developmental Growth|a future guide]].

## Conditionals

Conditions are syntactically expressed in one of two ways in Madlib.

### Ternary Expressions

Ternary conditions can be written in the following way:

```mad
x = 5
// condition ? truthy case : falsy case
y = x >= 0 ? “less than zero” : “greater than zero”
```

Note the way in which this is a logical expression. If we wanted to nest more cases, we could do so:

```mad
y = x == 0 ? “is zero” : x < 0 ? “less than zero” : “greater than zero”
```

### if … else

The other way of expressing conditions syntactically in Madlib is using the `if` and `else` keywords.

Unlike some other languages, this syntax is designed to be identical to ternary expressions: each branch has a single expression by default, has implicit returns and must be consistent in return type. Additionally, like ternary expressions, there is no means of short-circuiting.

Here’s the same example from above using `if` / `else` syntax:

```mad
x = 5
y = if (x >= 0) {
  “less than zero”
} else {
  “greater than zero”
}
```

Note the way in which this is also an expression and it is being assigned to `y`.

As constructed, we cannot add an additional expression to this.

If we wanted to add an additional expression, we’d need to add a [[Do Notation|do]] keyword:

#### Using `do` notation with conditionals

```mad
import IO from “IO”
x = 5
y = if (x >= 0) do {
  IO.putLine(“happy path”)
  return “less than zero”
} else {
  “greater than zero”
}
```

Note the way in which we’ve added both a `do` and an explicit `return` here. If the return is omitted, the compiler will infer that the return type of this expression is Unit / `{}`. Without the `do`, this is syntactically invalid because it’s not a single expression.

#### Implicit `else`

Given the behavior described above, the compiler will automatically add an `else` clause when it is omitted. However, this else clause necessarily returns [[Unit]] in order to be type consistent. Therefore it can be helpful to use `do` notation to eschew an explicit return value when using `if` clauses by themselves.

## Modules

Madlib allows segmentation of programs into separate files, called Modules. These are defined by usage of `import`s and `export`s.

### Imports

As we saw in the [[01 - Hello mad, mad world|previous guide]], files can make use of `import` syntax to pull in content from [[Prelude]]. They can also use the same syntax to pull in content from local modules or 3rd-party modules.

```mad
import MyLocalModule from "./MyLocalModule"
// or, direct access
import { method, variable } from "./MyLocalModule"
```

### Exports

The example above would correspond to a local module like this:

```mad
localvar = “this value is inaccessible outside of this file”
export variable = “this value is accessible elsewhere”

method :: a -> a
method = (x) => x
```

We'll discuss more about [[03 - Just My Type|defining functions]] in the next guide.
##### Summary
In this document, we've discussed:
- Annotating a Madlib program with [[02 - Syntactic Tactics#Comments|comments]]
- Defining branching logic with [[02 - Syntactic Tactics#Conditionals|conditionals]]
- Segmenting programs via modules, using [[02 - Syntactic Tactics#Imports|imports]] and [[02 - Syntactic Tactics#Exports|exports]]
- Defining and running a `main` function
