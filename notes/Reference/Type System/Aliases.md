---
aliases: 
tags:
  - syntax
---
##### Synopsis
- Aliases are a way of referencing more complex structures more tersely. They are ideal for expressing records concisely or referencing a type by a more idiomatic / domain-specific name.

## Usage

The most common use case of an alias is to name a [[Record]]:

```
alias Person = {
  age :: Integer,
  name :: String
}
```

### Alias Misuse

Technically it is possible to alias a simple type to a different name, but _currently_ the compiler doesn't follow this aliasing, so something like `alias Num = Integer` has potential to confuse unfamiliar readers.