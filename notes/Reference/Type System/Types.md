---
aliases: []
tags:
  - fundamentals
---
##### Synopsis
- Madlib has many types in its standard library (also called Prelude), including both literals and constructed data types.

## Literals

Here is a list of types that have a literal syntax:
```dataview
LIST L.text
FROM #literals
FLATTEN file.lists AS L
WHERE meta(L.section).subpath = "Synopsis"
SORT file.name
```
## Data Types

Here is a list of data types that are core to Madlib:
```dataview
LIST L.text
FROM #data-type 
FLATTEN file.lists AS L
WHERE meta(L.section).subpath = "Synopsis"
SORT file.name
```