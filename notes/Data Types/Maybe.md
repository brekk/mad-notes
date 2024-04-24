---
aliases: []
tags:
  - prelude
  - reference
---
##### Synopsis
- A Maybe is a tagged union which allows us to elide over cases which are otherwise impossible or illogical; "good" values are wrapped in `Just` and "bad" values are transformed into `Nothing`.
## Constructors
### Nothing 
A nullary constructor used to represent failure
### Just
A unary constructor used to represent a success
## Usage
We can use the Maybe type to model specific domains which we would otherwise have to avoid or treat with caution. What if we have a function which tries to access a specific index of a given List, but that index is greater than the length of the List? In other languages we might simply fail at the call site or otherwise wrap the request in a `try ... catch` block. In Madlib we can avoid this high-specificity case and instead just return `Nothing` if we have invalid inputs, or `Just(theValueAtIndex)` otherwise. Relative to how Maybe is [[Interfaces#Constraints|constrained]] by Functor, we can also safely map over `Just` values, while ignoring `Nothing` cases.