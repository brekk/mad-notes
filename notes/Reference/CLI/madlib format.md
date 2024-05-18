---
tags:
  - cli
---
##### Synopsis
- Madlib provides an automatic code formatter so you can focus more time on writing code and less time quibbling about how things look. Learn more about setting up an integrated development environment [[05 - The March of IDEs#Integrated Development Environment (IDE)|here]].

## Sort of important

The auto-formatter will sort import statements based on a few criteria and imported values alphabetically. This is done [[Avoid needless argument|to eschew]] what could otherwise be [bike-shed problems](https://en.wikipedia.org/wiki/Law_of_triviality).

### The Order of Imports

For both `import type` and concrete `import`s — the following order is observed:  

1. Prelude
2. Upstream Dependency
3. Local Module

And types get imported before concrete imports, so all possible imports have this sorting:

1. Prelude types
2. Upstream Dependency types
3. Local types
4. Prelude concrete imports
5. Upstream Dependency concrete imports
6. Local concrete imports