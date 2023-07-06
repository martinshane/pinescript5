# Pine Script v5 Release Notes

This document contains release notes for Pine Script v5. 

June 2023
New syminfo.* built-in variables were added:

syminfo.sector - Returns the sector of the symbol.
syminfo.industry - Returns the industry of the symbol.
syminfo.country - Returns the two-letter code of the country where the symbol is traded.
May 2023
Added a new input to Strategy properties: “Fill orders using standard OHLC” (parameter: fill_orders_on_standard_ohlc). When true, it allows a strategy running on Heikin Ashi charts to simulate orders using the market’s actual OHLC prices rather than the synthetic HA prices. This will help ensure realistic backtest results.

New parameter added to the strategy.entry(), strategy.order(), strategy.close(), strategy.close_all(), and strategy.exit() functions:

disable_alert - Disables order fill alerts for any orders placed by the function.
Our “Indicator on indicator” feature, which allows a script to pass another indicator’s plot as a source value via the input.source() function, now supports multiple external inputs. Scripts can use a multitude of external inputs originating from up to 10 different indicators.

We’ve added the following array functions:

array.every() - Returns true if all elements of the id array are true, false otherwise.
array.some() - Returns true if at least one element of the id array is true, false otherwise.
These functions also work with arrays of int and float types, in which case zero values are considered false, and all others true.

April 2023
Fixed an issue with trailing stops in strategy.exit() being filled on high/low prices rather than on intrabar prices.

Fixed behavior of array.mode(), matrix.mode() and ta.mode(). Now these functions will return the smallest value when the data has no most frequent value.

March 2023
It is now possible to use seconds-based timeframe strings for the timeframe parameter in request.security() and request.security_lower_tf().

A new function was added:

request.currency_rate() - provides a daily rate to convert a value expressed in the from currency to another in the to currency.
February 2023
Pine Script™ Methods
Pine Script™ methods are specialized functions associated with specific instances of built-in or user-defined types. They offer a more convenient syntax than standard functions, as users can access methods in the same way as object fields using the handy dot notation syntax. Pine Script™ includes built-in methods for array, matrix, line, linefill, label, box, and table types and facilitates user-defined methods with the new method keyword. For more details on this new feature, see our User Manual’s page on methods.

January 2023
New array functions were added:

array.first() - Returns the array’s first element.
array.last() - Returns the array’s last element.
2022
December 2022
Pine Objects
Pine objects are instantiations of the new user-defined composite types (UDTs) declared using the type keyword. Experienced programmers can think of UDTs as method-less classes. They allow users to create custom types that organize different values under one logical entity. A detailed rundown of the new functionality can be found in our User Manual’s page on objects.

A new function was added:

ticker.standard() - Creates a ticker to request data from a standard chart that is unaffected by modifiers like extended session, dividend adjustment, currency conversion, and the calculations of non-standard chart types: Heikin Ashi, Renko, etc.
New strategy.* functions were added:

strategy.opentrades.entry_comment() - The function returns the comment message of the open trade’s entry.
strategy.closedtrades.entry_comment() - The function returns the comment message of the closed trade’s entry.
strategy.closedtrades.exit_comment() - The function returns the comment message of the closed trade’s exit.
November 2022
Fixed behaviour of math.round_to_mintick() function. For ‘na’ values it returns ‘na’.

October 2022
Pine Script™ now has a new, more powerful and better-integrated editor. Read our blog to find out everything to know about all the new features and upgrades.

New overload for the fill() function was added. Now it can create vertical gradients. More info about it in the blog post.

A new function was added:

str.format_time() - Converts a timestamp to a formatted string using the specified format and time zone.
September 2022
The text_font_family parameter now allows the selection of a monospace font in label.new(), box.new() and table.cell() function calls, which makes it easier to align text vertically. Its arguments can be:

font.family_default - Specifies the default font.
font.family_monospace - Specifies a monospace font.
The accompanying setter functions are:

label.set_text_font_family() - The function sets the font family of the text inside the label.
box.set_text_font_family() - The function sets the font family of the text inside the box.
table.cell_set_text_font_family() - The function sets the font family of the text inside the cell.
August 2022
A new label style label.style_text_outline was added.

