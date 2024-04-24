---
aliases: []
tags:
  - syntax
  - fundamentals
---
##### Synopsis
- `#iftarget` syntax is used to specify code which is reliant on a specific ecosystem / target. It pairs well with [[The Fence]] and the [[Externally linked libraries|extern]] keyword when targeting JavaScript and LLVM environments respectively.

Code _should_ be written to be portable across all-targets by default:
- JS / web code uses `#iftarget js`
- LLVM code uses `#iftarget llvm`
- This may vary based upon intent — but all core Madlib code should have parity across both targets

Code which uses [[The Fence|The Fence]] should be written within `#iftarget js` 
Code which uses [[Externally linked libraries|externally linked libraries]] should be written within `#iftarget llvm`