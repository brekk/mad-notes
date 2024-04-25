---
aliases: []
tags:
  - prelude
  - reference
---
##### Synopsis
- The Wish type is used to model asynchrony. It is used throughout the core of Madlib to provide a monadic interface to capture, transform and evaluate values over time. It is lazily evaluated and is "cold" until the Wish is `fulfill`ed. Several core modules make use of the Wish interface, including [[Test]].

## Construction

Wishes can be created in a few different ways. Both `Wish.of` and `Wish.good` are means for creating Wishes that capture the successful path. `Wish.bad` is a function for creating a failure case. 

```mad
import Wish from "Wish"

g = Wish.of("cool")
h = Wish.good("nice")
b = Wish.bad("Failure!")
```