A new parameter for the ta.pivot_point_levels() function was added:

developing - If false, the values are those calculated the last time the anchor condition was true. They remain constant until the anchor condition becomes true again. If true, the pivots are developing, i.e., they constantly recalculate on the data developing between the point of the last anchor (or bar zero if the anchor condition was never true) and the current bar. Cannot be true when type is set to "Woodie".
A new parameter for the box.new() function was added:

text_wrap - It defines whether the text is presented in a single line, extending past the width of the box if necessary, or wrapped so every line is no wider than the box itself.
This parameter supports two arguments:

text.wrap_none - Disabled wrapping mode for box.new and box.set_text_wrap functions.
text.wrap_auto - Automatic wrapping mode for box.new and box.set_text_wrap functions.
New built-in functions were added:

ta.min() - Returns the all-time low value of source from the beginning of the chart up to the current bar.
ta.max() - Returns the all-time high value of source from the beginning of the chart up to the current bar.
A new annotation //@strategy_alert_message was added. If the annotation is added to the strategy, the text written after it will be automatically set as the default alert message in the Create Alert window.

//@version=5
// @strategy_alert_message My Default Alert Message
strategy("My Strategy")
plot(close)
July 2022
It is now possible to fine-tune where a script’s plot values are displayed through the introduction of new arguments for the display parameter of the plot(), plotchar(), plotshape(), plotarrow(), plotcandle(), and plotbar() functions.

Four new arguments were added, complementing the previously available display.all and display.none:

display.data_window displays the plot values in the Data Window, one of the items available from the chart’s right sidebar.
display.pane displays the plot in the pane where the script resides, as defined in with the overlay parameter of the script’s indicator(), strategy(), or library() declaration statement.
display.price_scale controls the display of the plot’s label and price in the price scale, if the chart’s settings allow them.
display.status_line displays the plot values in the script’s status line, next to the script’s name on the chart, if the chart’s settings allow them.
The display parameter supports the addition and subtraction of its arguments:

display.all - display.status_line will display the plot’s information everywhere except in the script’s status line.
display.price_scale + display.status_line will display the plot in the price scale and status line only.
June 2022
The behavior of the argument used with the qty_percent parameter of strategy.exit() has changed. Previously, the percentages used on successive exit orders of the same position were calculated from the remaining position at any given time. Instead, the percentages now always apply to the initial position size. When executing the following strategy, for example:

//@version=5
strategy("strategy.exit() example", overlay = true)
strategy.entry("Long", strategy.long, qty = 100)
strategy.exit("Exit Long1", "Long", trail_points = 50, trail_offset = 0, qty_percent = 20)
strategy.exit("Exit Long2", "Long", trail_points = 100, trail_offset = 0, qty_percent = 20)
20% of the initial position will be closed on each strategy.exit() call. Before, the first call would exit 20% of the initial position, and the second would exit 20% of the remaining 80% of the position, so only 16% of the initial position.

Two new parameters for the built-in ta.vwap() function were added:

anchor - Specifies the condition that triggers the reset of VWAP calculations. When true, calculations reset; when false, calculations proceed using the values accumulated since the previous reset.
stdev_mult - If specified, the ta.vwap() calculates the standard deviation bands based on the main VWAP series and returns a [vwap, upper_band, lower_band] tuple.
New overloaded versions of the strategy.close() and strategy.close_all() functions with the immediately parameter. When immediately is set to true, the closing order will be executed on the tick where it has been placed, ignoring the strategy parameters that restrict the order execution to the open of the next bar.

New built-in functions were added:

timeframe.change() - Returns true on the first bar of a new timeframe, false otherwise.
ta.pivot_point_levels() - Returns a float array with numerical values representing 11 pivot point levels: [P, R1, S1, R2, S2, R3, S3, R4, S4, R5, S5]. Levels absent from the specified type return na values.
New built-in variables were added:

session.isfirstbar - returns true if the current bar is the first bar of the day’s session, false otherwise.
session.islastbar - returns true if the current bar is the last bar of the day’s session, false otherwise.
session.isfirstbar_regular - returns true on the first regular session bar of the day, false otherwise.
session.islastbar_regular - returns true on the last regular session bar of the day, false otherwise.
chart.left_visible_bar_time - returns the time of the leftmost bar currently visible on the chart.
chart.right_visible_bar_time - returns the time of the rightmost bar currently visible on the chart.
May 2022
Matrix support has been added to the request.security() function.

