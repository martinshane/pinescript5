Language Operators
!=

Not equal to. Applicable to expressions of any type.
expr1 != expr2
RETURNS
Boolean value, or series of boolean values.
%

Modulo (integer remainder). Applicable to numerical expressions.
expr1 % expr2
RETURNS
Integer or float value, or series of values.
REMARKS
In Pine Script™, when the integer remainder is calculated, the quotient is truncated, i.e. rounded towards the lowest absolute value. The resulting value will have the same sign as the dividend.
Example: -1 % 9 = -1 - 9 * truncate(-1/9) = -1 - 9 * truncate(-0.111) = -1 - 9 * 0 = -1.
%=

Modulo assignment. Applicable to numerical expressions.
expr1 %= expr2
EXAMPLE

//@version=5
indicator("%=")
// Equals to expr1 = expr1 % expr2.
a = 3
b = 3
a %= b
// Result: a = 0.
plot(a)
RETURNS
Integer or float value, or series of values.
*

Multiplication. Applicable to numerical expressions.
expr1 * expr2
RETURNS
Integer or float value, or series of values.
*=

Multiplication assignment. Applicable to numerical expressions.
expr1 *= expr2
EXAMPLE

//@version=5
indicator("*=")
// Equals to expr1 = expr1 * expr2.
a = 2
b = 3
a *= b
// Result: a = 6.
plot(a)
RETURNS
Integer or float value, or series of values.
+

Addition or unary plus. Applicable to numerical expressions or strings.
expr1 + expr2
+ expr
RETURNS
Binary `+` for strings returns concatenation of expr1 and expr2
For numbers returns integer or float value, or series of values:
Binary `+` returns expr1 plus expr2.
Unary `+` returns expr (does nothing added just for the symmetry with the unary - operator).
REMARKS
You may use arithmetic operators with numbers as well as with series variables. In case of usage with series the operators are applied elementwise.
+=

Addition assignment. Applicable to numerical expressions or strings.
expr1 += expr2
EXAMPLE

//@version=5
indicator("+=")
// Equals to expr1 = expr1 + expr2.
a = 2
b = 3
a += b
// Result: a = 5.
plot(a)
RETURNS
For strings returns concatenation of expr1 and expr2. For numbers returns integer or float value, or series of values.
REMARKS
You may use arithmetic operators with numbers as well as with series variables. In case of usage with series the operators are applied elementwise.
-

Subtraction or unary minus. Applicable to numerical expressions.
expr1 - expr2
- expr
RETURNS
Returns integer or float value, or series of values:
Binary `-` returns expr1 minus expr2.
Unary `-` returns the negation of expr.
REMARKS
You may use arithmetic operators with numbers as well as with series variables. In case of usage with series the operators are applied elementwise.
-=

Subtraction assignment. Applicable to numerical expressions.
expr1 -= expr2
EXAMPLE

//@version=5
indicator("-=")
// Equals to expr1 = expr1 - expr2.
a = 2
b = 3
a -= b
// Result: a = -1.
plot(a)
RETURNS
Integer or float value, or series of values.
/

Division. Applicable to numerical expressions.
expr1 / expr2
RETURNS
Integer or float value, or series of values.
/=

Division assignment. Applicable to numerical expressions.
expr1 /= expr2
EXAMPLE

//@version=5
indicator("/=")
// Equals to expr1 = expr1 / expr2.
a = 3
b = 3
a /= b
// Result: a = 1.
plot(a)
RETURNS
Integer or float value, or series of values.
<

Less than. Applicable to numerical expressions.
expr1 < expr2
RETURNS
Boolean value, or series of boolean values.
<=

Less than or equal to. Applicable to numerical expressions.
expr1 <= expr2
RETURNS
Boolean value, or series of boolean values.
==

Equal to. Applicable to expressions of any type.
expr1 == expr2
RETURNS
Boolean value, or series of boolean values.
=>

The '=>' operator is used in user-defined function declarations and in switch statements.
The function declaration syntax is:
<identifier>([<parameter_name>[=<default_value>]], ...) =>
    <local_block>
    <function_result>
A <local_block> is zero or more Pine Script™ statements.
The <function_result> is a variable, an expression, or a tuple.
EXAMPLE

//@version=5
indicator("=>")
// single-line function
f1(x, y) => x + y
// multi-line function
f2(x, y) => 
    sum = x + y
    sumChange = ta.change(sum, 10)
    // Function automatically returns the last expression used in it
