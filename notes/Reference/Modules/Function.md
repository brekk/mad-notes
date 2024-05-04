---
tags:
  - module
  - logic
  - control-flow
---
##### Synopsis
- The Function module provides tooling that makes combinatorial logic easier

## Functions

### complement

A function which inverts a predicate function. Note that `complement` is similar to `not`, but instead of inverting a value it inverts a predicate.

```mad
import { complement } from "Function"

falsy = () => false
truthy = complement(falsy)
truthy() // true
```

### always

A function that takes a value and returns a nullary function that returns the original value when invoked. It is also known as the K combinator.

```mad
import { always } from "Function"

hundo = always(100)
hundo() // 100
```

### identity

A function which takes a value and returns it. It is also known as the I combinator.

```mad
import { identity } from "Function"

identity(100) // 100
```

### equals

A function which takes two values and compares them, returning true if they are equivalent. This is especially helpful when working in a [[Functions#Pipe Composition|composed function]].

```mad
import { equals } from "Function"

isFive = equals(5)
isFive(7) // false
isFive(5) // true

pipe(
  transformation,
  equals(expected)
)(input)
```

### notEquals

A function which takes two values and compares them, returning true if they are not equivalent. (It is the [[Function#complement|complement]] of [[Function#equals|equals]].) This is especially helpful when working in [[Functions#Pipe Composition|pipe composition]].

```mad
import { notEquals } from "Function"

isNotFive = notEquals(5)
isFive(7) // false
isFive(5) // true

pipe(
  transformation,
  notEquals(expected)
)(input)
```

### ifElse

A function which takes a predicate and two transformation functions. It is helpful for control flow in [[Functions#Pipe Composition|pipe composition]].

```mad
import { ifElse, equals } from "Function"

ifElse(
  equals(5),
  (x) => x * 2,
  (x) => x / 2
)(7) // 3.5
```

### when

A function which takes a predicate and a transformation function. If the predicate returns true, it runs the transformation function against the input. Otherwise the value is untransformed. `when` is a partial application of `ifElse` where the failure path is `identity`. It is helpful for control flow in [[Functions#Pipe Composition|pipe composition]].

```mad
import { when, equals } from "Function"

when(
  equals(5),
  (x) => x * 2
)(4) // 4
```

### unless

A function which takes a predicate and a transformation function. If the predicate returns false, it runs the transformation function against the input. Otherwise the value is untransformed. `unless` is a partial application of `ifElse` where the success path is `identity`. It is helpful for control flow in [[Functions#Pipe Composition|pipe composition]].

```mad
import { unless, equals } from "Function"

unless(
  equals(5),
  (x) => x * 2
)(4) // 8
```

### not

A function which takes a boolean and inverts it. It is the functional form of the boolean inversion operator (`!`). It is helpful for logic within [[Functions#Pipe Composition|pipe composition]]. Note that `not` is similar to `complement` but instead of inverting a function it inverts a value.

```mad
import { not } from "Function"

not(true) // false

pipe(
  successFunction,
  not
)(input)
```

### noop

Short for "no operation", this unary function takes anything and returns [[Unit]]. This can be useful for eschewing side-effects.

```mad
import { noop } from "Function"

noop(100)
```