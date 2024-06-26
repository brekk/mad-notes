---
aliases: []
tags:
  - fundamentals
  - syntax
---
##### Synopsis
- Madlib allows for the encapsulation and publication of code. This is largely leveraged by use of [[Modules#Imports|Imports]] and [[Modules#Exports|Exports]]. The `import` keyword is used to reference code written in a different file / module within the current file.  The `export` keyword is used to make locally declared values accessible from other files via [[Modules#Imports|Imports]]. Types, variables and constructors can be exported.

## Imports

Imports are the complement / consumer side of [[Modules#Exports|export]] syntax. There are two kinds of imports; in the same way that there are [[Type Signatures|type signatures]] and [[Functions#Construction|function implementations]], there are both `type` imports and concrete imports.
### Type Imports

In order to import a type from a external file, use the keywords `import type`, for example:

```
import type { Maybe } from "Maybe"
```

### Concrete Imports

In order to import a concrete value (for use in [[Functions#Construction|function implementations]]), one can either import a named container literal or specific import, using the `import` keyword.

#### Named literal import

A named literal import looks like this:

```
import Maybe from "Maybe"
```

The name of the literal is up to you, if you chose to you could also name it like so:

```
import M from "Maybe"
```

If you navigate the code in [Prelude](https://github.com/madlib-lang/madlib/tree/master/prelude/__internal__), we usually prefer the former by convention for readability, but this is not mandatory.

#### Specific imports

In addition to importing a literal with a name, you can also access a subset of the contained value upon import.

```
import { fromMaybe } from "Maybe"
```

This is is a terser sugar for:

```
import M from "Maybe"
fromMaybe = M.fromMaybe
```

In the pursuit of [[Explicitness|explicitness]] we have eschewed renaming the imported values as a syntactic feature, but you can do so manually if you wish:

```
import { always, identity } from "Function"
combinatorK = always
combinatorI = identity
```

## Exports