plot(f1(30, 8) + f2(1, 3))
REMARKS
You can learn more about user-defined functions in the User Manual's pages on Declaring functions and Libraries.
>

Greater than. Applicable to numerical expressions.
expr1 > expr2
RETURNS
Boolean value, or series of boolean values.
>=

Greater than or equal to. Applicable to numerical expressions.
expr1 >= expr2
RETURNS
Boolean value, or series of boolean values.
?:

Ternary conditional operator.
expr1 ? expr2 : expr3
EXAMPLE

//@version=5
indicator("?:")
// Draw circles at the bars where open crosses close
s2 = ta.cross(open, close) ? math.avg(open,close) : na
plot(s2, style=plot.style_circles, linewidth=2, color=color.red)

// Combination of ?: operators for 'switch'-like logic
c = timeframe.isintraday ? color.red : timeframe.isdaily ? color.green : timeframe.isweekly ? color.blue : color.gray
plot(hl2, color=c)
RETURNS
expr2 if expr1 is evaluated to true, expr3 otherwise. Zero value (0 and also NaN, +Infinity, -Infinity) is considered to be false, any other value is true.
REMARKS
Use na for 'else' branch if you do not need it.
You can combine two or more ?: operators to achieve the equivalent of a 'switch'-like statement (see examples above).
You may use arithmetic operators with numbers as well as with series variables. In case of usage with series the operators are applied elementwise.
SEE ALSO
na
[]

Series subscript. Provides access to previous values of series expr1. expr2 is the number of bars back, and must be numerical. Floats will be rounded down.
expr1[expr2]
EXAMPLE

//@version=5
indicator("[]")
// [] can be used to "save" variable value between bars
a = 0.0 // declare `a`
a := a[1] // immediately set current value to the same as previous. `na` in the beginning of history
if high == low // if some condition - change `a` value to another
    a := low
plot(a)
RETURNS
A series of values.
SEE ALSO
math.floor
and

Logical AND. Applicable to boolean expressions.
expr1 and expr2
RETURNS
Boolean value, or series of boolean values.
array

Keyword used to explicitly declare the "array" type of a variable or a parameter. Array objects (or IDs) can be created with the array.new<type>, array.from function.
EXAMPLE

//@version=5
indicator("array", overlay=true)
array<float> a = na
a := array.new<float>(1, close)
plot(array.get(a, 0))
REMARKS
Array objects are always of "series" form.
SEE ALSO
var
line
label
table
box
array.new<type>
array.from
bool

Keyword used to explicitly declare the "bool" (boolean) type of a variable or a parameter. "Bool" variables can have values true, false or na.
EXAMPLE

//@version=5
indicator("bool")
bool b = true    // Same as `b = true`
b := na
plot(b ? open : close)
REMARKS
Explicitly mentioning the type in a variable declaration is optional, except when it is initialized with na. Learn more about Pine Script™ types in the User Manual page on the Type System.
SEE ALSO
var
varip
int
float
color
string
true
false
box

Keyword used to explicitly declare the "box" type of a variable or a parameter. Box objects (or IDs) can be created with the box.new function.
EXAMPLE

//@version=5
indicator("box")
// Empty `box1` box ID.
var box box1 = na
// `box` type is unnecessary because `box.new()` returns a "box" type.
var box2 = box.new(na, na, na, na)
box3 = box.new(time, open, time + 60 * 60 * 24, close, xloc=xloc.bar_time)
REMARKS
Box objects are always of "series" form.
SEE ALSO
var
line
label
table
box.new
color

Keyword used to explicitly declare the "color" type of a variable or a parameter.
EXAMPLE

//@version=5
indicator("color", overlay = true)

color textColor = color.green   
color labelColor = #FF000080  // Red color (FF0000) with 50% transparency (80 which is half of FF).
if barstate.islastconfirmedhistory
    label.new(bar_index, high, text = "Label", color = labelColor, textcolor = textColor)

// When declaring variables with color literals, built-in constants(color.green) or functions (color.new(), color.rgb()), the "color" keyword for the type can be omitted.
c = color.rgb(0,255,0,0)
plot(close, color = c)
REMARKS
Color literals have the following format: #RRGGBB or #RRGGBBAA. The letter pairs represent 00 to FF hexadecimal values (0 to 255 in decimal) where RR, GG and BB pairs are the values for the color's red, green and blue components. AA is an optional value for the color's transparency (or alpha component) where 00 is invisible and FF opaque. When no AA pair is supplied, FF is used. The hexadecimal letters can be upper or lower case.
Explicitly mentioning the type in a variable declaration is optional, except when it is initialized with na. Learn more about Pine Script™ types in the User Manual page on the Type System.
SEE ALSO
var
varip
int
float
string
color.rgb
color.new
export

