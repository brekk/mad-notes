---
aliases:
  - Mutation
---
Values can only be mutated with the [[Language Design - Explicitness|explicit]] mutation operator: `:=` — and only within the body of a function (so not in top-level variables).

This is dangerous as it introduces ambiguity into the state, but we've allowed it for performance reasons. If you declare a value (`x = 5`) and then try to re-assign the value (`x = 8`) this will result in an error. If absolutely needed you can re-assign the value, e.g. `x := 8`