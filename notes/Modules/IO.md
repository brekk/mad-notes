---
aliases:
  - IO
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

## Show

When working with IO, you may need to call `show`, a built-in function that coerces values into strings in order to pass a value to one of the IO functions.

```
import IO from "IO"

main = () => {
  IO.put(12345) // this results in an "Instance not found" error
}
```

This can be fixed by changing the logging line to: `IO.put(show(12345))`