Used in libraries to prefix the declaration of functions or user-defined type definitions that will be available from other scripts importing the library.
EXAMPLE

//@version=5
//@description Library of debugging functions.
library("Debugging_library", overlay = true)
//@function Displays a string as a table cell for debugging purposes.
//@param txt String to display.
//@returns Void.
export print(string txt) => 
    var table t = table.new(position.middle_right, 1, 1)
    table.cell(t, 0, 0, txt, bgcolor = color.yellow)
// Using the function from inside the library to show an example on the published chart.
// This has no impact on scripts using the library.
print("Library Test")
REMARKS
Each library must have at least one exported function or user-defined type (UDT).
Exported functions cannot use variables from the global scope if they are arrays, mutable variables (reassigned with `:=`), or variables of 'input' form.
Exported functions cannot use `request.*()` functions.
Exported functions must explicitly declare each parameter's type and all parameters must be used in the function's body. By default, all arguments passed to exported functions are of the series form, unless they are explicitly specified as simple in the function's signature.
The @description, @function, @param, @type, @field, and @returns compiler annotations are used to automatically generate the library's description and release notes, and in the Pine Script™ Editor's tooltips.
SEE ALSO
library
false

Literal representing a bool value, and result of a comparison operation.
REMARKS
See the User Manual for comparison operators and logical operators.
SEE ALSO
bool
float

Keyword used to explicitly declare the "float" (floating point) type of a variable or a parameter.
EXAMPLE

//@version=5
indicator("float")
float f = 3.14    // Same as `f = 3.14`
f := na
plot(f)
REMARKS
Explicitly mentioning the type in a variable declaration is optional, except when it is initialized with na. Learn more about Pine Script™ types in the User Manual page on the Type System.
SEE ALSO
var
varip
int
bool
color
string
for

The 'for' structure allows the repeated execution of a number of statements:
[var_declaration =] for counter = from_num to to_num [by step_num]
    statements | continue | break
    return_expression
var_declaration - An optional variable declaration that will be assigned the value of the loop's return_expression.
counter - A variable holding the value of the loop's counter, which is incremented/decremented by 1 or by the step_num value on each iteration of the loop.
from_num - The starting value of the counter. "series int/float" values/expressions are allowed.
to_num - The end value of the counter. When the counter becomes greater than to_num (or less than to_num in cases where from_num > to_num) the loop is broken. "series int/float" values/expressions are allowed, but they are evaluated only on the loop's first iteration.
step_num - The increment/decrement value of the counter. It is optional. The default value is +1 or -1, depending on which of from_num or to_num is the greatest. When a value is used, the counter is also incremented/decremented depending on which of from_num or to_num is the greatest, so the +/- sign of step_num is optional.
statements | continue | break - Any number of statements, or the 'continue' or 'break' keywords, indented by 4 spaces or a tab.
return_expression - The loop's return value which is assigned to the variable in var_declaration if one is present. If the loop exits because of a 'continue' or 'break' keyword, the loop's return value is that of the last variable assigned a value before the loop's exit.
continue - A keyword that can only be used in loops. It causes the next iteration of the loop to be executed.
break - A keyword that exits the loop.
EXAMPLE

//@version=5
indicator("for")
// Here, we count the quantity of bars in a given 'lookback' length which closed above the current bar's close
qtyOfHigherCloses(lookback) =>
    int result = 0
    for i = 1 to lookback
        if close[i] > close
            result += 1
    result
plot(qtyOfHigherCloses(14))
EXAMPLE

//@version=5
indicator("`for` loop with a step")

