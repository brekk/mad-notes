---
tags:
  - syntax
---
##### Synopsis
- Madlib disallows [[Limit Mutation|mutation]] except in limited circumstances. The explicit mutation operator (`:=`) allows for re-assignment of a non top-level values. Attempting to re-assign a value with the [[Assignment Operator]] will result in an error.