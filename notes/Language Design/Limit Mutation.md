---
aliases: []
tags:
  - language-design
---
Mutation of stateful values is a common source of errors and ambiguity. Madlib tries to prevent this common footgun by disallowing mutation of top-level values and erroring on unintentional mutation.

## Copies instead of Mutation

Most functions do not mutate values in place, and instead return a modified copy.

```mad
import List from "List"
import IO from "IO"

list = ["a", "b", "c"]
list2 = List.append("d", list)
IO.pTrace("Note that list is unchanged!", #[list, list2])
```

## Explicit Mutation

If you declare a value (`x = 5`) and then try to re-assign the value (`x = 8`) this will result in an error. Values can only be mutated with the [[Explicitness|explicit]] mutation operator (`:=`) and only for non top-level values (i.e. within a function or scope). We added this feature somewhat recently for performance reasons, but most of the language is intended to be used _without_ this escape hatch.