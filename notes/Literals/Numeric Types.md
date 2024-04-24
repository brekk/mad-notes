---
aliases:
  - Number
  - Integer
  - Byte
  - Short
  - Float
tags:
  - literals
  - fundamentals
---
##### Synopsis
- Madlib has several literal types that represent numeric values, they can be one of the following: Integers, Bytes, Shorts or Floats

When needed in functions which can accept generic numbers, remember to declare the [constraints](app://obsidian.md/Core%20-%20Functions#Constraints).

## Integer

Signed whole numbers which can be negative. These can be expressed as digits with or without a `_i` suffix.

## Byte

Positive whole numbers between 0 and 255. These can be expressed with or without an explicit `_b` suffix.

## Short

Signed whole numbers between -2,147,483,647 and 2,147,483,647. These can be expressed with or without an explicit `_s` suffix.

## Float

Signed floating point numbers, which can be expressed with or without an explicit `_f` suffix.