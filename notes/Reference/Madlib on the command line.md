---
tags:
  - cli
---

```dataview
LIST L.text
FROM "Reference/CLI"
FLATTEN file.lists AS L
WHERE meta(L.section).subpath = "Synopsis"
SORT file.name
```