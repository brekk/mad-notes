```dataview
LIST L.text
FROM "Fundamentals"
FLATTEN file.lists AS L
WHERE meta(L.section).subpath = "Synopsis"
SORT file.name
```