Madlib is a language of intentional compromise. It's designed to be integrated with (currently) two external languages: JavaScript and LLVM.  There are many possible reasons to use these foreign language interfaces, including code performance, developer comfort or simple convenience. To that purpose, there are several syntactic affordances which allow developers to segment applications to specific targets and use-cases.

## Compilation Targets

Developers can use the `#iftarget` syntax to segment foreign language code. This allows you to write the same [[Modules#Exports|exported interface]] across one or more foreign function interfaces, so that code can remain portable but be leveraged in different ways. This pairs specifically with the [[madlib build]] command's [[target]] flag.

The `#iftarget` syntax can be used to segment code as follows:

```madlib
#iftarget js

exit :: Integer -> {}
export exit = (code) => #- {
{Node}
  process.exit(code)
{/Node}
} -#

#elseif llvm

exit :: Integer -> {}
export exit = extern "exit"

#endif
```

There's a few things happening here, so let's break them down.

## iftarget syntax

Firstly, the `#iftarget` / `#elseif` / `#endif` pattern allows for conditional segmentation of targets, (currently) between LLVM code and JS code. This can be used for targeting a single foreign language (in this case JavaScript):

```madlib
#iftarget js
// JS code goes here
#endif
```

Or it can be used to target multiple (both) foreign languages:

```madlib
#iftarget llvm
// llvm code headers go here
#elseif js
// JS code goes here
#endif
```

## JavaScript

### The Fence

The Fence is the name of our syntactic construct to allow for raw JavaScript code to be written inline. It is (somewhat intentionally) not a perfect bridge between the two languages, but rather an escape-hatch.

Learn more about The Fence in detail [[The Fence|here]].

As in the first example above, fenced JS code is best served with a Madlib type signature (because the contents of the fenced code is opaque to the compiler):

```madlib
badRandom :: {} -> Float
export badRandom = () => #- Math.random() -#
```

### Fence Segments

Within the Fence, there is a secondary division that is possible to delineate between JavaScript code intended for [Node](https://nodejs.org/en) vs. the browser.

As in the first example above, this involves wrapping fenced code with either `{Node}` / `{/Node}` or `{Browser}` / `{/Browser}` pairs:

```madlib
exit :: Integer -> {}
export exit = (code) => #- {
{Node}
  process.exit(code)
{/Node}
} -#
```

The above function targets _only_ Node compilation targets. Since there isn't an equivalent in the browser, that code has been eschewed.

Here's an example taken from [[Prelude]]'s [[IO]] module:

```madlib
put :: String -> {}
export put = (a) => #- {
  {Node}
    process.stdout.write(a)
  {/Node}
  {Browser}
    console.log(a)
  {/Browser}
} -#
```

The above function allows for both Node and browser compilation targets.
## LLVM

LLVM code has slightly more syntactic baggage than JS code, so we've chosen to segment it with a more explicit bridge than Fenced code.

### extern syntax

Syntactically LLVM code is referenced by the `extern` keyword:

```madlib
#iftarget llvm

exit :: Integer -> {}
export exit = extern "exit"

#endif
```

Since the [exit](https://en.cppreference.com/w/c/program/exit) function is globally available the standard library, there's no additional context / files needed to make it work.

Additionally, unlike native Madlib functions, since the compiler cannot infer type and context from these functions, they need an explicit type signature to guide the compiler.
### Building linked libraries

However, in most cases you'll want to write your own LLVM code and link your Madlib code to it. In order to facilitate this, the compiler will need to be told which compiled builds are needed in the [[madlib.json]] file:

```json
"staticLibs": [ "./lib/build/libpickaxe.a" ]
```

