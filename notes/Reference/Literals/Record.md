---
tags:
  - literals
  - syntax
---
##### Synopsis
- Records offer a syntactic means of expressing a container type that has key-value pairs of possibly differing types, delimited by single curly braces: `{` ... `}`. It  It is similar but distinct from a [[Dictionary]], which has a slightly different syntax and whose fields must be of the same type. Records pair well with [[Aliases|alias]] syntax in order to reference a complex type by a domain-specific name.

## Example
```mad
alias CoolRec = {
  j :: Boolean,
  k :: Boolean,
  l :: String
}
myRecord :: CoolRec
myRecord = {
  j: true,
  k: false,
  l: "hello"
}
```

The fields of this record follow the same rules as literal values, they can be alphanumeric characters and underscores, cannot include a space nor start with a number.

## Differing fields, different types

A record with a different number of fields or different type of fields is considered distinct. Consider the following:

```mad
alias Person = {
  name :: String,
}
alias PersonWithAddress = {
  name :: String,
  address :: String
}
alias RobotWithAddress = {
  name :: Integer,
  address :: String
}
```

These are all distinct types, so they cannot be mixed in a collection or as return types.

## Reusing aliased definitions

`alias` definitions can be merged with the spread operator (`...`):

```
alias Person = {
  name :: String,
}
// id, name, description
type Item = Item(Integer, String, String)
alias PersonWithCart = {
  ...Person,
  address :: String,
  cart :: List Item
}
```

But you cannot currently spread more than one record definition together:

```mad
// this is not valid
alias Addressable = {
  address :: String
}
alias PersonWithCart = {
  ...Person,
  ...Addressable,
  cart :: List Item
}
```

## Merging instances

Record literals can also be merged together via the spread operator (`...`):

```
// id, name, description
type Item = Item(Integer, String, String)
alias PersonWithCart = {
  name :: String,
  address :: String,
  cart :: List Item
}
brekk = { name: "Brekk" }
customer = {
  ...brekk,
  address: "The Moon!",
  cart: [
    Item(1, "Moon Rock", "It's like an earth rock, but different")
  ]
}
```

But you cannot currently merge more than one record definition together:

```mad
brekk = { name: "Brekk" }
moon = { address: "The Moon!" }
// this is not valid
customer = {
  ...brekk,
  ...moon,
  cart: [
    Item(1, "Moon Rock", "It's like an earth rock, but different")
  ]
}
```
## Dot Property shorthand

Madlib has a sugar syntax for accessing a property of a [[Record]] simply. If there is a Record with a "name" field, e.g. `person = { name: "Arnaud" }`, we can both access the name inline, like `person.name`, or we can use the dot property shorthand to access the value as a function: `.name(person)`. This is especially helpful when combined with [[Functions#Pipe Composition|pipe composition]], e.g. `pipe(.name, String.toUpper)(person)`