a = array.from(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
sum = 0.0

for i = 0 to 9 by 5
    // Because the step is set to 5, we are adding only the first (0) and the sixth (5) value from the array `a`.
    sum += array.get(a, i)

plot(sum)
SEE ALSO
for...in
for...in

The `for...in` structure allows the repeated execution of a number of statements for each element in an array. It can be used with either one argument: `array_element`, or with two: `[index, array_element]`. The second form doesn't affect the functionality of the loop. It tracks the current iteration's index in the tuple's first variable.
[var_declaration =] for array_element in array_id
    statements | continue | break
    return_expression

[var_declaration =] for [index, array_element] in array_id
    statements | continue | break
    return_expression
var_declaration - An optional variable declaration that will be assigned the value of the loop's `return_expression`.
index - An optional variable that tracks the current iteration's index. Indexing starts at 0. The variable is immutable in the loop's body. When used, it must be included in a tuple also containing `array_element`.
array_element - A variable containing each successive array element to be processed in the loop. The variable is immutable in the loop's body.
array_id - The ID of the array over which the loop is iterated.
statements | continue | break - Any number of statements, or the 'continue' or 'break' keywords, indented by 4 spaces or a tab.
return_expression - The loop's return value assigned to the variable in `var_declaration`, if one is present. If the loop exits because of a 'continue' or 'break' keyword, the loop's return value is that of the last variable assigned a value before the loop's exit.
continue - A keyword that can only be used in loops. It causes the next iteration of the loop to be executed.
break - A keyword that exits the loop.
It is allowed to modify the array's elements or its size inside the loop.
Here, we use the single-argument form of `for...in` to determine on each bar how many of the bar's OHLC values are greater than the SMA of 'close' values:
EXAMPLE

//@version=5
indicator("for...in")
// Here we determine on each bar how many of the bar's OHLC values are greater than the SMA of 'close' values
float[] ohlcValues = array.from(open, high, low, close)
qtyGreaterThan(value, array) =>
    int result = 0
    for currentElement in array
        if currentElement > value
            result += 1
        result
plot(qtyGreaterThan(ta.sma(close, 20), ohlcValues))
Here, we use the two-argument form of for...in to set the values of our `isPos` array to `true` when their corresponding value in our `valuesArray` array is positive:
EXAMPLE

//@version=5
indicator("for...in")
var valuesArray = array.from(4, -8, 11, 78, -16, 34, 7, 99, 0, 55)
var isPos = array.new_bool(10, false)

for [index, value] in valuesArray
    if value > 0
        array.set(isPos, index, true)

if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(isPos))
Iterate through matrix rows as arrays.
EXAMPLE

//@version=5
indicator("`for ... in` matrix Example")

// Create a 2x3 matrix with values `4`.
matrix1 = matrix.new<int>(2, 3, 4)

sum = 0.0
// Loop through every row of the matrix.
for rowArray in matrix1
    // Sum values of the every row 
    sum += array.sum(rowArray)

plot(sum)
SEE ALSO
for
if

If statement defines what block of statements must be executed when conditions of the expression are satisfied.
To have access to and use the if statement, one should specify the version >= 2 of Pine Script™ language in the very first line of code, for example: //@version=5
The 4th version of Pine Script™ Language allows you to use “else if” syntax.
General code form:
var_declarationX = if condition
    var_decl_then0
    var_decl_then1
    …
    var_decl_thenN
else if [optional block]
    var_decl_else0
    var_decl_else1
    …
    var_decl_elseN
else
    var_decl_else0
    var_decl_else1
    …
    var_decl_elseN
    return_expression_else
where
var_declarationX — this variable gets the value of the if statement
condition — if the condition is true, the logic from the block 'then' (var_decl_then0, var_decl_then1, etc.) is used.
If the condition is false, the logic from the block 'else' (var_decl_else0, var_decl_else1, etc.) is used.
return_expression_then, return_expression_else — the last expression from the block then or from the block else will return the final value of the statement. If declaration of the variable is in the end, its value will be the result.
The type of returning value of the if statement depends on return_expression_then and return_expression_else type (their types must match: it is not possible to return an integer value from then, while you have a string value in else block).
EXAMPLE

//@version=5
indicator("if")
// This code compiles
x = if close > open
    close
else
    open

// This code doesn’t compile
// y = if close > open
//     close
// else
//     "open"
plot(x)
It is possible to omit the `else` block. In this case if the condition is false, an “empty” value (na, false, or “”) will be assigned to the var_declarationX variable:
EXAMPLE

//@version=5
indicator("if")
x = if close > open
    close
// If current close > current open, then x = close.
// Otherwise the x = na.
plot(x)
It is possible to use either multiple “else if” blocks or none at all. The blocks “then”, “else if”, “else” are shifted by four spaces:
EXAMPLE

//@version=5
indicator("if")
x = if open > close
    5
else if high > low
    close
else
    open
