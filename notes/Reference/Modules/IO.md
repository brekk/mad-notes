---
aliases: []
tags:
  - module
---
##### Synopsis
- The IO module provides interfaces for inputs and outputs, both reading stdin and writing to stdout.

## Example

```mad
import IO from "IO"

main = () => {
  IO.put("this is a sentence with no trailing newline")
  IO.putLine("this is a sentence with a trailing newline")
  pipe(
    (x) => x + 5,
    IO.trace("two plus five"),
    (x) => ({x}),
    IO.pTrace("pretty trace the record with x in it!")
  )(2)
}
```


