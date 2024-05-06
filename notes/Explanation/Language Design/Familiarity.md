---
aliases: []
tags:
  - language-design
---
To the extent that JavaScript or Haskell has a defined position on a topic, _usually_ we've tried to cleave to the familiar. To a lesser extent, since we also target LLVM (C / C++) as a binary, some of those language considerations have also carried over. When there's inconsistency between our core languages, we've tried to make an overt choice in favor of [[Explicitness|explicitness]], as well as providing sugar syntax to [[Low Friction|lower the friction]] / mental overhead of adopting Madlib's paradigms.