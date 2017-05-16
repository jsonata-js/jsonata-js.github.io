---
title: Numeric aggregation functions
sidebar: mydoc_sidebar
permalink: aggregation-functions.html
folder: mydoc
---

## `$sum(array)`

Returns the arithmetic sum of an array of numbers.  It is an error if the input array contains an item which isn't a number.

__Example__  
`$sum([5,1,3,7,4])` => `20`

## `$max(array)`

Returns the maximum number in an array of numbers.  It is an error if the input array contains an item which isn't a number.

__Example__  
`$max([5,1,3,7,4])` => `7`

## `$min(array)`

Returns the minimum number in an array of numbers.  It is an error if the input array contains an item which isn't a number.

__Example__  
`$min([5,1,3,7,4])` => `1`

## `$average(array)`

Returns the mean value of an array of numbers.  It is an error if the input array contains an item which isn't a number.

__Example__  
`$average([5,1,3,7,4])` => `4`


