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

If `arg` is not specified (i.e. this function is invoked with no arguments), then the context value is used as the value of `arg`. 

__Examples__  
`$number("5")` => `5`  
`["1", "2", "3", "4", "5"].$number()` => `[1, 2, 3, 4, 5]`


## `$abs(number)`

Returns the absolute value of the `number` parameter, i.e. if the number is negative, it returns the positive value.

If `number` is not specified (i.e. this function is invoked with no arguments), then the context value is used as the value of `number`. 

__Examples__  
`$abs(5)` => `5`  
`$abs(-5)` => `-5`

## `$floor(number)`

Returns the value of `number` rounded down to the nearest integer that is smaller or equal to `number`. 

If `number` is not specified (i.e. this function is invoked with no arguments), then the context value is used as the value of `number`. 

__Examples__  
`$floor(5)` => `5`  
`$floor(5.3)` => `5`  
`$floor(5.8)` => `5`  
`$floor(-5.3)` => `-6`  


## `$ceil(number)`

Returns the value of `number` rounded up to the nearest integer that is greater than or equal to `number`. 

If `number` is not specified (i.e. this function is invoked with no arguments), then the context value is used as the value of `number`. 

__Examples__  
`$ceil(5)` => `5`  
`$ceil(5.3)` => `6`  
`$ceil(5.8)` => `6`  
`$ceil(-5.3)` => `-5`  


## `$round(number [, precision])`

Returns the value of the `number` parameter rounded to the number of decimal places specified by the optional `precision` parameter.  

The `precision` parameter (which must be an integer) species the number of decimal places to be present in the rounded number.   If `precision` is not specified then it defaults to the value `0` and the number is rounded to the nearest integer.  If `precision` is negative, then its value specifies which column to round to on the left side of the decimal place

This function uses the [Round half to even](https://en.wikipedia.org/wiki/Rounding#Round_half_to_even) strategy to decide which way to round numbers that fall exactly between two candidates at the specified precision.  This strategy is commonly used in financial calculations and is the default rounding mode in IEEE 754.

__Examples__  
`$round(123.456)` => `123`  
`$round(123.456, 2)` => `123.46`  
`$round(123.456, -1)` => `120`  
`$round(123.456, -2)` => `100`  
`$round(11.5)` => `12`  
`$round(12.5)` => `12`  
`$round(125, -1)` => `120`

## `$power(base, exponent)`

Returns the value of `base` raised to the power of `exponent` (<code>base<sup>exponent</sup></code>).

If `base` is not specified (i.e. this function is invoked with one argument), then the context value is used as the value of `base`. 

An error is thrown if the values of `base` and `exponent` lead to a value that cannot be represented as a JSON number (e.g. Infinity, complex numbers).

__Examples__  
`$power(2, 8)` => `8`  
`$power(2, 0.5)` => `1.414213562373`  
`$power(2, -2)` => `0.25`  

## `$sqrt(number)`

Returns the square root of the value of the `number` parameter.

If `number` is not specified (i.e. this function is invoked with one argument), then the context value is used as the value of `number`. 

An error is thrown if the value of `number` is negative.

__Examples__  
`$sqrt(4)` => `2`
`$sqrt(2)` => `1.414213562373`  

## `$random()`

Returns a pseudo random number greater than or equal to zero and less than one (<code>0 &#8804; n < 1</code>) 

__Examples__  
`$random()` => `0.7973541067127`  
`$random()` => `0.4029142127028`  
`$random()` => `0.6558078550072`  

## `$millis()`

Returns the number of milliseconds since the Unix *Epoch* (1 January, 1970 UTC) as a number.  All invocations of `$millis()` within an evaluation of an expression will all return the same value

__Examples__  
`$millis()` => `1502700297574`  
