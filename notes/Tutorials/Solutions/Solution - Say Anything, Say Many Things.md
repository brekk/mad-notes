See the original [[Say Anything, Say Many Things|challenge here]].

As with many things in Madlib, there are multiple ways to get the same results, but here's a basic solution which prints `"madlib says: hello world, hello there, hello developer !"` without changing our `say` implementation:

```mad
import IO from "IO"
import String from "String"



say :: String -> String -> String
say = (word, subject) => word ++ " " ++ subject

main = () => {
  pipe(
    map(say("hello")),
    String.join(", "),
    say($, "!"),
    say("madlib says:"),
    IO.putLine,
  )(["world", "there", "developer"])
}
```

If there's anything that you're unfamiliar with, you can see the [[Getting Started]] guide here.