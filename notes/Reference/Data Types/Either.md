---
aliases: 
tags:
  - reference
  - "#prelude"
  - data-type
---
##### Synopsis
- An Either is a tagged union which allows us to elide over cases which are [[Make Invalid States Unrepresentable|otherwise impossible or illogical]] while capturing additional information for consumption downstream; success values are wrapped in `Right` and failure values are wrapped in a `Left`. It is similar to the [[Maybe]] type but unlike the `Nothing` constructor the `Left` can be transformed and contains additional info.
## Construction
### Left
A unary constructor which captures a failure case
### Right
A unary constructor which captures a success case
## Usage

We can use the Either type to model cases in which we have multiple points of possible failure. Consider a problem where we have a command-line executable which accepts inline flags _or_ a configuration file which specifies those same flags. What if one specifies an invalid flag? What if one specifies an invalid path to a configuration file? What if one has made a mistake in the configuration file itself?