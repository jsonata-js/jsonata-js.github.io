---
title: Basic Selection
sidebar: mydoc_sidebar
permalink: basic.html
folder: mydoc
---

To support the extraction of values from a JSON structure, a location path syntax is defined.
In common with XPath, this will select all possible values in the document that match the
specified location path.  The two structural constructs of JSON are objects and arrays.

## Navigating JSON Objects
A JSON object is an associative array (a.k.a map or hash).
The location path syntax to navigate into an arbitrarily deeply nested structure of
JSON objects comprises the field names separated by dot '.' delimiters.
The expression returns the JSON value referenced after navigating to the last step in the
location path.  If during the navigation of the location path, a field is not found, then
the expression returns nothing (represented by Javascript _undefined_).  No errors are thrown
as a result of non-existing data in the input document.

The following sample JSON document is used by examples throughout this guide, unless otherwise indicated:
```
{
  "FirstName": "Fred",
  "Surname": "Smith",
  "Age": 28,
  "Address": {
    "Street": "Hursley Park",
    "City": "Winchester",
    "Postcode": "SO21 2JN"
  },
  "Phone": [
    {
      "type": "home",
      "number": "0203 544 1234"
    },
    {
      "type": "office",
      "number": "01962 001234"
    },
    {
      "type": "office",
      "number": "01962 001235"
    },
    {
      "type": "mobile",
      "number": "077 7700 1234"
    }
  ],
  "Email": [
    {
      "type": "work",
      "address": ["fred.smith@my-work.com", "fsmith@my-work.com"]
    },
    {
      "type": "home",
      "address": ["freddy@my-social.com", "frederic.smith@very-serious.com"]
    }
  ],
  "Other": {
    "Over 18 ?": true,
    "Misc": null,
    "Alternative.Address": {
      "Street": "Brick Lane",
      "City": "London",
      "Postcode": "E1 6RF"
    }
  }
}
```

The following expressions yield the following results when applied to this JSON document:

|Expression | Output | Comments|
| ---------- | ------ |----|
| `Surname` | `"Smith"` | Returns a JSON string ("double quoted")|
|`Age` |`28`| Returns a JSON number
|`Address.City`|`"Winchester"`| Field references separated by '.'
|`Other.Misc`|`null`|Matched the path and returns the null value
|`Other.Nothing`| |Path not found.  Returns Javascript _undefined_
|`Other.'Over 18 ?'`|`true`|Field references containing whitespace or reserved tokens <br>can be enclosed in quotes (single or double)



## Navigating JSON Arrays
JSON arrays are used when an ordered collection of values is required.  
Each value in the array is associated with an index (position) rather than a name, so in order to address
individual values in an array, extra syntax is required to specify the index.
This is done using square brackets after the field name of the array.  If the square brackets contains
a number, or an expression that evaluates to a number, then the number represents the index of the value
to select.  Indexes are zero offset, i.e. the first value in an array `arr` is `arr[0]`.  If the number is not an
integer, then it is rounded _down_ to an integer.  If the expression in square brackets is non-numeric, or is an expression
that doesn't evaluate to a number, then it is treated as a [predicate](#predicates).

Negative indexes count from the end of the array, for example, `arr[-1]` will select the last value, `arr[-2]` the second to last, etc.
If an index is specified that exceeds the size of the array, then nothing is selected.

If no index is specified for an array (i.e. no square brackets after the field reference), then the whole array is selected.
If the array contains objects, and the location path selects fields within these objects, then each object within the
array will be queried for selection.

| Expression | Output | Comments|
| ---------- | ------ |----|
| `Phone[0]` | `{ "type": "home", "number": "0203 544 1234" }` | Returns the first item (an object)
| `Phone[1]` | `{ "type": "office", "number": "01962 001234" }` | Returns the second item
| `Phone[-1]` | `{ "type": "mobile", "number": "077 7700 1234" }` | Returns the last item
| `Phone[-2]` | `{ "type": "office", "number": "01962 001235" }` | Negative indexed count from the end
| `Phone[8]` |  | Doesn't exist - returns nothing
| `Phone[0].number`| `"0203 544 1234"`| Selects the `number` field in the first item
| `Phone.number`| `[ "0203 544 1234", "01962 001234", "01962 001235", "077 7700 1234" ]`| No index is given to `Phone` so it selects all of <br>them (the whole array), then it selects all the `number` fields for each of them
| `Phone.number[0]`| `[ "0203 544 1234", "01962 001234", "01962 001235", "077 7700 1234" ]`| Might expect it to just return the first number, <br>but it returns the first number of each of the items selected by `Phone`
| `(Phone.number)[0]`| `"0203 544 1234"`| Applies the index to the array returned by `Phone.number`. One use of [parentheses](#parenthesized-expressions-and-blocks).

### Top level arrays, nested arrays and array flattening
Consider the JSON document:
```
[
  { "ref": [ 1,2 ] },
  { "ref": [ 3,4 ] }
]
```
At the top level, we have an array rather than an object.  If we want to select the first object in this top level array,
we don't have a field name to append the `[0]` to.  We can't use `[0]` on its own because that clashes with the
[array constructor](#array-constructors) syntax.  However, we can use the _context_ reference `$` to refer to the start of
the document as follows:

| Expression | Output | Comments|
| ---------- | ------ |----|
| `$[0]` | `{ "ref": [ 1,2 ] }` | `$` at the start of an expression refers to the entire input document
| `$[0].ref` | `[ 1,2 ]` | `.ref` here returns the entire internal array
| `$[0].ref[0]` | `1` | returns element on first position of the internal array
| `$.ref` | `[ 1, 2, 3, 4 ]` | Despite the structure of the nested array, the resultant selection<br>is flattened into a single flat array.  The original nested structure<br>of the input arrays is lost. See [Array constructors](#array-constructors) for how to<br>maintain the original structure in the results.

