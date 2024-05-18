---
tags:
  - challenge
---
Given the code sample [[01 - Hello mad, mad world|we started with]] (and taking [[String]] from Prelude), how can we change our `main` function so that we print this sentence: `"madlib says: hello world, hello there, hello developer !"` _without_ changing our `say` implementation and only changing our piped composition in `main`?

```mad
import IO from "IO"
import String from "String"

say :: String -> String -> String
say = (word, subject) => word ++ " " ++ subject

main = () => {
  pipe(
    say("hello"),
    IO.putLine
  )("world")
}
```

Hint: You can use another `IO` function, `trace`, to add logging as we step through the contents of the pipe:

```mad
pipe(
  IO.trace("in"),
  Math.max(100),
  IO.trace("biggest"),
  (x) => x * 2,
  IO.trace("doubled"),
)(10)
```

See [[Solution - Say Anything, Say Many Things|the solution]] here.