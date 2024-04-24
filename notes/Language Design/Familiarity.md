---
aliases: []
tags:
  - language-design
---
To the extent that JavaScript or Haskell has a defined position on a topic, _usually_ we've cleaved to the familiar. To a lesser extent, since we also target LLVM (C / C++) as a binary, some of those language considerations have also carried over. When there's inconsistency between our core languages, we've tried to make an overt choice in favor of [[Explicitness|explicitness]].

## JavaScript
- [[The Fence]] allows for a JS escape hatch
## LLVM
- [[Externally linked libraries]] / the `extern` keyword allow for a LLVM escape hatch