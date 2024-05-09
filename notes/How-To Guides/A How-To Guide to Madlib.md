---
tags:
  - development
  - guide
  - walkthrough
---
```dataview
LIST rows.item.text
FLATTEN file.lists as item
WHERE lower(meta(item.section).subpath) = "summary"
GROUP BY link(item.section, file.name) as myFile
SORT meta(myFile).display
```