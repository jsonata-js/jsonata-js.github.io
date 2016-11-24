---
title: Numeric functions
sidebar: mydoc_sidebar
permalink: numeric-functions.html
folder: mydoc
---

## `$number(arg)`

Casts the `arg` parameter to a number using the following casting rules
   - Numbers are unchanged
   - Strings that contain a sequence of characters that represent a legal JSON number are converted to that number
   - All other values cause an error to be thrown.

## `$abs(number)` - to be implemented
## `$round(number)` - to be implemented

Rounds up to the nearest integer

## `$roundHalfToEven(number)` - to be implemented

  [Round half to even](https://en.wikipedia.org/wiki/Rounding#Round_half_to_even) Commonly used in financial calculations.

## `$power(base, exponent)` - to be implemented

