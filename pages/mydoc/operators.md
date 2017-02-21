---
title: Operators
sidebar: mydoc_sidebar
permalink: operators.html
folder: mydoc
---

Operators are a set of symbols that implement commonly used functions and are easier to read that an equivalent notation that just used function syntax.  Most operators act on two values, placed on the left-hand side (LHS) and right-hand side (RHS) of the operator.  Expressions containing operators can be ready chained, the order in which they are evaluated is determined by their relative precedence.

## `.` (dot)

The dot operator is one of the fundamental building blocks in JSONata expressions.  It implements the 'for each' or 'map' function that is common in many functional languages.

The dot operator performs the following logic:

- The expression on the LHS is evaluated to produce an array of values.
      - If it evaluates to a single value, that is treated as equivalent to an array containing that single value
      - If it evaluates to nothing (no match or empty array), then the result of the operator expression is nothing
- For each value in the LHS array in turn:
      - The value is known as the _context_ and is used as the basis for any relative path expression on the RHS.  It is also accessible in the RHS expression using the `$` symbol.
      - The RHS expression is evaluated to produce a value or array of values (or nothing).  These values are appended to a combined array of results for the operator as a whole.
- The combined result of the operator is returned.

This operator is left associative meaning that the expression `a.b.c.d` is evaluated like `((a.b).c).d`; i.e. left to right

__Examples__

`Address.City` => `"Winchester"`
`Phone.number` => `[ "0203 544 1234", "01962 001234", "01962 001235", "077 7700 1234" ]`
`Account.Order.Product.(Price * Quantity)` => `[ 68.9, 21.67, 137.8, 107.99 ]`
`Account.Order.OrderID.$uppercase()` => `[ "ORDER103", "ORDER104"]`

## `~>` 

The function chaining operator is used in the situations where multiple nested functions need to be applied to a value, while making it easy to read. The value on the LHS is evaluated, then passed into the function on the RHS as its first argument.  If the function has any other arguments, then these are passed to the function in parenthesis as usual.  It is an error if the RHS is not a function, or an expression that evaluates to a function.

__Examples__

`$uppercase($substringBefore($substringAfter(Customer.Email, "@"), "."))`
and
`$sum(Account.Order.Product.(Price * Quantity))`

can be more clearly written:

`Customer.Email ~> $substringAfter("@") ~> $substringBefore(".") ~> $uppercase()`
and
`Account.Order.Product.(Price * Quantity) ~> $sum()`

This operator can also be used in a more abstract form to define new functions based on a combination of existing functions.  In this form, there is no value passed in on the LHS of the first function in the chain.

For example, the expression
```
(
  $uppertrim := $trim ~> $uppercase;
  $uppertrim("   Hello    World   ")
)
```
=> `"HELLO WORLD"`
creates a new function `$uppertrim` that performs `$trim` followed by `$uppercase`.


## `&` 

The string concatenation operator is used to join the string values of the operands into a single resultant string.  If either or both of the operands are not strings, then they are first cast to string using the rules of the `$string` function.

__Example__

`"Hello" & "World"` => `"HelloWorld"`

## `+` 

The addition operator adds the operands to produce the numerical sum.  It is an error if either operand is not a number.

__Example__

`5 + 2` => `7`


## `-` 

The subtraction operator subtracts the RHS value from the LHS value to produce the numerical difference  It is an error if either operand is not a number.

It can also be used in its unary form to negate a number

__Examples__

`5 - 2` => `3`
`- 42` => `-42`

## `*` 

The multiplication operator multiplies the operands to produce the numerical product.  It is an error if either operand is not a number.

__Example__

`5 * 2` => `10`

## `/` 

The division operator divides the RHS into the LHS to produce the numerical quotient.  It is an error if either operand is not a number.

__Example__

`5 / 2` => `2.5`


## `%` 

The modulo operator divides the RHS into the LHS using whole number division to produce a whole number quotient and a remainder.  This operator returns the remainder.  It is an error if either operand is not a number.

__Example__

`5 / 2` => `1`

 
## `=`

The equality operator returns Boolean `true` if both operands are the same (type and value).  Otherwise it returns `false`.

__Examples__

`1+1 = 2` => `true`
`"Hello" = "World"` => `false`

## `!=` 

The equality operator returns Boolean `false` if both operands are the same (type and value).  Otherwise it returns `true`.

__Examples__

`1+1 = 2` => `true`
`"Hello" = "World"` => `false`

## `>` 

The 'greater than' operator returns Boolean `true` if the LHS is numerically greater than the RHS.  Otherwise it returns `false`.

__Examples__

`22 / 7 > 3` => `true`
`5 > 5` => `false`

## `<`

The 'less than' operator returns Boolean `true` if the LHS is numerically less than the RHS.  Otherwise it returns `false`.

__Examples__

`22 / 7 < 3` => `false`
`5 < 5` => `false`


## `>=`

The 'greater than or equals' operator returns Boolean `true` if the LHS is numerically greater than or equal to the RHS.  Otherwise it returns `false`.

__Examples__

`22 / 7 >= 3` => `true`
`5 >= 5` => `true`


## `<=` 

The 'less than or equals' operator returns Boolean `true` if the LHS is numerically less than or equal to the RHS.  Otherwise it returns `false`.

__Examples__

`22 / 7 <= 3` => `false`
`5 <= 5` => `true`


## `in`

The array inclusion operator returns Boolean `true` if the value of the LHS is included in the array of values on the RHS.  Otherwise it returns `false`.  If the RHS is a single value, then it is treated as a singleton array.

__Examples__
`"world" in ["hello", "world"]` => `true`
`"hello" in "hello"` => `true`
`library.books["Aho" in authors].title`


## `and`

The 'and' operator returns Boolean `true` if both operands evaluate to `true`.  If either or both operands is not a Boolean type, then they are first cast to a Boolean using the rules of the `$boolean` function.

__Example__
`library.books["Aho" in authors and price < 50].title`

## `or` 

The 'or' operator returns Boolean `true` if either operand evaluates to `true`.  If either or both operands is not a Boolean type, then they are first cast to a Boolean using the rules of the `$boolean` function.

__Example__
`library.books[price < 10 or section="diy"].title`

## `..`
The sequence generation operator is used to create an array of monotonically increasing integer start with the number on the LHS and ending with the number on the RHS.  It is an error if either operand does not evaluate to an integer.  The sequence generator can only be used within an array constructor [].

__Examples__

`[1..5]` => `[1, 2, 3, 4, 5]`
`[1..3, 7..9]` => `[1, 2, 3, 7, 8, 9]`
`[1..$count(Items)].("Item " & $)` => `["Item 1","Item 2","Item 3"]`
`[1..5].($*$)` => `[1, 4, 9, 16, 25]`


## `? :`

The conditional ternary operator is used to evaluate one of two alternative expressions based on the result of a predicate (test) condition.  The operator takes the form:

`<test_expr> ? <expr_T> : <expr_F>`

The `<test_expr>` expression is first evaluated.  If it evaluates to Boolean `true`, then the operator returns the result of evaluating the `<expr_T>` expression.  Otherwise it returns the result of evaluating the `<expr_F>` expression. If `<test_expr>` evaluates to a non-Boolean value, then the value is first cast to Boolean using the rules of the `$boolean` function.

__Example__
`Price < 50 ? "Cheap" : "Expensive"`
