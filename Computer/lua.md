---
title: Lua
---

# Basic Concepts

Three fundamental mechanisms:

-   Tables: Represent both maps and lists
-   Closures: Every scope is a closure, e.g. function, module, do block, etc.
-   Stackful coroutines enable cooperative multithreading, generators, and
    versatile control for both Lua and its host (Nvim).

# Basic Syntax

## Variable names and keywords

-   Any string of letters, digits and underscore
-   Not start with a digit.
-   Case-sensitive

Reserved keywords: and, break, do, else, elseif, end, false, for, function, if,
in, local, nil, not, or, repeat, return, then, true, until, while.

Also as a convention, names that start with an **underscored followed by
uppercase letters are reserved** for Lua's internal global variables.

## Other tokens

```
+     -     *     /     %     ^     #
==    ~=    <=    >=    <     >     =
(     )     {     }     [     ]
;     :     ,     .     ..    ...
```

## Literal strings

Use single `'` or double quotes `"`, both can contain:

-   `\a` bell
-   `\b` backspace
-   `\f` form feed
-   `\n` newline
-   `\r` carriage return
-   `\t` horizontal tab
-   `\v` vertical tab
-   `\\` backslash
-   `\"` quotation mark (double quote)
-   `\'` apostrophe (single quote)

### Long brackets

Literal strings can also be defined using a long format enclosed by long
brackets. We define an opening long bracket of level n as an opening square
bracket followed by n equal signs followed by another opening square bracket.
So, an opening long bracket of level 0 is written as `[[`, an opening long
bracket of level 1 is written as `[=[`, and so on. A closing long bracket is
defined similarly; for instance, a closing long bracket of level 4 is written as
`]====]`. A long string starts with an opening long bracket of any level and
ends at the first closing long bracket of the same level. Literals in this
bracketed form may run for several lines, do not interpret any escape sequences,
and ignore long brackets of any other level. They may contain anything except a
closing bracket of the proper level.

For convenience, when the opening long bracket is immediately followed by a
newline, the newline is not included in the string. As an example, in a system
using ASCII (in which `a` is coded as 97, newline is coded as 10, and `1` is
coded as 49), the five literals below denote the same string:

```lua
a = 'alo\n123"'
a = "alo\n123\""
a = '\97lo\10\04923"'
a = [[alo
123"]]
a = [==[
alo
123"]==]
```

## Numerical constants

-   3
-   3.0
-   3.1416
-   314.16e-2
-   0.31316E1
-   0xff
-   0x56

## Comments

`--` starts a comment until the end of line. Or use opening long brackets after
`--` to start a multiline comments until a corresponding closing long brackets.

# Values and Types

## Introduction

Lua is **dynamically typed**. There are no type definitions, only values carry
their own type.

All values are **first-class** values, i.e. can be stored in variables, passed
as arguments and returned as results.

## Basic types

### Eight basic types

-   nil
-   boolean
-   number
-   string
-   function
-   userdata
-   thread
-   table

### nil

The only value is `nil`, which represents absence of a useful value. It makes a
condition false.

### boolean

`true` and `false`.

Only `false` and `nil` make a condition false, any other make it true.

### number

Double precision floating-point.

