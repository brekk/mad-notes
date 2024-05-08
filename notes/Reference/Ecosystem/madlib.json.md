## "main" field

The `"main"` field tells downstream packages which file to access when being `import`ed as a library. For example, if you had a package called `sunglasses`, you could then access its main export via `import Lib from "sunglasses"`. This is a shorthand for: `import Lib from "sunglasses/path/to/main/field.mad"`

## "bin" field

The `"bin"` field tells downstream packages which file to access when being called as an executable. This means that they can be called via `madlib run`. For example, if you had a package called `nice` which had an executable, in a downstream module that used `nice` as a dependency, you could then `madlib run nice`.
