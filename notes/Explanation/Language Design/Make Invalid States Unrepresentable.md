---
aliases: 
tags:
  - language-design
---
Like other functional programming languages, Madlib attempts to add guide rails to make it harder to represent invalid states. Many operations are made safer by paving over invalid states with features like [[Maybe|Maybes]] so that developers can elide over states which they would otherwise then need to recover from.