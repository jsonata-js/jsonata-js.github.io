---
title: String functions
sidebar: mydoc_sidebar
permalink: string-functions.html
folder: mydoc
---

## `$string(arg)`

Casts the `arg` parameter to a string using the following casting rules
   - Strings are unchanged
   - Functions are converted to an empty string
   - Numeric infinity and NaN throw an error because they cannot be represented as a JSON number
   - All other values are converted to a JSON string using the [JSON.stringify](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) function
   
## `$length(str)`

Returns the number of characters in the string `str`.  An error is thrown if `str` is not a string.
   
## `$substring(str, start[, length])`

Returns a string containing the characters in the first parameter `str` starting at position `start` (zero-offset).  If `length` is specified, then the substring will contain maximum `length` characters.  If `start` is negative then it indicates the number of characters from the end of `str`.  See [substr](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substr) for full definition.

## `$substringBefore(str, chars)`

Returns the substring before the first occurrence of the character sequence `chars` in `str`.  If `str` does not contain `chars`, then it returns `str`.

## `$substringAfter(str, chars)`

Returns the substring after the first occurrence of the character sequence `chars` in `str`.  If `str` does not contain `chars`, then it returns `str`.


## `$uppercase(str)`

Returns a string with all the characters of `str` converted to uppercase.


## `$lowercase(str)`

Returns a string with all the characters of `str` converted to lowercase.


## `$split(str, separator, limit)`

Splits the `str` parameter into an array of substrings.  It is an error if `str` is not a string.

The optional `separator` parameter specifies the characters within the `str` about which it should be split. If `separator` is not specified, then the empty string is assumed, and `str` will be split into an array of single characters.  It is an error if `separator` is not a string.

The optional `limit` parameter is a number that specifies the maximum number of substrings to  include in the resultant array.  Any additional substrings are discarded.  If `limit` is not  specified, then `str` is fully split with no limit to the size of the resultant array.  It is an error if `limit` is not a non-negative number. 
 
## `$join(array[, separator])`

Joins an array of component strings into a single concatenated string with each component string separated by the optional `separator` parameter.

It is an error if the input array contains an item which isn't a string.

If `separator` is not specified, then it is assumed to be the empty string, i.e. no separator between the component strings.  It is an error if `separator` is not a string.