plot(x)
It is possible to ignore the resulting value of an `if` statement (“var_declarationX=“ can be omitted). It may be useful if you need the side effect of the expression, for example in strategy trading:
EXAMPLE

//@version=5
strategy("if")
if (ta.crossover(high, low))
    strategy.entry("BBandLE", strategy.long, stop=low, oca_name="BollingerBands", oca_type=strategy.oca.cancel, comment="BBandLE")
else
    strategy.cancel(id="BBandLE")
If statements can include each other:
EXAMPLE

//@version=5
indicator("if")
float x = na
if close > open
    if close > close[1]
        x := close
    else
        x := close[1]
else
    x := open
plot(x)
import

Used to load an external library into a script and bind its functions to a namespace. The importing script can be an indicator, a strategy, or another library. A library must be published (privately or publicly) before it can be imported.
import {username}/{libraryName}/{libraryVersion} as {alias}
EXAMPLE

//@version=5
indicator("num_methods import")
// Import the first version of the username’s "num_methods" library and assign it to the "m" namespace",
import username/num_methods/1 as m
// Call the “sinh()” function from the imported library
y = m.sinh(3.14)
// Plot value returned by the "sinh()" function",
plot(y)
ARGUMENTS
username (literal string) User name of the library's author.
libraryName (literal string) Name of the imported library, which corresponds to the `title` argument used by the author in his library script.
libraryVersion (literal int) Version number of the imported library.
alias (literal string) Namespace used to refer to the library's functions. Optional. The default is the libraryName string.
REMARKS
Using an alias that replaces a built-in namespace such as math.* or strategy.* is allowed, but if the library contains function names that shadow Pine Script™'s built-in functions, the built-ins will become unavailable. The same version of a library can only be imported once. Aliases must be distinct for each imported library. When calling library functions, casting their arguments to types other than their declared type is not allowed.
SEE ALSO
library
export
int

Keyword used to explicitly declare the "int" (integer) type of a variable or a parameter.
EXAMPLE

//@version=5
indicator("int")
int i = 14    // Same as `i = 14`
i := na
plot(i)
REMARKS
Explicitly mentioning the type in a variable declaration is optional, except when it is initialized with na. Learn more about Pine Script™ types in the User Manual page on the Type System.
SEE ALSO
var
varip
float
bool
color
string
label

Keyword used to explicitly declare the "label" type of a variable or a parameter. Label objects (or IDs) can be created with the label.new function.
EXAMPLE

//@version=5
indicator("label")
// Empty `label1` label ID.
var label label1 = na
// `label` type is unnecessary because `label.new()` returns "label" type.
var label2 = label.new(na, na, na)
if barstate.islastconfirmedhistory
    label3 = label.new(bar_index, high, text = "label3 text")
REMARKS
Label objects are always of "series" form.
SEE ALSO
var
line
box
label.new
line

Keyword used to explicitly declare the "line" type of a variable or a parameter. Line objects (or IDs) can be created with the line.new function.
EXAMPLE

//@version=5
indicator("line")
// Empty `line1` line ID.
var line line1 = na
// `line` type is unnecessary because `line.new()` returns "line" type.
var line2 = line.new(na, na, na, na)
line3 = line.new(bar_index - 1, high, bar_index, high, extend = extend.right)
REMARKS
Line objects are always of "series" form.
SEE ALSO
var
label
box
line.new
linefill

Keyword used to explicitly declare the "linefill" type of a variable or a parameter. Linefill objects (or IDs) can be created with the linefill.new function.
EXAMPLE

//@version=5
indicator("linefill", overlay=true)
// Empty `linefill1` line ID.
var linefill linefill1 = na
// `linefill` type is unnecessary because `linefill.new()` returns "linefill" type.
var linefill2 = linefill.new(na, na, na)

if barstate.islastconfirmedhistory
    line1 = line.new(bar_index - 10, high+1, bar_index, high+1, extend = extend.right)
    line2 = line.new(bar_index - 10, low+1, bar_index, low+1, extend = extend.right)
    linefill3 = linefill.new(line1, line2, color = color.new(color.green, 80))
REMARKS
Linefill objects are always of "series" form.
SEE ALSO
var
line
label
table
box
linefill.new
matrix

Keyword used to explicitly declare the "matrix" type of a variable or a parameter. Matrix objects (or IDs) can be created with the matrix.new<type> function.
EXAMPLE

//@version=5
indicator("matrix example")

