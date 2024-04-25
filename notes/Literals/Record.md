---
tags:
  - fundamentals
  - literals
  - syntax
---
##### Synopsis
- Records offer a syntactic means of expressing a container type that has fields of differing types, delimited by single curly braces: `{` ... `}`. It is designed to pair wel[[Record]]shorthand]]. It is similar but distinct from a [[Dictionary]], which has a slightly different syntax and whose fields must be of the same type. Records pair well with [[Aliases|alias]] syntax in order to reference a complex type by a domain-specific name.

## Dot Property shorthand

Madlib has a sugar syntax for accessing a property of a [[Record]] simply. If there is a Record with a "name" field, e.g. `person = { name: "Arnaud" }`, we can both access the name inline, like `person.name`, or we can use the dot property shorthand to access the value as a function: `.name(person)`. This is especially helpful when combined with [[Pipe Composition|pipe composition]], e.g. `pipe(.name, String.toUpper)(person)`