The historical states of arrays and matrices can now be referenced with the [] operator. In the example below, we reference the historic state of a matrix 10 bars ago:

//@version=5
indicator("matrix.new<float> example")
m = matrix.new<float>(1, 1, close)
float x = na
if bar_index > 10
    x := matrix.get(m[10], 0, 0)
plot(x)
plot(close)
The ta.change() function now can take values of int and bool types as its source parameter and return the difference in the respective type.

New built-in variables were added:

chart.bg_color - Returns the color of the chart’s background from the "Chart settings/Appearance/Background" field.
chart.fg_color - Returns a color providing optimal contrast with chart.bg_color.
chart.is_standard - Returns true if the chart type is bars, candles, hollow candles, line, area or baseline, false otherwise.
currency.USDT - A constant for the Tether currency code.
New functions were added:

syminfo.prefix() - returns the exchange prefix of the symbol passed to it, e.g. “NASDAQ” for “NASDAQ:AAPL”.
syminfo.ticker() - returns the ticker of the symbol passed to it without the exchange prefix, e.g. “AAPL” for “NASDAQ:AAPL”.
request.security_lower_tf() - requests data from a lower timeframe than the chart’s.
Added use_bar_magnifier parameter for the strategy() function. When true, the Broker Emulator uses lower timeframe data during history backtesting to achieve more realistic results.

Fixed behaviour of strategy.exit() function when stop loss triggered at prices outside the bars price range.

Added new comment and alert message parameters for the strategy.exit() function:

comment_profit - additional notes on the order if the exit was triggered by crossing profit or limit specifically.
comment_loss - additional notes on the order if the exit was triggered by crossing stop or loss specifically.
comment_trailing - additional notes on the order if the exit was triggered by crossing trail_offset specifically.
alert_profit - text that will replace the '{{strategy.order.alert_message}}' placeholder if the exit was triggered by crossing profit or limit specifically.
alert_loss - text that will replace the '{{strategy.order.alert_message}}' placeholder if the exit was triggered by crossing stop or loss specifically.
alert_trailing - text that will replace the '{{strategy.order.alert_message}}' placeholder if the exit was triggered by crossing trail_offset specifically.
April 2022
Added the display parameter to the following functions: barcolor, bgcolor, fill, hline.

A new function was added:

request.economic() - Economic data includes information such as the state of a country’s economy or of a particular industry.
New built-in variables were added:

strategy.max_runup - Returns the maximum equity run-up value for the whole trading interval.
syminfo.volumetype - Returns the volume type of the current symbol.
chart.is_heikinashi - Returns true if the chart type is Heikin Ashi, false otherwise.
chart.is_kagi - Returns true if the chart type is Kagi, false otherwise.
chart.is_linebreak - Returns true if the chart type is Line break, false otherwise.
chart.is_pnf - Returns true if the chart type is Point & figure, false otherwise.
chart.is_range - Returns true if the chart type is Range, false otherwise.
chart.is_renko - Returns true if the chart type is Renko, false otherwise.
New matrix functions were added:

matrix.new<type> - Creates a new matrix object. A matrix is a two-dimensional data structure containing rows and columns. All elements in the matrix must be of the type specified in the type template (“<type>”).
matrix.row() - Creates a one-dimensional array from the elements of a matrix row.
matrix.col() - Creates a one-dimensional array from the elements of a matrix column.
matrix.get() - Returns the element with the specified index of the matrix.
matrix.set() - Assigns value to the element at the column and row index of the matrix.
matrix.rows() - Returns the number of rows in the matrix.
matrix.columns() - Returns the number of columns in the matrix.
matrix.elements_count() - Returns the total number of matrix elements.
matrix.add_row() - Adds a row to the matrix. The row can consist of na values, or an array can be used to provide values.
matrix.add_col() - Adds a column to the matrix. The column can consist of na values, or an array can be used to provide values.
matrix.remove_row() - Removes the row of the matrix and returns an array containing the removed row’s values.
matrix.remove_col() - Removes the column of the matrix and returns an array containing the removed column’s values.
matrix.swap_rows() - Swaps the rows in the matrix.
matrix.swap_columns() - Swaps the columns in the matrix.
matrix.fill() - Fills a rectangular area of the matrix defined by the indices from_column to to_column.
matrix.copy() - Creates a new matrix which is a copy of the original.
matrix.submatrix() - Extracts a submatrix within the specified indices.
matrix.reverse() - Reverses the order of rows and columns in the matrix. The first row and first column become the last, and the last become the first.
matrix.reshape() - Rebuilds the matrix to rows x cols dimensions.
matrix.concat() - Append one matrix to another.
matrix.sum() - Returns a new matrix resulting from the sum of two matrices, or of a matrix and a scalar (a numerical value).
matrix.diff() - Returns a new matrix resulting from the subtraction between matrices, or of matrix and a scalar (a numerical value).
matrix.mult() - Returns a new matrix resulting from the product between the matrices, or between a matrix and a scalar (a numerical value), or between a matrix and a vector (an array of values).
matrix.sort() - Rearranges the rows in the id matrix following the sorted order of the values in the column.
matrix.avg() - Calculates the average of all elements in the matrix.
matrix.max() - Returns the largest value from the matrix elements.
matrix.min() - Returns the smallest value from the matrix elements.
matrix.median() - Calculates the median (“the middle” value) of matrix elements.
matrix.mode() - Calculates the mode of the matrix, which is the most frequently occurring value from the matrix elements. When there are multiple values occurring equally frequently, the function returns the smallest of those values.
matrix.pow() - Calculates the product of the matrix by itself power times.
matrix.det() - Returns the determinant of a square matrix.
matrix.transpose() - Creates a new, transposed version of the matrix by interchanging the row and column index of each element.
matrix.pinv() - Returns the pseudoinverse of a matrix.
matrix.inv() - Returns the inverse of a square matrix.
matrix.rank() - Calculates the rank of the matrix.
matrix.trace() - Calculates the trace of a matrix (the sum of the main diagonal’s elements).
matrix.eigenvalues() - Returns an array containing the eigenvalues of a square matrix.
matrix.eigenvectors() - Returns a matrix of eigenvectors, in which each column is an eigenvector of the matrix.
matrix.kron() - Returns the Kronecker product for the two matrices.
matrix.is_zero() - Determines if all elements of the matrix are zero.
matrix.is_identity() - Determines if a matrix is an identity matrix (elements with ones on the main diagonal and zeros elsewhere).
matrix.is_binary() - Determines if the matrix is binary (when all elements of the matrix are 0 or 1).
matrix.is_symmetric() - Determines if a square matrix is symmetric (elements are symmetric with respect to the main diagonal).
matrix.is_antisymmetric() - Determines if a matrix is antisymmetric (its transpose equals its negative).
matrix.is_diagonal() - Determines if the matrix is diagonal (all elements outside the main diagonal are zero).
matrix.is_antidiagonal() - Determines if the matrix is anti-diagonal (all elements outside the secondary diagonal are zero).
matrix.is_triangular() - Determines if the matrix is triangular (if all elements above or below the main diagonal are zero).
matrix.is_stochastic() - Determines if the matrix is stochastic.
matrix.is_square() - Determines if the matrix is square (it has the same number of rows and columns).
Added a new parameter for the strategy() function:

risk_free_rate - The risk-free rate of return is the annual percentage change in the value of an investment with minimal or zero risk, used to calculate the Sharpe and Sortino ratios.
March 2022
New array functions were added:

array.sort_indices() - returns an array of indices which, when used to index the original array, will access its elements in their sorted order.
array.percentrank() - returns the percentile rank of a value in the array.
array.percentile_nearest_rank() - returns the value for which the specified percentage of array values (percentile) are less than or equal to it, using the nearest-rank method.
array.percentile_linear_interpolation() - returns the value for which the specified percentage of array values (percentile) are less than or equal to it, using linear interpolation.
array.abs() - returns an array containing the absolute value of each element in the original array.
array.binary_search() - returns the index of the value, or -1 if the value is not found.
array.binary_search_leftmost() - returns the index of the value if it is found or the index of the next smallest element to the left of where the value would lie if it was in the array.
array.binary_search_rightmost() - returns the index of the value if it is found or the index of the element to the right of where the value would lie if it was in the array.
Added a new optional nth parameter for the array.min() and array.max() functions.