// Create `m1` matrix of `int` type.
matrix<int> m1 = matrix.new<int>(2, 3, 0)

// `matrix<int>` is unnecessary because the `matrix.new<int>()` function returns an `int` type matrix object.
m2 = matrix.new<int>(2, 3, 0)

// Display matrix using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m2))
REMARKS
Matrix objects are always of "series" form.
SEE ALSO
var
matrix.new<type>
array
method

This keyword is used to prefix a function declaration, indicating it can then be invoked using dot notation by appending its name to a variable of the type of its first parameter and omitting that first parameter. Alternatively, functions declared as methods can also be invoked like normal user-defined functions. In that case, an argument must be supplied for its first parameter.
The first parameter of a method declaration must be explicitly typified.
[export] method <functionName>(<paramType> <paramName> [= <defaultValue>], …) =>
    <functionBlock>
EXAMPLE

//@version=5
indicator("")

var prices = array.new<float>()

//@function Pushes a new value into the array and removes the first one if the resulting array is greater than `maxSize`. Can be used as a method.
method maintainArray(array<float> id, maxSize, value) =>
    id.push(value)
    if id.size() > maxSize
        id.shift()

prices.maintainArray(50, close)
// The method can also be called like a function, without using dot notation.
// In this case an argument must be supplied for its first parameter.
// maintainArray(prices, 50, close)

// This calls the `array.avg()` built-in using dot notation with the `prices` array.
// It is possible because built-in functions belonging to some namespaces that are a special Pine type
// can be invoked with method notation when the function's first parameter is an ID of that type.
// Those namespaces are: `array`, `matrix`, `line`, `linefill`, `label`, `box`, and `table`.
plot(prices.avg())
not

Logical negation (NOT). Applicable to boolean expressions.
not expr1
RETURNS
Boolean value, or series of boolean values.
or

Logical OR. Applicable to boolean expressions.
expr1 or expr2
RETURNS
Boolean value, or series of boolean values.
series

series is a keyword that can be used in a library's exported functions to specify the type form required for a function's arguments. Explicit use of the `series` keyword is usually unnecessary because all arguments of exported functions are automatically converted to the "series" form by default.
export <functionName>([[series] <type>] <arg1>[ = <default_value>])
EXAMPLE

//@version=5
//@description Library of debugging functions.
library("Debugging_library", overlay = true)
export smaCustom(series float source, series int length) =>
    ta.sma(source, length)
simple

simple is a keyword that can be used in a library's exported functions to specify the type form required for a function's arguments. By default, all arguments of exported functions are automatically converted into the "series" type form. In some cases, this would make arguments unusable with those of built-in functions that do not support the "series" form. For these cases, the `simple` keyword can be used instead.
export <functionName>([[simple] <type>] <arg1>[ = <default_value>])
EXAMPLE

//@version=5
//@description Library of debugging functions.
library("Debugging_library", overlay = true)
export emaWrong(float source, int length) =>
    // By default, both `source` and `length` will expect values of the `series` type form: `series float` for `source`, `series int` for `length`.
    // This function will not compile because `ema()` does not support a "series int" argument for `length`. A "simple int" is required.
    ta.ema(source, length)

export emaRight(float source, simple int length) =>
    // This function requires an argument of "simple int" type for its `length` parameter.
    ta.ema(source, length)
string

Keyword used to explicitly declare the "string" type of a variable or a parameter.
EXAMPLE

//@version=5
indicator("string")
string s = "Hello World!"    // Same as `s = "Hello world!"`
// string s = na // same as "" 
plot(na, title=s)
REMARKS
Explicitly mentioning the type in a variable declaration is optional, except when it is initialized with na. Learn more about Pine Script™ types in the User Manual page on the Type System.
SEE ALSO
var
varip
int
float
bool
str.tostring
str.format
switch

The switch operator transfers control to one of the several statements, depending on the values of a condition and expressions.
[variable_declaration = ] switch expression
    value1 => local_block
    value2 => local_block
    …
    => default_local_block

[variable_declaration = ] switch
    boolean_expression1 => local_block
    boolean_expression2 => local_block
    …
    => default_local_block
Switch with an expression:
EXAMPLE

//@version=5
indicator("Switch using an expression")

string i_maType = input.string("EMA", "MA type", options = ["EMA", "SMA", "RMA", "WMA"])

float ma = switch i_maType
    "EMA" => ta.ema(close, 10)
    "SMA" => ta.sma(close, 10)
    "RMA" => ta.rma(close, 10)
    // Default used when the three first cases do not match.
    => ta.wma(close, 10)

