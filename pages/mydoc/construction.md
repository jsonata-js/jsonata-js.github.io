---
title: Constructing output
sidebar: mydoc_sidebar
permalink: construction.html
folder: mydoc
---

So far, we have discovered how to extract values from a JSON document, and how to manipulate the data using numeric, string and other operators.  It is useful to be able to specify how this processed data is presented in the output.

## Array constructors
As previously observed, when a location path matches multiple values in the input document, these values are returned as an array.  The values might be objects or arrays, and as such will have their own structure, but the _matched values_ themselves are at the top level in the resultant array.

It is possible to build extra structure into the resultant array by specifying the construction of arrays (or [objects](#object-constructors)) within the location path expression.  At any point in a location path where a field reference is expected, a pair of square brackets `[]` can be inserted to specify that the results of the expression within those brackets should be contained within a new array in the output.  Commas are used to separate multiple expressions within the array constructor.

Array constructors can also be used within location paths for making multiple selections without the broad brush use of wildcards.

Examples:

|Expression | Output | Comments|
| ---------- | ------ |----|
| `Email.address` | `[ "fred.smith@my-work.com",`<br>`"fsmith@my-work.com",`<br>`"freddy@my-social.com",`<br>`"frederic.smith@very-serious.com" ]` | The four emails addresses are returned in a flat array
| `Email.[address]` | `[ [ "fred.smith@my-work.com",  "fsmith@my-work.com" ],`<br>`[ "freddy@my-social.com", "frederic.smith@very-serious.com" ] ]` | Each email object generates an array of addresses
| ``[Address, Other.`Alternative.Address`].City`` | `[ "Winchester", "London" ]` | Selects the `City` value of both <br>`Address` and `Alternative.Address` objects


## Object constructors
In a similar manner to the way arrays can be constructed, JSON objects can also be constructed in the output.  At any point in a location path where a field reference is expected, a pair of braces `{}` containing key/value pairs separated by commas, with each key and value separated by a colon: `{key1: value2, key2:value2}`.  The keys and values can either be literals or can be expressions. The key must either be a string or an expression that evaluates to a string.

When an object constructor follows an expression that selects multiple values, the object constructor will create a single object that contains a key/value pair for each of those context values.  If an array of objects is required (one for each context value), then the object constructor should immediately follow the dot '.' operator.

Examples:

|Expression | Output | Comments|
| ---------- | ------ |----|
| `Phone{type: number}` | `{ "home": "0203 544 1234", "office": "01962 001235", "mobile": "077 7700 1234" }` | One of the `office` numbers was lost because it had a duplicate key
| `Phone.{type: number}` | `[ { "home": "0203 544 1234" }, { "office": "01962 001234" }, { "office": "01962 001235" }, { "mobile": "077 7700 1234"  } ]` | Produces an array of objects

## JSON literals
The array and object constructors use the standard JSON syntax for JSON arrays and JSON objects.  In addition to this values of the other JSON data types can be entered into an expression using their native JSON syntax:
- strings - `"hello world"`
- numbers - `34.5`
- Booleans - `true` or `false`
- nulls - `null`
- objects - `{"key1": "value1", "key2": "value2"}`
- arrays - `["value1", "value2"]`

This means that any valid JSON document is also a valid expression.  This property allows you to use a JSON document as a template for the desired output, and then replace parts of it with expressions to insert data into the output from the input document.
