---
title: Programming constructs
sidebar: mydoc_sidebar
permalink: programming.html
folder: mydoc
---

So far, we have introduced all the parts of the language that allow us to extract data from an input JSON document, combine the data using string and numeric operators, and format the structure of the output JSON document.  What follows are the parts that turn this into a Turing complete, functional programming language.

## Conditional expressions
If/then/else constructs can be written using the ternary operator "? :".
`predicate ? expr1 : expr2`

The expression `predicate` is evaluated.  If its effective boolean value (see definition) is `true` then `expr1` is evaluated and returned, otherwise `expr2` is evaluated and returned.

## Parenthesized expressions and blocks
Used to override the operator precedence rules.  E.g.
- `(5 + 3) * 4`

Used to compute complex expressions on a context value
- `Product.(Price * Quantity)` - both Price and Quantity are fields of the Product value

Used to support 'code blocks' - multiple expressions, separated by semicolons

`(expr1; expr2; expr3)`

Each expression in the block is evaluated _in sequential order_; the result of the last expression is returned from the block.

## Variables
Any name that starts with a dollar '$' is a variable.  A variable is a named reference to a value.  The value can be one of any type in the language's type system (link).

### Built-in variables

- `$` The variable with no name refers to the context value at any point in the input JSON hierarchy. Examples
- `$$` The root of the input JSON.  Only needed if you need to break out of the current context to temporarily navigate down a different path.  E.g. for cross-referencing or joining data. Examples
- Native (built-in) functions.  See function library.

### Variable assignment
Values (of any type in the type system) can be assigned to variables

`$var_name := "value"`

The stored value can be later referenced using the expression `$var_name`.

The scope of a variable is limited to the 'block' in which it was assigned.  E.g.

```
Invoice.(
  $p := Product.Price;
  $q := Product.Quantity;
  $p * $q
)
```
Returns Price multiplied by Quantity for the Product in the Invoice.

## Functions
The function is a first-class type, and can be stored in a variable just like any other data type.  A library of built-in functions is provided (link) and assigned to variables in the global scope.  For example, `$uppercase` contains a function which, when invoked with a string argument, `str`, will return a string with all the characters in `str` changed to uppercase.

### Invoking a function
A function is invoked by following its reference (or definition) by parentheses containing a comma delimited sequence of arguments. Examples:

- `$uppercase("Hello")` returns the string "HELLO".
- `$substring("hello world", 0, 5)` returns the string "hello"
- `$sum([1,2,3])` returns the number 6

### Defining a function
Anonymous (lambda) functions can be defined using the following syntax:

`function($l, $w, $h){ $l * $w * $h }`

and can be invoked using

`function($l, $w, $h){ $l * $w * $h }(10, 10, 5)` which returns 500

The function can also be assigned to a variable for future use (within the block)

```
(
  $volume := function($l, $w, $h){ $l * $w * $h };
  $volume(10, 10, 5);
)
```

### Recursive functions
Functions that have been assigned to variables can invoke themselves using that variable reference.  This allows recursive functions to be defined.  Eg.

```
(
  $factorial:= function($x){ $x <= 1 ? 1 : $x * $factorial($x-1) };
  $factorial(4)
)
```
Note that it is actually possible to write a recursive function using purely anonymous functions (i.e. nothing gets assigned to variables).  This is done using the [Y-combinator](https://en.wikipedia.org/wiki/Fixed-point_combinator#Fixed_point_combinators_in_lambda_calculus) which might be an interesting [diversion](#advanced-stuff) for those interested in functional programming.


### Higher order functions
A function, being a first-class data type, can be passed as a parameter to another function, or returned from a function.  Functions that process other functions are known as higher order functions.  Consider the following example:

```
(
  $twice := function($f) { function($x){ $f($f($x)) } };
  $add3 := function($y){ $y + 3 };
  $add6 := $twice($add3);
  $add6(7)
)
```
- The function stored in variable `$twice` is a higher order function.  It takes a parameter `$f` which is a function, and returns a function which takes a parameter `$x` which, when invoked, applies the function `$f` twice to `$x`.
- `$add3` stores a function that adds 3 to its argument.  Neither `$twice` or `$add3` have been invoked yet.
- `$twice` is invoked by passing the function `add3` as its argument.  This returns a function that applies `$add3` twice to _its_ argument.  This returned function is not invoked yet, but rather assigned to the variable `add6`.
- Finally the function in `$add6` is invoked with the argument 7, resulting in 3 being added to it twice.  It returns 13.

### Functions are closures
When a lambda function is defined, the evaluation engine takes a snapshot of the environment and stores it with the function body definition.  The environment comprises the context item (i.e. the current value in the location path) together with the current in-scope variable bindings.  When the lambda function is later invoked, it is done so in that stored environment rather than the current environment at invocation time.  This property is known as _lexical scoping_ and is a fundamental property of _closures_.

Consider the following example:
```
Account.(
  $AccName := function() { $.'Account Name' };

  Order[OrderID = 'order104'].Product.{
    'Account': $AccName(),
    'SKU-' & $string(ProductID): $.'Product Name'
  }
)
```

When the function is created, the context item (referred to by '$') is the value of `Account`.  Later, when the function is invoked, the context item has moved down the structure to the value of each `Product` item.  However, the function body is invoked in the environment that was stored when it was defined, so its context item is the value of `Account`.  This is a somewhat contrived example, you wouldn't really need a function to do this.  The expression produces the following result:

```
{
  "Account": "Firefly",
  "SKU-858383": "Bowler Hat",
  "SKU-345664": "Cloak"
}
```

### Advanced stuff
There is no need to read this section - it will do nothing for your sanity or ability to manipulate JSON data.

Earlier we learned how to write a recursive function to calculate the factorial of a number and hinted that this could be done without naming any functions.  We can take higher-order functions to the extreme and write the following:

`λ($f) { λ($x) { $x($x) }( λ($g) { $f( (λ($a) {$g($g)($a)}))})}(λ($f) { λ($n) { $n < 2 ? 1 : $n * $f($n - 1) } })(6)`

which produces the result `720`.  The Greek lambda (λ) symbol can be used in place of the word `function` which, if you can find it on your keyboard, will save screen space and please the fans of lambda calculus.

The first part of this above expression is an implementation of the [Y-combinator](https://en.wikipedia.org/wiki/Fixed-point_combinator#Fixed_point_combinators_in_lambda_calculus) in this language.  We could assign it to a variable and apply it to other recursive anonymous functions:

```
(
  $Y := λ($f) { λ($x) { $x($x) }( λ($g) { $f( (λ($a) {$g($g)($a)}))})};
  [1,2,3,4,5,6,7,8,9] . $Y(λ($f) { λ($n) { $n <= 1 ? $n : $f($n-1) + $f($n-2) } }) ($)
)
```
to produce the Fibonacci series `[ 1, 1, 2, 3, 5, 8, 13, 21, 34 ]`.

But we don't need to do any of this.  Far more sensible to use named functions:

```
(
  $fib := λ($n) { $n <= 1 ? $n : $fib($n-1) + $fib($n-2) };
  [1,2,3,4,5,6,7,8,9] . $fib($)
)
```