plot(ma)
Switch without an expression:
EXAMPLE

//@version=5
strategy("Switch without an expression", overlay = true)

bool longCondition  = ta.crossover( ta.sma(close, 14), ta.sma(close, 28))
bool shortCondition = ta.crossunder(ta.sma(close, 14), ta.sma(close, 28))

switch
    longCondition  => strategy.entry("Long ID", strategy.long)
    shortCondition => strategy.entry("Short ID", strategy.short)
RETURNS
The value of the last expression in the local block of statements that is executed.
REMARKS
Only one of the `local_block` instances or the `default_local_block` can be executed. The `default_local_block` is introduced with the `=>` token alone and is only executed when none of the preceding blocks are executed. If the result of the `switch` statement is assigned to a variable and a `default_local_block` is not specified, the statement returns `na` if no `local_block` is executed. When assigning the result of the `switch` statement to a variable, all `local_block` instances must return the same type of value.
SEE ALSO
if
?:
table

Keyword used to explicitly declare the "table" type of a variable or a parameter. Table objects (or IDs) can be created with the table.new function.
EXAMPLE

//@version=5
indicator("table")
// Empty `table1` table ID.
var table table1 = na
// `table` type is unnecessary because `table.new()` returns "table" type.
var table2 = table.new(position.top_left, na, na)

if barstate.islastconfirmedhistory
    var table3 = table.new(position = position.top_right, columns = 1, rows = 1, bgcolor = color.yellow, border_width = 1)
    table.cell(table_id = table3, column = 0, row = 0, text = "table3 text")
REMARKS
Table objects are always of "series" form.
SEE ALSO
var
line
label
box
table.new
true

Literal representing one of the values a bool variable can hold, or an expression can evaluate to when it uses comparison or logical operators.
REMARKS
See the User Manual for comparison operators and logical operators.
SEE ALSO
bool
type

This keyword allows the declaration of user-defined types (UDT) from which scripts can instantiate objects. UDTs are composite types that contain an arbitrary number of fields of any built-in or user-defined type, including the defined UDT itself. The syntax to define a UDT is:
[export ]type <UDT_identifier>
    [varip ]<field_type> <field_name> [= <value>]
    …
Once a UDT is defined, scripts can instantiate objects from it with the `UDT_identifier.new()` construct. When creating a new type instance, the fields of the resulting object will initialize with the default values from the UDT's definition. Any type fields without specified defaults will initialize as na. Alternatively, users can pass initial values as arguments in the `*.new()` method to override the type's defaults. For example, `newFooObject = foo.new(x = true)` assigns a new `foo` object to the `newFooObject` variable with its `x` field initialized using a value of true.
Field declarations can include the varip keyword, in which case the field values persist between successive script iterations on the same bar.
For more information see the User Manual's sections on defining UDTs and using objects.
Libraries can export UDTs. See theLibraries page of our User Manual to learn more.
EXAMPLE

//@version=5
indicator("Multi Time Period Chart", overlay = true)

timeframeInput = input.timeframe("1D")

type bar
    float o = open
    float h = high
    float l = low
    float c = close
    int   t = time

drawBox(bar b, right) =>
    bar s = bar.new()
    color boxColor = b.c >= b.o ? color.green : color.red
    box.new(b.t, b.h, right, b.l, boxColor, xloc = xloc.bar_time, bgcolor = color.new(boxColor, 90))

updateBox(box boxId, bar b) =>
    color boxColor = b.c >= b.o ? color.green : color.red
    box.set_border_color(boxId, boxColor)
    box.set_bgcolor(boxId, color.new(boxColor, 90))
    box.set_top(boxId, b.h)
    box.set_bottom(boxId, b.l)
    box.set_right(boxId, time)

secBar = request.security(syminfo.tickerid, timeframeInput, bar.new())

if not na(secBar)
    // To avoid a runtime error, only process data when an object exists.
    if not barstate.islast
        if timeframe.change(timeframeInput)
            // On historical bars, draw a new box in the past when the HTF closes.
            drawBox(secBar, time[1])
    else
        var box lastBox = na
        if na(lastBox) or timeframe.change(timeframeInput)
            // On the last bar, only draw a new current box the first time we get there or when HTF changes.
            lastBox := drawBox(secBar, time)
        else
            // On other chart updates, use setters to modify the current box.
            updateBox(lastBox, secBar)
