---
aliases: 
tags:
  - fundamentals
  - syntax
---
##### Synopsis
- The Fence is a syntactic escape hatch which allows for raw JavaScript to be written in Madlib between `#-` ... `-#` pairs, at the cost of automatic inference.

**The Fence** is our built-in escape hatch. While Madlib has a powerful type and inference system, "fenced" code allows you to drop down to any native JS construct. However, doing so will eschew any of the automatic type inference and instead treat code within the **The Fence** as opaque.

It has been a core feature of Madlib ideology from the very start of this project, as it can (if used _judiciously_) allow developers to build on top of existing JavaScript projects or allow for incremental migration to Madlib [[Coming From JavaScript|from JavaScript]].