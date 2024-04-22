---
aliases:
  - Mutation
---
Mutation of stateful values is a common source of errors and ambiguity. Madlib tries to prevent this common footgun by disallowing mutation of top-level values and erroring on unintentional mutation within closures.

If you declare a value (`x = 5`) and then try to re-assign the value (`x = 8`) this will result in an error. Values can only be mutated with the [[Language Design - Explicitness|explicit]] mutation operator (`:=`) — we added this feature somewhat recently for performance reasons, but most of the language is intended to be used without this escape hatch.