var

var is the keyword used for assigning and one-time initializing of the variable.
Normally, a syntax of assignment of variables, which doesn’t include the keyword var, results in the value of the variable being overwritten with every update of the data. Contrary to that, when assigning variables with the keyword var, they can “keep the state” despite the data updating, only changing it when conditions within if-expressions are met.
var variable_name = expression
where:
variable_name - any name of the user’s variable that’s allowed in Pine Script™ (can contain capital and lowercase Latin characters, numbers, and underscores (_), but can’t start with a number).
expression - any arithmetic expression, just as with defining a regular variable. The expression will be calculated and assigned to a variable once.
EXAMPLE

//@version=5
indicator("Var keyword example")
var a = close
var b = 0.0
var c = 0.0
var green_bars_count = 0
if close > open
    var x = close
    b := x
    green_bars_count := green_bars_count + 1
    if green_bars_count >= 10
        var y = close
        c := y
plot(a)
plot(b)
plot(c)
The variable 'a' keeps the closing price of the first bar for each bar in the series.
The variable 'b' keeps the closing price of the first "green" bar in the series.
The variable 'c' keeps the closing price of the tenth "green" bar in the series.
varip

varip (var intrabar persist) is the keyword used for the assignment and one-time initialization of a variable or a field of a user-defined type. It’s similar to the var keyword, but variables and fields declared with varip retain their values between executions of the script on the same bar.
varip [<variable_type> ]<variable_name> = <expression>

[export ]type <UDT_identifier>
    varip <field_type> <field_name> [= <value>]
where:
variable_type - An optional fundamental type (int, float, bool, color, string) or a user-defined type. Arrays, matrices, and other special types are not compatible with this keyword.
variable_name - A valid identifier. The variable can also be an object created from a UDT.
expression - Any arithmetic expression, just as when defining a regular variable. The expression will be calculated and assigned to the variable only once, on the first bar.
UDT_identifier, field_type, field_name, value - Constructs related to user-defined types as described in the type section.
EXAMPLE

//@version=5
indicator("varip")
varip int v = -1
v := v + 1
plot(v)
With var, `v` would equal the value of the bar_index. On historical bars, where the script calculates only once per chart bar, the value of `v` is the same as with var. However, on realtime bars, the script will evaluate the expression on each new chart update, producing a different result.
EXAMPLE

//@version=5
indicator("varip with types")
type barData
    int index = -1
    varip int ticks = -1

var currBar = barData.new()
currBar.index += 1
currBar.ticks += 1

// Will be equal to bar_index on all bars
plot(currBar.index)
// In real time, will increment per every tick on the chart
plot(currBar.ticks)
The same += operation applied to both the `index` and `ticks` fields results in different real-time values because `ticks` increases on every chart update, while `index` only does so once per bar. Note how the `currBar` object does not use the varip keyword. The `ticks` field of the object can increment on every tick, but the reference itself is defined once and then stays unchanged. If we were to declare `currBar` using varip, the behavior of `index` would remain unchanged because while the reference to the type instance would persist between chart updates, the `index` field of the object would not.
REMARKS
When using varip to declare variables in strategies that may execute more than once per historical chart bar, the values of such variables are preserved across successive iterations of the script on the same bar.
The effect of varip eliminates the rollback of variables before each successive execution of a script on the same bar.
while

The `while` statement allows the conditional iteration of a local code block.
variable_declaration = while boolean_expression
    …
    continue
    …
    break
    …
    return_expression
where:
variable_declaration - An optional variable declaration. The `return expression` can provide the initialization value for this variable.
boolean_expression - when true, the local block of the `while` statement is executed. When false, execution of the script resumes after the `while` statement.
continue - The `continue` keyword causes the loop to branch to its next iteration.
break - The `break` keyword causes the loop to terminate. The script's execution resumes after the `while` statement.
return_expression - An optional line providing the `while` statement's returning value.
EXAMPLE

//@version=5
indicator("while")
// This is a simple example of calculating a factorial using a while loop.
int i_n = input.int(10, "Factorial Size", minval=0)
int counter   = i_n
int factorial = 1
while counter > 0
    factorial := factorial * counter
    counter   := counter - 1

plot(factorial)
REMARKS
The local code block after the initial `while` line must be indented with four spaces or a tab. For the `while` loop to terminate, the boolean expression following `while` must eventually become false, or a `break` must be executed.