Added index in for..in operator. It tracks the current iteration’s index.

Table merging and cell tooltips
It is now possible to merge several cells in a table. A merged cell doesn’t have to be a header: you can merge cells in any direction, as long as the resulting cell doesn’t affect any already merged cells and doesn’t go outside of the table’s bounds. Cells can be merged with the new table.merge_cells() function.
Tables now support tooltips, floating labels that appear when you hover over a table’s cell. To add a tooltip, pass a string to the tooltip argument of the table.cell() function or use the new table.cell_set_tooltip() function.
February 2022
Added templates and the ability to create arrays via templates. Instead of using one of the array.new_*() functions, a template function array.new<type> can be used. In the example below, we use this functionality to create an array filled with float values:

//@version=5
indicator("array.new<float> example")
length = 5
var a = array.new<float>(length, close)
if array.size(a) == length
        array.remove(a, 0)
        array.push(a, close)
plot(array.sum(a) / length, "SMA")
New functions were added:

timeframe.in_seconds(timeframe) - converts the timeframe passed to the timeframe argument into seconds.
input.text_area() - adds multiline text input area to the Script settings.
strategy.closedtrades.entry_id() - returns the id of the closed trade’s entry.
strategy.closedtrades.exit_id() - returns the id of the closed trade’s exit.
strategy.opentrades.entry_id() - returns the id of the open trade’s entry.
January 2022
Added new functions to clone drawings:

line.copy()
label.copy()
box.copy()
2021
December 2021
Linefills
The space between lines drawn in Pine Script™ can now be filled! We’ve added a new linefill drawing type, along with a number of functions dedicated to manipulating it. Linefills are created by passing two lines and a color to the linefill.new() function, and their behavior is based on the lines they’re tied to: they extend in the same direction as the lines, move when their lines move, and are deleted when one of the two lines is deleted.

New linefill-related functions:

array.new_linefill()
linefill()
linefill.delete()
linefill.get_line1()
linefill.get_line2()
linefill.new()
linefill.set_color()
linefill.all()
New functions for string manipulation
Added a number of new functions that provide more ways to process strings, and introduce regular expressions to Pine Script™:

str.contains(source, str) - Determines if the source string contains the str substring.
str.pos(source, str) - Returns the position of the str string in the source string.
str.substring(source, begin_pos, end_pos) - Extracts a substring from the source string.
str.replace(source, target, replacement, occurrence) - Contrary to the existing str.replace_all() function, str.replace() allows the selective replacement of a matched substring with a replacement string.
str.lower(source) and str.upper(source) - Convert all letters of the source string to lower or upper case:
str.startswith(source, str) and str.endswith(source, str) - Determine if the source string starts or ends with the str substring.
str.match(source, regex) - Extracts the substring matching the specified regular expression.
Textboxes
Box drawings now supports text. The box.new() function has five new parameters for text manipulation: text, text_size, text_color, text_valign, and text_halign. Additionally, five new functions to set the text properties of existing boxes were added:

box.set_text()
box.set_text_color()
box.set_text_size()
box.set_text_valign()
box.set_text_halign()
New built-in variables
Added new built-in variables that return the bar_index and time values of the last bar in the dataset. Their values are known at the beginning of the script’s calculation:

last_bar_index - Bar index of the last chart bar.
last_bar_time - UNIX time of the last chart bar.
New built-in source variable:

hlcc4 - A shortcut for (high + low + close + close)/4. It averages the high and low values with the double-weighted close.
November 2021
for…in
Added a new for…in operator to iterate over all elements of an array:

//@version=5
indicator("My Script")
int[] a1 = array.from(1, 3, 6, 3, 8, 0, -9, 5)

highest(array) =>
    var int highestNum = na
    for item in array
        if na(highestNum) or item > highestNum
            highestNum := item
    highestNum

plot(highest(a1))
Function overloads
Added function overloads. Several functions in a script can now share the same name, as long one of the following conditions is true:

Each overload has a different number of parameters:

//@version=5
indicator("Function overload")

// Two parameters
mult(x1, x2) =>
    x1 * x2

// Three parameters
mult(x1, x2, x3) =>
    x1 * x2 * x3

plot(mult(7, 4))
plot(mult(7, 4, 2))
When overloads have the same number of parameters, all parameters in each overload must be explicitly typified, and their type combinations must be unique:

//@version=5
indicator("Function overload")

// Accepts both 'int' and 'float' values - any 'int' can be automatically cast to 'float'
mult(float x1, float x2) =>
    x1 * x2

// Returns a 'bool' value instead of a number
mult(bool x1, bool x2) =>
    x1 and x2 ? true : false

mult(string x1, string x2) =>
    str.tonumber(x1) * str.tonumber(x2)

// Has three parameters, so explicit types are not required
mult(x1, x2, x3) =>
    x1 * x2 * x3

plot(mult(7, 4))
plot(mult(7.5, 4.2))
plot(mult(true, false) ? 1 : 0)
plot(mult("5", "6"))
plot(mult(7, 4, 2))
Currency conversion
Added a new currency argument to most request.*() functions. If specified, price values returned by the function will be converted from the source currency to the target currency. The following functions are affected:

request.dividends()
request.earnings()
request.financial()
request.security()
October 2021
Pine Script™ v5 is here! This is a list of the new features added to the language, and a few of the changes made. See the Migration guide to Pine Script™ v5 for a complete list of the changes in v5.

New features
Libraries are a new type of publication. They allow you to create custom functions for reuse in other scripts. See this manual’s page on Libraries.

Pine Script™ now supports switch structures! They provide a more convenient and readable alternative to long ternary operators and if statements.

while loops are here! They allow you to create a loop that will only stop when its controlling condition is false, or a break command is used in the loop.

New built-in array variables are maintained by the Pine Script™ runtime to hold the IDs of all the active objects of the same type drawn by your script. They are label.all, line.all, box.all and table.all.

The runtime.error() function makes it possible to halt the execution of a script and display a runtime error with a custom message. You can use any condition in your script to trigger the call.

Parameter definitions in user-defined functions can now include a default value: a function defined as f(x = 1) => x will return 1 when called as f(), i.e., without providing an argument for its x parameter.

New variables and functions provide better script visibility on strategy information:

strategy.closedtrades.entry_price() and strategy.opentrades.entry_price()
strategy.closedtrades.entry_bar_index() and strategy.opentrades.entry_bar_index()
strategy.closedtrades.entry_time() and strategy.opentrades.entry_time()
strategy.closedtrades.size() and strategy.opentrades.size()
strategy.closedtrades.profit() and strategy.opentrades.profit()
strategy.closedtrades.commission() and strategy.opentrades.commission()
strategy.closedtrades.max_runup() and strategy.opentrades.max_runup()
strategy.closedtrades.max_drawdown() and strategy.opentrades.max_drawdown()
strategy.closedtrades.exit_price()
strategy.closedtrades.exit_bar_index()
strategy.closedtrades.exit_time()
strategy.convert_to_account()
strategy.convert_to_symbol()
strategy.account_currency
A new earnings.standardized constant for the request.earnings() function allows requesting standardized earnings data.

A v4 to v5 converter is now included in the Pine Script™ Editor. See the Migration guide to Pine Script™ v5 for more information on converting your scripts to v5.

The Reference Manual now includes the systematic mention of the form and type (e.g., “simple int”) required for each function parameter.

The User Manual was reorganized and new content was added.

Changes
Many built-in variables, functions and function arguments were renamed or moved to new namespaces in v5. The venerable study(), for example, is now indicator(), and security() is now request.security(). New namespaces now group related functions and variables together. This consolidation implements a more rational nomenclature and provides an orderly space to accommodate the many additions planned for Pine Script™.

See the Migration guide to Pine Script™ v5 for a complete list of the changes made in v5.

September 2021
New parameter has been added for the dividends(), earnings(), financial(), quandl(), security(), and splits() functions:

ignore_invalid_symbol - determines the behavior of the function if the specified symbol is not found: if false, the script will halt and return a runtime error; if true, the function will return na and execution will continue.