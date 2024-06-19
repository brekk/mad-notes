---
aliases: 
tags:
  - fundamentals
  - syntax
---
##### Synopsis
- The Fence is a syntactic escape hatch which allows for raw JavaScript to be written in Madlib between `#-` ... `-#` pairs, at the cost of automatic inference.

**The Fence** is our built-in escape hatch. While Madlib has a powerful type and inference system, "fenced" code allows you to drop down to any native JS construct. However, doing so will eschew any of the automatic type inference and instead treat code within the **The Fence** as opaque.

It has been a core feature of Madlib ideology from the very start of this project, as it can (if used _judiciously_) allow developers to build on top of existing JavaScript projects or allow for incremental migration to Madlib [[Coming From JavaScript|from JavaScript]].

However, there are some specific edges which can make interop between fenced JS code and native Madlib code inconvenient. This may change in the future, but the original intention was to provide an escape-hatch rather than a permanent bridge.

## Fence targets

When using fenced code, developers can segment it to different compilation targets, specifically for use in either the browser or in Node.

Here's an example of code written within in The Fence, and a Madlib wrapped function (`Argv`) which is made available for Node code but not the browser.

```madlib
#-
const makeArgs = () => {
  let list = {}
  let start = list
  Object.keys(process.argv.slice(0)).forEach((key) => {
    list = list.n = { v: process.argv[key], n: null }
  }, {})
  return {
    n: start.n.n.n,
    v: start.n.n.v
  }
}
-#


Argv :: List String
export Argv = #-
{Node}
  makeArgs()
{/Node}
{Browser}
  null
{/Browser}
-#
```



