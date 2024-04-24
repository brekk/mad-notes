---
aliases:
  - Types
tags:
  - fundamentals
---
## Boolean
Binary values: `true` or `false`
## Char
Single characters delimited by single quotes: e.g. `'a'` 
## Numeric
Types that represent number values. When needed in functions which can accept generic numbers, remember to declare the [[Functions#Constraints|constraints]].
### Integer
Signed whole numbers which can be negative. These can be expressed as digits with or without a `_i` suffix.
### Byte
Positive whole numbers between 0 and 255. These can be expressed with or without an explicit `_b` suffix.
### Short
Signed whole numbers between -32,768 and 32,767. These can be expressed with or without an explicit `_s` suffix.

Statements:
- They are constructed as [complements of two](https://en.wikipedia.org/wiki/Two%27s_complement)
- The Short contains minimum value of -32,768 and a maximum value of 32,767 (inclusive)
- Its default value is 0
- Its default size is 2 byte
- Adding two Shorts results in an Integer
### Float
Signed floating point numbers, which can be expressed with or without an explicit `_f` suffix.
## String
A [[Types#List|List]] of [[Types#Char|Characters]], delimited by double quotes `"` or backticks `` ` ``
## List
An enumeration of the same type, delimited by square brackets
## Tuples
A specific length enumeration of possibly mixed types, delimited by square brackets beginning with a hash, e.g. `#["two", 2]`
## Dictionaries
A key-value structure whose values are all the same type
## Records
A key-value structure whose values do not need to be the same type
## Unit
A unit type is a type which holds no concrete value. It can also be thought of as a [[Types#Tuples|Tuple]] with no elements. This is the inferred return type for a multi-line function which has no explicit `return`. 

## Aliases
Aliases are a way of encapsulating (ideally) more complex structures more tersely. They are ideal for expressing records concisely:
```
alias Person = {
  age :: Integer,
  name :: String
}
```
### Alias Misuse
Technically it is possible to alias a simple type to a different name, but _currently_ the compiler doesn't follow this aliasing, so something like `alias Num = Integer` has potential to confuse unfamiliar readers.