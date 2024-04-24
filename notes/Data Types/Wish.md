---
aliases: []
tags:
  - prelude
  - reference
---
##### Synopsis
- The Wish type is used to model asynchrony. It is used throughout the core of Madlib to provide a monadic interface to capture, transform and evaluate values over time. It is lazily evaluated and is "cold" until the Wish is `fulfill`ed. Several useful modules take advantage of the Wish interface, including [[Test]].