---
tags:
  - fundamentals
---
```dataview
LIST L.text
FROM #fundamentals AND !#literals
FLATTEN file.lists AS L
WHERE meta(L.section).subpath = "Synopsis"
SORT file.name
```