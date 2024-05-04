---
aliases: 
tags:
  - prelude
---
##### Synopsis
- The built-in algebraic data types that Madlib ships with will give you powerful tools to manipulate and encapsulate logic within applications.
## Prelude

Our standard library (the functionality available without a third-party library) is called **Prelude**, much like Haskell's. These documents may refer to the standard library and Prelude interchangeably.

## Core Data Types

```dataview
LIST L.text
FROM "Data Types"
FLATTEN file.lists AS L
WHERE meta(L.section).subpath = "Synopsis"
SORT file.name
```

## Usage

To make use of the core data types, there are two kinds of [[Modules|syntactic imports]], `import type` and concrete `import`s.

To use a data type in a function signature, you must import its type:

```
import type { Maybe } from "Maybe"
import { Just, Nothing } from "Maybe"

first :: List a -> Maybe a
export first = (list) => where (list) {
  [] => Nothing
  [x, ..._] => Just(x)
}
```

In order to make use of a type constructor, you must import its constructor:

```
import { Just } from "Maybe"
x = Just(5)
```

or reference it via a named import:

```
import Maybe from "Maybe"
x = Maybe.Just(5)
```

## Deconstruction

A common pattern in Madlib is to use [[Pattern Matching|pattern matching via `where`]] to deconstruct the interior values of a given data type.