Also see [## Numerical constants](#numerical-constants).

### string

Array of 8-bit characters, including `\0`. Use `..` to concatenate strings and
numbers (after inplicit type casting).

Also see [## Literal strings](#literal-strings).

### userdata

-   To store arbitrary C data.
-   It corresponds to a block of raw memory.
-   No predefined operation, except assignment and identity test.
-   Cannot be created or modified in Lua, but only through C API. This
    guarantees the integrity of data owned by the host program.

### thread

Represents independent threads of execution, and used to implement Lua
coroutine.

### table

#### Introduction to tables

-   Implements associative arrays. I.e. index not only with numbers but with
    value (except `nil`).
-   Both indices and values can be heterogeneous. i.e. contains data of
    different types (except `nil`).
    -   If a value is a function, then it works similar to a method.
-   The only data structuring mechanism, and used to represent ordinary arrays,
    symbol tables, sets, records, graphs, trees, etc.
-   Can use dot notation to represent bracket notation: `a.name` is the same as
    `a["name"]`.
-   Can be returned in a function as an object to return multiple values.

#### Defining and using tables

For formal description, see [#### Table constructors](#table-constructors).

For list like definition:

```lua
local colors = {"red", "green", "blue"}
```

Without giving the key, Lua by defaults assigns keys to values starting from
`1`. So `colors[1]` yields the value `"red"`.

For dictionary like definition:

```lua
local teams = {
    ["teamA"] = 12,
    ["teamB"] = 15
}
```

Now `teams["teamA"]` yields 12. Also recall that the values and keys can be
heterogeneous.

For iterating tables with for loops, see [## For statements](#for-statements)

#### Modifying tables

```lua
myTable[myKey] = myValue
```

If `myKey` already exists, then update its value with `myValue`. Otherwise,
insert a new key-value pair to the table.

Note that when `myValue` has the value `nil`, it **removes** the key from the
table instead:

```lua
teams["teamB"] = nil
```

#### Table methods

You can use following methods of `table` object, e.g.
`table.insert(myTable, newItem)`.

-   `insert(myTable, newValue)` appends a new value to the end of a table
-   `insert(myTable, idx, newValue)` inserts a new value at target idx, and push
    remaining existing values one idx back
-   `remove(myTable, key)` removes an item of that key from a table

#### Table constructors

```
tableconstructor ::= { [ fieldlist ] }
fieldlist ::= field { fieldsep field } [ fieldsep ]
field ::= [ exp ] = exp | Name = exp | exp
fieldsep ::=  , | ;
```

-   `name = exp` is equivalent to `["name"] = exp`.
-   `exp` is equivalent to `[i] = exp`, where `i` are consecutive numerical
    integers starting from `1`.
-   If the **last field** has the form of `exp` and it is a **function call**,
    then all returned values enter the field list consecutively.
    -   Unless the function call is enclosed by parentheses.
-   May have optional trailing separator.

### Object references

Tables, functions, threads and (full) userdata are objects. Variables contains
references to their values. So object assignment doesn't imply copy of values.

## Implicit type casting

There is implicit type casting between string and number.

# Variables

## Three kinds of variables

There are three kinds of variables:

-   global variables
-   local variables
-   table fields

Any variables is assumed to be global unless explicitly declared as local. Local
variables are **lexically scoped**: freely accessed by functions defined inside
their scope.

Before the first assignment to a variable, it's value is `nil`.

## Environment tables

All global variables live as fields in a Lua table called **_environment
tables_** or simply _environments_.

Each function has its own reference to an environment. All global variables in
this function will refer to this environment table, because when a function is
created, it inherits the environment from the parent.

-   `getfenv()` to get get the environment table of a Lua function.
-   `setfenv()` to replace it.

If `_env` is the environment of the running function, `x` is equivalent with
`_env.x`.

## Naming variables

See [## Variable names and keywords](#variable-names-and-keywords).

## Assignments

```
stat ::= varlist1 = explist1
varlist1 ::= var { , var }
explist1 ::= exp { , exp }
```

-   If explist1 is larger than varlist1, excesses expressions are thrown away.
-   If varlist1 is larger than explist1, explist1 is extended with `nil`s.
-   If explist1 ends with a **function call**, all return values append to
    explist1.
    -   Except when the call is enclosed by parentheses.

Lua evaluates all expressions (in both varlist1 and explist1) before assignment.

```lua
i = 3
i, a[i] = i+1, 20
```

In here, a[3] is changed whereas a[4] is untouched.

To exchange values of two variables, `x, y = y, x`.

## Local declaration

```
stat ::= local namelist [ = explist1 ]
namelist ::= Name { , Name }
```

# Statements

## Semicolons

Lua's statements are similar to C's.

-   Each statement optionally terminated by `;`.
-   No empty statement, i.e. no `;;`.

## Block

A _block_ is just a list of statements. It can be explicitly **delimited** to
produce a single statement:

```
stat ::= do block end
```

It is useful to:

-   Control the **scope** of variables
-   Use `return` and `break` in the middle of another block. See
    [## Return and break](#return-and-break).

# Operators

## Arithematic operators

-   `+`
-   `-`
-   `*`
-   `/`: Yields float format number
-   `//`: Yields integer format number
-   `%`
    -   Defined as `a % b == a - math.floor(a/b)*b`
-   `^`: Exponentiation

`-` can also be negative unary operator.

All arithematic operators can take both numbers and strings that can be type
casted to numbers.

## Relational operators

Always return true or false.

-   `==`: True only if both **type** and value are the same. (1 == "1" is false)
-   `~=`: True if either type or value is different.
-   `<`
-   `>`
-   `<=`
-   `>=`

Objects are compared by reference. They objects are equal if they are the same
object. For `<` and `>`, Lua tries to call "lt" or "le" metamethods.

## Logical operators

-   `and`: Return **1st argument** if 1st operator is false, otherwise return
    **2nd argument**.
-   `or`: Return **1st argument** if 1st operator is true, otherwise return
    **2nd argument**.
-   `not`: Always return true or false.

## Concatenation operator

`..` concatenate numbers and strings. Numbers are implicitly type casted to
strings first.

Metamethod "concat" is called when either or both aren't number or string.

## Length operator

Unary operator `#` returns:

-   String: number of bytes (i.e. characters)
-   Table: _any_ integer index `n` such that `t[n]` is not `nil` but `t[n+1]` is
    `nil`. If `t[1]` is `nil`, then _may_ return `0`.

## Operators precedence

From lower to higher priority:

1.  `or`
2.  `and`
3.  `<`, `>`, `<=`, `=>`, `~=`, `==`
4.  `..`
5.  `+`, `-`
6.  `*`, `/`
7.  `not`, `#`, `-` (unary)
8.  `^`

**Parenthese** can be used to change the precedence.

## Ternary operator

There is no `a ? b : c` syntax. But it can be approximated by `a and b : c`.
Note that they aren't exactly the same though, when `b` is false while `a` and
`c` are true.

# Expressions

```
exp ::= prefixexp
exp ::=  nil  |  false  |  true
exp ::= Number
exp ::= String
exp ::= function
exp ::= tableconstructor
exp ::= ...
exp ::= exp binop exp
exp ::= unop exp
prefixexp ::= var | functioncall | ( exp )
```

-   A prefixexp can be a variable referencing a table.
-   binop is a binary operator; unop is a unary operator.

## Multiple values in an expression

To quote NeoVim's documentation:

Both function calls and vararg expressions may result in multiple values. If the
expression is used as a statement (see |luaref-langFuncStat|) (only possible for
function calls), then its return list is adjusted to zero elements, thus
discarding all returned values. If the expression is used as the last (or the
only) element of a list of expressions, then no adjustment is made (unless the
call is enclosed in parentheses). In all other contexts, Lua adjusts the result
list to one element, discarding all values except the first one.

Here are some examples:

|                |                                                                                                                               |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| f()            | adjusted to 0 results                                                                                                         |
| g(f(), x)      | f() is adjusted to 1 result                                                                                                   |
| g(x, f())      | g gets x plus all results from f()                                                                                            |
| a,b,c = f(), x | f() is adjusted to 1 result (c gets nil)                                                                                      |
| a,b = ...      | a gets the first vararg parameter, b gets the second (both a and b may get nil if there is no corresponding vararg parameter) |

|                |                                           |
| -------------- | ----------------------------------------- |
| a,b,c = x, f() | f() is adjusted to 2 results              |
| a,b,c = f()    | f() is adjusted to 3 results              |
| return f()     | returns all results from f()              |
| return ...     | returns all received vararg parameters    |
| return x,y,f() | returns x, y, and all results from f()    |
| {f()}          | creates a list with all results from f()  |
| {...}          | creates a list with all vararg parameters |
| {f(), nil}     | f() is adjusted to 1 result               |

An expression enclosed in parentheses always results in only one value. Thus,
`(f(x,y,z))` is always a single value, even if `f` returns several values. (The
value of `(f(x,y,z))` is the first value returned by `f` or `nil` if `f` does
not return any values.)

# Flow control

## While and repeat

```
stat ::= while exp do block end
stat ::= repeat block until exp
```

Recall that `0` and `""` are considered true in condition expression. Only
`false` and `nil` are considered false.

You can make infinit loop with `while true` or `until false`.

## If statements

```
stat ::= if exp then block
         { elseif exp then block }
         [ else block ] end
```

## Return and break

`return` returns value(s) from a function: `stat ::= return [explist1]`.

`break` terminates the execution of the innermost `while`, `repeat` or `for`
loop: `stat ::= break`.

Note that `return` and `break` can only be used as the **last statement of a
block**. To use it in the middle of a block, then use an explicit block, e.g.
`do break end`.

## For statements

### Numeric for loop

```
stat ::= for Name = exp, exp [ , exp ] do block end
```

1.  The variable initialize with the value of the 1st exp
2.  Until it **passes** the 2nd exp, repeat block (i.e. also repeat if equal to
    the 2nd exp)
3.  After each loop its value is added by the 3rd expression.

-   All three control expressions are evaluated only once at start.
-   Default step (value of the 3rd exp) is 1.
-   The control variable is local to the loop.

```lua
mytable = { "apple", "orange", "pear" }
for i = 1, #mytable do
    print(mytable[i])
end
```

### Generic for loop

```
stat ::= for namelist in explist1 do block end
namelist ::= Name { , Name }
```

```lua
local teams = {
    ["teamA"] = 12,
    ["teamB"] = 15
}
for key,value in pairs(teams) do
    print(key .. ":" .. value)
end
```

# Functions

## Function call

`funcName(arg1, arg2)`

Missing arguments are passed as `nil`; extra arguments are discarded.

If the function only takes one literal string or one table literal, you can omit
the parentheses:

```lua
local func_with_opts = function(opts)
    local will_do_foo = opts.foo
    local filename = opts.filename
    ...
end

func_with_opts { foo = true, filename = "hello.world" }
```

## Function definition

```lua
function printTax(price)
    local tax = price * 0.21
    print("tax:" .. tax)
end
```

-   `function f () body end` == `f = function () body end`
-   `t.a.b.c.f () body end` == `t.a.b.c.f = function () body end`
-   `local function f () body end` == `local f; f = function () body end`
    -   But **not** equivalent to `local f = function () body end`. But it only
        makes a difference when the body of the function contains references to
        `f`

## Builtin functions

-   `print('hello')`
    -   `print(true) --true`

## Builtin math library

-   `math.pi`
-   `math.huge` is a constant that represents +infinity
-   `math.abs()`
-   `math.ceil()` ceiling of a number, i.e. round up to nearest integer
-   `math.floor()`
-   `math.sqrt()`
-   `math.log()`
-   `math.exp()`
-   `math.sin()`
-   `math.cos()`
-   `math.tan()`
-   `math.asin()`
-   `math.acos()`
-   `math.atan()`
-   `math.deg()`: turns rad to degrees
-   `math.rad()`: turns degrees to rad
-   `math.randomseed()`
-   `math.random()`: random value in [0, 1)
    -   `math.random(100)`: random integer from 1 to 100 (inclusive)
    -   `math.random(20, 100)`: random integer from 20 to 100 (inclusive)

# Patterns

Lua intentionally doesn't support regular expressions. It provides limited
patterns to maintain high performance:

```lua
print(string.match("foo123bar123", "%d+"))
-- 123
print(string.match("foo123bar123", "[^%d]+"))
-- foo
print(string.match("foo123bar123", "[abc]+"))
-- ba
print(string.match("foo.bar", "%.bar"))
-- .bar
```

# Import modules

```lua
require("otherfile")
```

Runs the code of another file. Don't need to include the extension of that file
in the filename.

# Running Lua

Use `lua` interpreter from the terminal. Support for both script mode and
interactive mode.

There is C integration, where you can invoke lua codes in C codes, and vice
versa through C API.

# References

-   `luaref.txt` in NeoVim help

# 🧭 Navigation

-   [🔼 Back to top](#)
-   📑 [Index](../../index.md)
