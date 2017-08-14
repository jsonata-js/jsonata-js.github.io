---
title: Combining values
sidebar: mydoc_sidebar
permalink: combining.html
folder: mydoc
---

## String expressions

Path expressions that point to a string value will return that value.  Strings can be combined using the concatenation operator '&'

Examples

| Expression | Output | Comments|
| ---------- | ------ |----|
| <code>FirstName & ' ' & Surname</code> | `"Fred Smith"` | Concatenate `FirstName` followed by space <br>followed by `Surname`
| <code>Address.(Street & ', ' & City)</code> | `"Hursley Park, Winchester"` | Another nice use of [parentheses](#parenthesized-expressions-and-blocks)

Consider the following JSON document:
``` 
{
  "Numbers": [1, 2.4, 3.5, 10, 20.9, 30]
}
```


## Numeric expressions

Path expressions that point to a number value will return that value.  Numbers can be combined using the usual mathematical operators to produce a resulting number.  Supported operators:
- `+` addition
- `-` subtraction
- `*` multiplication
- `/` division
- `%` remainder (modulo)

_Examples_

| Expression | Output | Comments
| ---------- | ------ |----|
| `Numbers[0] + Numbers[1]` | 3.4 |Adding 2 prices|
| `Numbers[0] - Numbers[4]` | -19.9 | Subtraction |
| `Numbers[0] * Numbers[5]` | 30 |Multiplying price by quantity|
| `Numbers[0] / Numbers[4]` | 0.04784688995215 |Division|
| `Numbers[2] % Numbers[5]` | 3.5 |Modulo operator|


## Comparison expressions

Often used in predicates, for comparison of two values.  Returns Boolean `true` or `false`. Supported operators:

- `=` equals
- `!=` not equals
- `<` less than
- `<=` less than or equal
- `>` greater than
- `>=` greater than or equal
- `in` value is contained in an array


_Examples_

| Expression | Output | Comments
| ---------- | ------ |----|
| `Numbers[0] = Numbers[5]` | false |Equality |
| `Numbers[0] != Numbers[4]` | true | Inequality |
| `Numbers[1] < Numbers[5]` | true |Less than|
| `Numbers[1] <= Numbers[5]` | true |Less than or equal|
| `Numbers[2] > Numbers[4]` | false |Greater than|
| `Numbers[2] >= Numbers[4]` | false |Greater than or equal|
| `"01962 001234" in Phone.number` | true | Value is contained in|

## Boolean expressions

Used to combine Boolean results, often to support more sophisticated predicate expressions. Supported operators:

- `and`
- `or`

Note that `not` is supported as a function, not an operator.

_Examples_

| Expression | Output | Comments
| ---------- | ------ |----|
| `(Numbers[2] != 0) and (Numbers[5] != Numbers[1])` | true |`and` operator |
| `(Numbers[2] != 0) or (Numbers[5] = Numbers[1])` | true | `or` operator |

