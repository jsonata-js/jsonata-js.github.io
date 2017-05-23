---
title: Object functions
sidebar: mydoc_sidebar
permalink: object-functions.html
folder: mydoc
---

## `$keys(object)`

Returns an array containing the keys in the object.  If the argument is an array of objects, then the array returned contains a de-duplicated list of all the keys in all of the objects.

## `$lookup(object, key)`

Returns the value associated with `key` in `object`. If the first argument is an array of objects, then all of the objects in the array are searched, and the values associated with all occurrences of `key` are returned.
  

## `$spread(object)`

Splits an `object` containing key/value pairs into an array of objects, each of which has a single key/value pair from the input `object`.  If the parameter is an array of objects, then the resultant array contains an object for every key/value pair in every object in the supplied array.

## `$sift(object, function)`

See definition under 'Higher-order functions'

## `$each(object, function)`

Returns an array containing the values return by the `function` when applied to each key/value pair in the `object`.


