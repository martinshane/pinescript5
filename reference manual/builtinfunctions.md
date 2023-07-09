Built-In Functions
alert

Creates an alert event when called during the real-time bar, which will trigger a script alert based on "alert function events" if one was previously created for the indicator or strategy through the "Create Alert" dialog box.
alert(message, freq) → void
EXAMPLE

//@version=5
indicator("`alert()` example", "", true)
ma = ta.sma(close, 14)
xUp = ta.crossover(close, ma)
if xUp
    // Trigger the alert the first time a cross occurs during the real-time bar.
    alert("Price (" + str.tostring(close) + ") crossed over MA (" + str.tostring(ma) +  ").", alert.freq_once_per_bar)
plot(ma)
plotchar(xUp, "xUp", "▲", location.top, size = size.tiny)
ARGUMENTS
message (series string) Message sent when the alert triggers. Required argument.
freq (input string) The triggering frequency. Possible values are: alert.freq_all (all function calls trigger the alert), alert.freq_once_per_bar (the first function call during the bar triggers the alert), alert.freq_once_per_bar_close (the function call triggers the alert only when it occurs during the last script iteration of the real-time bar, when it closes). The default is alert.freq_once_per_bar.
REMARKS
The Help Center explains how to create such alerts.
Contrary to alertcondition, alert calls do NOT count as an additional plot.
Function calls can be located in both global and local scopes.
Function calls do not display anything on the chart.
The 'freq' argument only affects the triggering frequency of the function call where it is used.
SEE ALSO
alertcondition
alertcondition

Creates alert condition, that is available in Create Alert dialog. Please note, that alertcondition does NOT create an alert, it just gives you more options in Create Alert dialog. Also, alertcondition effect is invisible on chart.
alertcondition(condition, title, message) → void
EXAMPLE

//@version=5
indicator("alertcondition", overlay=true)
alertcondition(close >= open, title='Alert on Green Bar', message='Green Bar!')
ARGUMENTS
condition (series bool) Series of boolean values that is used for alert. True values mean alert fire, false - no alert. Required argument.
title (const string) Title of the alert condition. Optional argument.
message (const string) Message to display when alert fires. Optional argument.
REMARKS
Please note that an alertcondition call generates an additional plot. All such calls are taken into account when we calculate the number of the output series per script.
SEE ALSO
alert
array.abs

Returns an array containing the absolute value of each element in the original array.
array.abs(id) → float[]
array.abs(id) → int[]
ARGUMENTS
id (int[]/float[]) An array object.
SEE ALSO
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.avg

The function returns the mean of an array's elements.
array.avg(id) → series float
array.avg(id) → series int
EXAMPLE

//@version=5
indicator("array.avg example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.avg(a))
RETURNS
Mean of array's elements.
ARGUMENTS
id (int[]/float[]) An array object.
SEE ALSO
array.new_float
array.max
array.min
array.stdev
array.binary_search

The function returns the index of the value, or -1 if the value is not found. The array to search must be sorted in ascending order.
array.binary_search(id, val) → series int
EXAMPLE

//@version=5
indicator("array.binary_search")
a = array.from(5, -2, 0, 9, 1)
array.sort(a) // [-2, 0, 1, 5, 9]
position = array.binary_search(a, 0) // 1
plot(position)
ARGUMENTS
id (int[]/float[]) An array object.
val (series int/float) The value to search for in the array.
REMARKS
A binary search works on arrays pre-sorted in ascending order. It begins by comparing an element in the middle of the array with the target value. If the element matches the target value, its position in the array is returned. If the element's value is greater than the target value, the search continues in the lower half of the array. If the element's value is less than the target value, the search continues in the upper half of the array. By doing this recursively, the algorithm progressively eliminates smaller and smaller portions of the array in which the target value cannot lie.
SEE ALSO
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.binary_search_leftmost

The function returns the index of the value if it is found. When the value is not found, the function returns the index of the next smallest element to the left of where the value would lie if it was in the array. The array to search must be sorted in ascending order.
array.binary_search_leftmost(id, val) → series int
EXAMPLE

//@version=5
indicator("array.binary_search_leftmost")
a = array.from(5, -2, 0, 9, 1)
array.sort(a) // [-2, 0, 1, 5, 9]
position = array.binary_search_leftmost(a, 3) // 2
plot(position)
EXAMPLE

//@version=5
indicator("array.binary_search_leftmost, repetitive elements")
a = array.from(4, 5, 5, 5)
// Returns the index of the first instance.
position = array.binary_search_leftmost(a, 5) 
plot(position) // Plots 1
ARGUMENTS
id (int[]/float[]) An array object.
val (series int/float) The value to search for in the array.
REMARKS
A binary search works on arrays pre-sorted in ascending order. It begins by comparing an element in the middle of the array with the target value. If the element matches the target value, its position in the array is returned. If the element's value is greater than the target value, the search continues in the lower half of the array. If the element's value is less than the target value, the search continues in the upper half of the array. By doing this recursively, the algorithm progressively eliminates smaller and smaller portions of the array in which the target value cannot lie.
SEE ALSO
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.binary_search_rightmost

The function returns the index of the value if it is found. When the value is not found, the function returns the index of the element to the right of where the value would lie if it was in the array. The array must be sorted in ascending order.
array.binary_search_rightmost(id, val) → series int
EXAMPLE

//@version=5
indicator("array.binary_search_rightmost")
a = array.from(5, -2, 0, 9, 1)
array.sort(a) // [-2, 0, 1, 5, 9]
position = array.binary_search_rightmost(a, 3) // 3
plot(position)
EXAMPLE

//@version=5
indicator("array.binary_search_rightmost, repetitive elements")
a = array.from(4, 5, 5, 5)
// Returns the index of the last instance.
position = array.binary_search_rightmost(a, 5) 
plot(position) // Plots 3
ARGUMENTS
id (int[]/float[]) An array object.
val (series int/float) The value to search for in the array.
REMARKS
A binary search works on sorted arrays in ascending order. It begins by comparing an element in the middle of the array with the target value. If the element matches the target value, its position in the array is returned. If the element's value is greater than the target value, the search continues in the lower half of the array. If the element's value is less than the target value, the search continues in the upper half of the array. By doing this recursively, the algorithm progressively eliminates smaller and smaller portions of the array in which the target value cannot lie.
SEE ALSO
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.clear

The function removes all elements from an array.
array.clear(id) → void
EXAMPLE

//@version=5
indicator("array.clear example")
a = array.new_float(5,high)
array.clear(a)
array.push(a, close)
plot(array.get(a,0))
plot(array.size(a))
ARGUMENTS
id (any array type) An array object.
SEE ALSO
array.new_float
array.insert
array.push
array.remove
array.pop
array.concat

The function is used to merge two arrays. It pushes all elements from the second array to the first array, and returns the first array.
array.concat(id1, id2) → array<type>
EXAMPLE

//@version=5
indicator("array.concat example")
a = array.new_float(0,0)
b = array.new_float(0,0)
for i = 0 to 4
    array.push(a, high[i])
    array.push(b, low[i])
c = array.concat(a,b)
plot(array.size(a))
plot(array.size(b))
plot(array.size(c))
RETURNS
The first array with merged elements from the second array.
ARGUMENTS
id1 (any array type) The first array object.
id2 (any array type) The second array object.
SEE ALSO
array.new_float
array.insert
array.slice
array.copy

The function creates a copy of an existing array.
array.copy(id) → array<type>
EXAMPLE

//@version=5
indicator("array.copy example")
length = 5
a = array.new_float(length, close)
b = array.copy(a)
a := array.new_float(length, open)
plot(array.sum(a) / length)
plot(array.sum(b) / length)
RETURNS
A copy of an array.
ARGUMENTS
id (any array type) An array object.
SEE ALSO
array.new_float
array.get
array.slice
array.sort
array.covariance

The function returns the covariance of two arrays.
array.covariance(id1, id2, biased) → series float
EXAMPLE

//@version=5
indicator("array.covariance example")
a = array.new_float(0)
b = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
    array.push(b, open[i])
plot(array.covariance(a, b))
RETURNS
The covariance of two arrays.
ARGUMENTS
id1 (int[]/float[]) An array object.
id2 (int[]/float[]) An array object.
biased (series bool) Determines which estimate should be used. Optional. The default is true.
REMARKS
If `biased` is true, function will calculate using a biased estimate of the entire population, if false - unbiased estimate of a sample.
SEE ALSO
array.new_float
array.max
array.stdev
array.avg
array.variance
array.every

Returns true if all elements of the `id` array are true, false otherwise.
array.every(id) → series bool
ARGUMENTS
id (bool[]) An array object.
REMARKS
This function also works with arrays of int and float types, in which case zero values are considered false, and all others true.
SEE ALSO
array.some
array.get
array.fill

The function sets elements of an array to a single value. If no index is specified, all elements are set. If only a start index (default 0) is supplied, the elements starting at that index are set. If both index parameters are used, the elements from the starting index up to but not including the end index (default na) are set.
array.fill(id, value, index_from, index_to) → void
EXAMPLE

//@version=5
indicator("array.fill example")
a = array.new_float(10)
array.fill(a, close)
plot(array.sum(a))
ARGUMENTS
id (any array type) An array object.
value (series <type of the array's elements>) Value to fill the array with.
index_from (series int) Start index, default is 0.
index_to (series int) End index, default is na. Must be one greater than the index of the last element to set.
SEE ALSO
array.new_float
array.set
array.slice
array.first

Returns the array's first element. Throws a runtime error if the array is empty.
array.first(id) → series <type>
EXAMPLE

//@version=5
indicator("array.first example")
arr = array.new_int(3, 10)
plot(array.first(arr))
ARGUMENTS
id (any array type) An array object.
SEE ALSO
array.last
array.get
array.from

The function takes a variable number of arguments with one of the types: int, float, bool, string, label, line, color, box, table, linefill, and returns an array of the corresponding type.
array.from(arg0, arg1, ...) → int[]
array.from(arg0, arg1, ...) → float[]
array.from(arg0, arg1, ...) → bool[]
array.from(arg0, arg1, ...) → string[]
array.from(arg0, arg1, ...) → label[]
array.from(arg0, arg1, ...) → line[]
array.from(arg0, arg1, ...) → color[]
array.from(arg0, arg1, ...) → box[]
array.from(arg0, arg1, ...) → table[]
array.from(arg0, arg1, ...) → linefill[]
array.from(arg0, arg1, ...) → type[]
EXAMPLE

//@version=5
indicator("array.from_example", overlay = false)
arr = array.from("Hello", "World!") // arr (string[]) will contain 2 elements: {Hello}, {World!}.
plot(close)
RETURNS
The array element's value.
ARGUMENTS
arg0, arg1, ... (series int/float/bool/color/string/label/line/box/table/linefill) Array arguments.
array.get

The function returns the value of the element at the specified index.
array.get(id, index) → series <type>
EXAMPLE

//@version=5
indicator("array.get example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i] - open[i])
plot(array.get(a, 9))
RETURNS
The array element's value.
ARGUMENTS
id (any array type) An array object.
index (series int) The index of the element whose value is to be returned.
SEE ALSO
array.new_float
array.set
array.slice
array.sort
array.includes

The function returns true if the value was found in an array, false otherwise.
array.includes(id, value) → series bool
EXAMPLE

//@version=5
indicator("array.includes example")
a = array.new_float(5,high)
p = close
if array.includes(a, high)
    p := open
plot(p)
RETURNS
True if the value was found in the array, false otherwise.
ARGUMENTS
id (any array type) An array object.
value (series <type of the array's elements>) The value to search in the array.
SEE ALSO
array.new_float
array.indexof
array.shift
array.remove
array.insert
array.indexof

The function returns the index of the first occurrence of the value, or -1 if the value is not found.
array.indexof(id, value) → series int
EXAMPLE

//@version=5
indicator("array.indexof example")
a = array.new_float(5,high)
index = array.indexof(a, high)
plot(index)
RETURNS
The index of an element.
ARGUMENTS
id (any array type) An array object.
value (series <type of the array's elements>) The value to search in the array.
SEE ALSO
array.lastindexof
array.get
array.lastindexof
array.remove
array.insert
array.insert

The function changes the contents of an array by adding new elements in place.
array.insert(id, index, value) → void
EXAMPLE

//@version=5
indicator("array.insert example")
a = array.new_float(5, close)
array.insert(a, 0, open)
plot(array.get(a, 5))
ARGUMENTS
id (any array type) An array object.
index (series int) The index at which to insert the value.
value (series <type of the array's elements>) The value to add to the array.
SEE ALSO
array.new_float
array.set
array.push
array.remove
array.pop
array.unshift
array.join

The function creates and returns a new string by concatenating all the elements of an array, separated by the specified separator string.
array.join(id, separator) → series string
EXAMPLE

//@version=5
indicator("array.join example")
a = array.new_float(5, 5)
label.new(bar_index, close, array.join(a, ","))
ARGUMENTS
id (int[]/float[]/string[]) An array object.
separator (series string) The string used to separate each array element.
SEE ALSO
array.new_float
array.set
array.insert
array.remove
array.pop
array.unshift
array.last

Returns the array's last element. Throws a runtime error if the array is empty.
array.last(id) → series <type>
EXAMPLE

//@version=5
indicator("array.last example")
arr = array.new_int(3, 10)
plot(array.last(arr))
ARGUMENTS
id (any array type) An array object.
SEE ALSO
array.first
array.get
array.lastindexof

The function returns the index of the last occurrence of the value, or -1 if the value is not found.
array.lastindexof(id, value) → series int
EXAMPLE

//@version=5
indicator("array.lastindexof example")
a = array.new_float(5,high)
index = array.lastindexof(a, high)
plot(index)
RETURNS
The index of an element.
ARGUMENTS
id (any array type) An array object.
value (series <type of the array's elements>) The value to search in the array.
SEE ALSO
array.new_float
array.set
array.push
array.remove
array.insert
array.max

The function returns the greatest value, or the nth greatest value in a given array.
array.max(id) → series float
array.max(id) → series int
array.max(id, nth) → series float
array.max(id, nth) → series int
EXAMPLE

//@version=5
indicator("array.max")
a = array.from(5, -2, 0, 9, 1)
thirdHighest = array.max(a, 2) // 1
plot(thirdHighest)
RETURNS
The greatest or the nth greatest value in the array.
ARGUMENTS
id (int[]/float[]) An array object.
nth (series int) The nth greatest value to return, where zero is the greatest. Optional. The default is zero.
SEE ALSO
array.new_float
array.min
array.sum
array.median

The function returns the median of an array's elements.
array.median(id) → series float
array.median(id) → series int
EXAMPLE

//@version=5
indicator("array.median example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.median(a))
RETURNS
The median of the array's elements.
ARGUMENTS
id (int[]/float[]) An array object.
SEE ALSO
array.median
array.avg
array.variance
array.min
array.min

The function returns the smallest value, or the nth smallest value in a given array.
array.min(id) → series float
array.min(id) → series int
array.min(id, nth) → series float
array.min(id, nth) → series int
EXAMPLE

//@version=5
indicator("array.min")
a = array.from(5, -2, 0, 9, 1)
secondLowest = array.min(a, 1) // 0
plot(secondLowest)
RETURNS
The smallest or the nth smallest value in the array.
ARGUMENTS
id (int[]/float[]) An array object.
nth (series int) The nth smallest value to return, where zero is the smallest. Optional. The default is zero.
SEE ALSO
array.new_float
array.max
array.sum
array.mode

The function returns the mode of an array's elements. If there are several values with the same frequency, it returns the smallest value.
array.mode(id) → series float
array.mode(id) → series int
EXAMPLE

//@version=5
indicator("array.mode example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.mode(a))
RETURNS
The most frequently occurring value from the `id` array. If none exists, returns the smallest value instead.
ARGUMENTS
id (int[]/float[]) An array object.
SEE ALSO
array.new_float
ta.mode
matrix.mode
array.avg
array.variance
array.min
array.new<type>

The function creates a new array object of <type> elements.
array.new<type>(size, initial_value) → array<type>
EXAMPLE

//@version=5
indicator("array.new<string> example")
a = array.new<string>(1, "Hello, World!")
label.new(bar_index, close, array.get(a, 0))
EXAMPLE

//@version=5
indicator("array.new<color> example")
a = array.new<color>()
array.push(a, color.red)
array.push(a, color.green)
plot(close, color = array.get(a, close > open ? 1 : 0))
EXAMPLE

//@version=5
indicator("array.new<float> example")
length = 5
var a = array.new<float>(length, close)
if array.size(a) == length
    array.remove(a, 0)
    array.push(a, close)
plot(array.sum(a) / length, "SMA")
EXAMPLE

//@version=5
indicator("array.new<line> example")
// draw last 15 lines
var a = array.new<line>()
array.push(a, line.new(bar_index - 1, close[1], bar_index, close))
if array.size(a) > 15
    ln = array.shift(a)
    line.delete(ln)
RETURNS
The ID of an array object which may be used in other array.*() functions.
ARGUMENTS
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series <type>) Initial value of all array elements. Optional. The default is 'na'.
REMARKS
An array index starts from 0.
If you want to initialize an array and specify all its elements at the same time, then use the function array.from.
SEE ALSO
array.from
array.push
array.get
array.size
array.remove
array.shift
array.sum
array.new_bool

The function creates a new array object of bool type elements.
array.new_bool(size, initial_value) → bool[]
EXAMPLE

//@version=5
indicator("array.new_bool example")
length = 5
a = array.new_bool(length, close > open)
plot(array.get(a, 0) ? close : open)
RETURNS
The ID of an array object which may be used in other array.*() functions.
ARGUMENTS
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series bool) Initial value of all array elements. Optional. The default is 'na'.
REMARKS
An array index starts from 0.
SEE ALSO
array.new_float
array.get
array.slice
array.sort
array.new_box

The function creates a new array object of box type elements.
array.new_box(size, initial_value) → box[]
EXAMPLE

//@version=5
indicator("array.new_box example")
box[] boxes = array.new_box()
array.push(boxes, box.new(time, close, time+2, low, xloc=xloc.bar_time))
plot(1)
RETURNS
The ID of an array object which may be used in other array.*() functions.
ARGUMENTS
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series box) Initial value of all array elements. Optional. The default is 'na'.
REMARKS
An array index starts from 0.
SEE ALSO
array.new_float
array.get
array.slice
array.new_color

The function creates a new array object of color type elements.
array.new_color(size, initial_value) → color[]
EXAMPLE

//@version=5
indicator("array.new_color example")
length = 5
a = array.new_color(length, color.red)
plot(close, color = array.get(a, 0))
RETURNS
The ID of an array object which may be used in other array.*() functions.
ARGUMENTS
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series color) Initial value of all array elements. Optional. The default is 'na'.
REMARKS
An array index starts from 0.
SEE ALSO
array.new_float
array.get
array.slice
array.sort
array.new_float

The function creates a new array object of float type elements.
array.new_float(size, initial_value) → float[]
EXAMPLE

//@version=5
indicator("array.new_float example")
length = 5
a = array.new_float(length, close)
plot(array.sum(a) / length)
RETURNS
The ID of an array object which may be used in other array.*() functions.
ARGUMENTS
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series int/float) Initial value of all array elements. Optional. The default is 'na'.
REMARKS
An array index starts from 0.
SEE ALSO
array.new_color
array.new_bool
array.get
array.slice
array.sort
array.new_int

The function creates a new array object of int type elements.
array.new_int(size, initial_value) → int[]
EXAMPLE

//@version=5
indicator("array.new_int example")
length = 5
a = array.new_int(length, int(close))
plot(array.sum(a) / length)
RETURNS
The ID of an array object which may be used in other array.*() functions.
ARGUMENTS
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series int) Initial value of all array elements. Optional. The default is 'na'.
REMARKS
An array index starts from 0.
SEE ALSO
array.new_float
array.get
array.slice
array.sort
array.new_label

The function creates a new array object of label type elements.
array.new_label(size, initial_value) → label[]
EXAMPLE

//@version=5
indicator("array.new_label example")
var a = array.new_label()
l = label.new(bar_index, close, "some text")
array.push(a, l)
if close > close[1] and close[1] > close[2]
    // remove all labels
    size = array.size(a) - 1
    for i = 0 to size
        lb = array.get(a, i)
        label.delete(lb)
RETURNS
The ID of an array object which may be used in other array.*() functions.
ARGUMENTS
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series label) Initial value of all array elements. Optional. The default is 'na'.
REMARKS
An array index starts from 0.
SEE ALSO
array.new_float
array.get
array.slice
array.new_line

The function creates a new array object of line type elements.
array.new_line(size, initial_value) → line[]
EXAMPLE

//@version=5
indicator("array.new_line example")
// draw last 15 lines
var a = array.new_line()
array.push(a, line.new(bar_index - 1, close[1], bar_index, close))
if array.size(a) > 15
    ln = array.shift(a)
    line.delete(ln)
RETURNS
The ID of an array object which may be used in other array.*() functions.
ARGUMENTS
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series line) Initial value of all array elements. Optional. The default is 'na'.
REMARKS
An array index starts from 0.
SEE ALSO
array.new_float
array.get
array.slice
array.new_linefill

The function creates a new array object of linefill type elements.
array.new_linefill(size, initial_value) → linefill[]
RETURNS
The ID of an array object which may be used in other array.*() functions.
ARGUMENTS
size (series int) Initial size of an array.
initial_value (series linefill) Initial value of all array elements.
REMARKS
An array index starts from 0.
array.new_string

The function creates a new array object of string type elements.
array.new_string(size, initial_value) → string[]
EXAMPLE

//@version=5
indicator("array.new_string example")
length = 5
a = array.new_string(length, "text")
label.new(bar_index, close, array.get(a, 0))
RETURNS
The ID of an array object which may be used in other array.*() functions.
ARGUMENTS
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series string) Initial value of all array elements. Optional. The default is 'na'.
REMARKS
An array index starts from 0.
SEE ALSO
array.new_float
array.get
array.slice
array.new_table

The function creates a new array object of table type elements.
array.new_table(size, initial_value) → table[]
EXAMPLE

//@version=5
indicator("table array")
table[] tables = array.new_table()
array.push(tables, table.new(position = position.top_left, rows = 1, columns = 2, bgcolor = color.yellow, border_width=1))
plot(1)
RETURNS
The ID of an array object which may be used in other array.*() functions.
ARGUMENTS
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series table) Initial value of all array elements. Optional. The default is 'na'.
REMARKS
An array index starts from 0.
SEE ALSO
array.new_float
array.get
array.slice
array.percentile_linear_interpolation

Returns the value for which the specified percentage of array values (percentile) are less than or equal to it, using linear interpolation.
array.percentile_linear_interpolation(id, percentage) → series float
array.percentile_linear_interpolation(id, percentage) → series int
ARGUMENTS
id (int[]/float[]) An array object.
percentage (series int/float) The percentage of values that must be equal or less than the returned value.
REMARKS
In statistics, the percentile is the percent of ranking items that appear at or below a certain score. This measurement shows the percentage of scores within a standard frequency distribution that is lower than the percentile rank you're measuring. Linear interpolation estimates the value between two ranks.
SEE ALSO
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.percentile_nearest_rank

Returns the value for which the specified percentage of array values (percentile) are less than or equal to it, using the nearest-rank method.
array.percentile_nearest_rank(id, percentage) → series float
array.percentile_nearest_rank(id, percentage) → series int
ARGUMENTS
id (int[]/float[]) An array object.
percentage (series int/float) The percentage of values that must be equal or less than the returned value.
REMARKS
In statistics, the percentile is the percent of ranking items that appear at or below a certain score. This measurement shows the percentage of scores within a standard frequency distribution that is lower than the percentile rank you're measuring.
SEE ALSO
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.percentrank

Returns the percentile rank of the element at the specified `index`.
array.percentrank(id, index) → series float
array.percentrank(id, index) → series int
ARGUMENTS
id (int[]/float[]) An array object.
index (series int) The index of the element for which the percentile rank should be calculated.
REMARKS
Percentile rank is the percentage of how many elements in the array are less than or equal to the reference value.
SEE ALSO
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.pop

The function removes the last element from an array and returns its value.
array.pop(id) → series <type>
EXAMPLE

//@version=5
indicator("array.pop example")
a = array.new_float(5,high)
removedEl = array.pop(a)
plot(array.size(a))
plot(removedEl)
RETURNS
The value of the removed element.
ARGUMENTS
id (any array type) An array object.
SEE ALSO
array.new_float
array.set
array.push
array.remove
array.insert
array.shift
array.push

The function appends a value to an array.
array.push(id, value) → void
EXAMPLE

//@version=5
indicator("array.push example")
a = array.new_float(5, 0)
array.push(a, open)
plot(array.get(a, 5))
ARGUMENTS
id (any array type) An array object.
value (series <type of the array's elements>) The value of the element added to the end of the array.
SEE ALSO
array.new_float
array.set
array.insert
array.remove
array.pop
array.unshift
array.range

The function returns the difference between the min and max values from a given array.
array.range(id) → series float
array.range(id) → series int
EXAMPLE

//@version=5
indicator("array.range example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.range(a))
RETURNS
The difference between the min and max values in the array.
ARGUMENTS
id (int[]/float[]) An array object.
SEE ALSO
array.new_float
array.min
array.max
array.sum
array.remove

The function changes the contents of an array by removing the element with the specified index.
array.remove(id, index) → series <type>
EXAMPLE

//@version=5
indicator("array.remove example")
a = array.new_float(5,high)
removedEl = array.remove(a, 0)
plot(array.size(a))
plot(removedEl)
RETURNS
The value of the removed element.
ARGUMENTS
id (any array type) An array object.
index (series int) The index of the element to remove.
SEE ALSO
array.new_float
array.set
array.push
array.insert
array.pop
array.shift
array.reverse

The function reverses an array. The first array element becomes the last, and the last array element becomes the first.
array.reverse(id) → void
EXAMPLE

//@version=5
indicator("array.reverse example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.get(a, 0))
array.reverse(a)
plot(array.get(a, 0))
ARGUMENTS
id (any array type) An array object.
SEE ALSO
array.new_float
array.sort
array.push
array.set
array.avg
array.set

The function sets the value of the element at the specified index.
array.set(id, index, value) → void
EXAMPLE

//@version=5
indicator("array.set example")
a = array.new_float(10)
for i = 0 to 9
    array.set(a, i, close[i])
plot(array.sum(a) / 10)
ARGUMENTS
id (any array type) An array object.
index (series int) The index of the element to be modified.
value (series <type of the array's elements>) The new value to be set.
SEE ALSO
array.new_float
array.get
array.slice
array.shift

The function removes an array's first element and returns its value.
array.shift(id) → series <type>
EXAMPLE

//@version=5
indicator("array.shift example")
a = array.new_float(5,high)
removedEl = array.shift(a)
plot(array.size(a))
plot(removedEl)
RETURNS
The value of the removed element.
ARGUMENTS
id (any array type) An array object.
SEE ALSO
array.unshift
array.set
array.push
array.remove
array.includes
array.size

The function returns the number of elements in an array.
array.size(id) → series int
EXAMPLE

//@version=5
indicator("array.size example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
// note that changes in slice also modify original array
slice = array.slice(a, 0, 5)
array.push(slice, open)
// size was changed in slice and in original array
plot(array.size(a))
plot(array.size(slice))
RETURNS
The number of elements in the array.
ARGUMENTS
id (any array type) An array object.
SEE ALSO
array.new_float
array.sum
array.slice
array.sort
array.slice

The function creates a slice from an existing array. If an object from the slice changes, the changes are applied to both the new and the original arrays.
array.slice(id, index_from, index_to) → array<type>
EXAMPLE

//@version=5
indicator("array.slice example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
// take elements from 0 to 4
// *note that changes in slice also modify original array 
slice = array.slice(a, 0, 5)
plot(array.sum(a) / 10)
plot(array.sum(slice) / 5)
RETURNS
A shallow copy of an array's slice.
ARGUMENTS
id (any array type) An array object.
index_from (series int) Zero-based index at which to begin extraction.
index_to (series int) Zero-based index before which to end extraction. The function extracts up to but not including the element with this index.
SEE ALSO
array.new_float
array.get
array.slice
array.sort
array.some

Returns true if at least one element of the `id` array is true, false otherwise.
array.some(id) → series bool
ARGUMENTS
id (bool[]) An array object.
REMARKS
This function also works with arrays of int and float types, in which case zero values are considered false, and all others true.
SEE ALSO
array.every
array.get
array.sort

The function sorts the elements of an array.
array.sort(id, order) → void
EXAMPLE

//@version=5
indicator("array.sort example")
a = array.new_float(0,0)
for i = 0 to 5
    array.push(a, high[i])
array.sort(a, order.descending)
if barstate.islast
    label.new(bar_index, close, str.tostring(a))
ARGUMENTS
id (int[]/float[]/string[]) An array object.
order (simple sort_order) The sort order: order.ascending (default) or order.descending.
SEE ALSO
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.sort_indices

Returns an array of indices which, when used to index the original array, will access its elements in their sorted order. It does not modify the original array.
array.sort_indices(id, order) → int[]
EXAMPLE

//@version=5
indicator("array.sort_indices")
a = array.from(5, -2, 0, 9, 1)
sortedIndices = array.sort_indices(a) // [1, 2, 4, 0, 3]
indexOfSmallestValue = array.get(sortedIndices, 0) // 1
smallestValue = array.get(a, indexOfSmallestValue) // -2
plot(smallestValue)
ARGUMENTS
id (int[]/float[]/string[]) An array object.
order (series sort_order) The sort order: order.ascending or order.descending. Optional. The default is order.ascending.
SEE ALSO
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.standardize

The function returns the array of standardized elements.
array.standardize(id) → float[]
array.standardize(id) → int[]
EXAMPLE

//@version=5
indicator("array.standardize example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
b = array.standardize(a)
plot(array.min(b))
plot(array.max(b))
RETURNS
The array of standardized elements.
ARGUMENTS
id (int[]/float[]) An array object.
SEE ALSO
array.max
array.min
array.mode
array.avg
array.variance
array.stdev
array.stdev

The function returns the standard deviation of an array's elements.
array.stdev(id, biased) → series float
array.stdev(id, biased) → series int
EXAMPLE

//@version=5
indicator("array.stdev example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.stdev(a))
RETURNS
The standard deviation of the array's elements.
ARGUMENTS
id (int[]/float[]) An array object.
biased (series bool) Determines which estimate should be used. Optional. The default is true.
REMARKS
If `biased` is true, function will calculate using a biased estimate of the entire population, if false - unbiased estimate of a sample.
SEE ALSO
array.new_float
array.max
array.min
array.avg
array.sum

The function returns the sum of an array's elements.
array.sum(id) → series float
array.sum(id) → series int
EXAMPLE

//@version=5
indicator("array.sum example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.sum(a))
RETURNS
The sum of the array's elements.
ARGUMENTS
id (int[]/float[]) An array object.
SEE ALSO
array.new_float
array.max
array.min
array.unshift

The function inserts the value at the beginning of the array.
array.unshift(id, value) → void
EXAMPLE

//@version=5
indicator("array.unshift example")
a = array.new_float(5, 0)
array.unshift(a, open)
plot(array.get(a, 0))
ARGUMENTS
id (any array type) An array object.
value (series <type of the array's elements>) The value to add to the start of the array.
SEE ALSO
array.shift
array.set
array.insert
array.remove
array.indexof
array.variance

The function returns the variance of an array's elements.
array.variance(id, biased) → series float
array.variance(id, biased) → series int
EXAMPLE

//@version=5
indicator("array.variance example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.variance(a))
RETURNS
The variance of the array's elements.
ARGUMENTS
id (int[]/float[]) An array object.
biased (series bool) Determines which estimate should be used. Optional. The default is true.
REMARKS
If `biased` is true, function will calculate using a biased estimate of the entire population, if false - unbiased estimate of a sample.
SEE ALSO
array.new_float
array.stdev
array.min
array.avg
array.covariance
barcolor

Set color of bars.
barcolor(color, offset, editable, show_last, title, display) → void
EXAMPLE

//@version=5
indicator("barcolor example", overlay=true)
barcolor(close < open ? color.black : color.white)
ARGUMENTS
color (series color) Color of bars. You can use constants like 'red' or '#ff001a' as well as complex expressions like 'close >= open ? color.green : color.red'. Required argument.
offset (series int) Shifts the color series to the left or to the right on the given number of bars. Default is 0.
editable (const bool) If true then barcolor style will be editable in Format dialog. Default is true.
show_last (input int) If set, defines the number of bars (from the last bar back to the past) to fill on chart.
title (const string) Title of the barcolor. Optional argument.
display (input plot_simple_display) Controls where the barcolor is displayed. Possible values are: display.none, display.all. Default is display.all.
SEE ALSO
bgcolor
plot
fill
bgcolor

Fill background of bars with specified color.
bgcolor(color, offset, editable, show_last, title, display) → void
EXAMPLE

//@version=5
indicator("bgcolor example", overlay=true)
bgcolor(close < open ? color.new(color.red,70) : color.new(color.green, 70))
ARGUMENTS
color (series color) Color of the filled background. You can use constants like 'red' or '#ff001a' as well as complex expressions like 'close >= open ? color.green : color.red'. Required argument.
offset (series int) Shifts the color series to the left or to the right on the given number of bars. Default is 0.
editable (const bool) If true then bgcolor style will be editable in Format dialog. Default is true.
show_last (input int) If set, defines the number of bars (from the last bar back to the past) to fill on chart.
title (const string) Title of the bgcolor. Optional argument.
display (input plot_simple_display) Controls where the bgcolor is displayed. Possible values are: display.none, display.all. Default is display.all.
SEE ALSO
barcolor
plot
fill
bool

Casts na to bool
bool(x) → const bool
bool(x) → input bool
bool(x) → simple bool
bool(x) → series bool
RETURNS
The value of the argument after casting to bool.
SEE ALSO
float
int
color
string
line
label
box

Casts na to box.
box(x) → series box
RETURNS
The value of the argument after casting to box.
SEE ALSO
float
int
bool
color
string
line
label
box.copy

Clones the box object.
box.copy(id) → series box
EXAMPLE

//@version=5
indicator('Last 50 bars price ranges', overlay = true)
LOOKBACK = 50
highest = ta.highest(LOOKBACK)
lowest = ta.lowest(LOOKBACK)
if barstate.islastconfirmedhistory
    var BoxLast = box.new(bar_index[LOOKBACK], highest, bar_index, lowest, bgcolor = color.new(color.green, 80))
    var BoxPrev = box.copy(BoxLast)
    box.set_lefttop(BoxPrev, bar_index[LOOKBACK * 2], highest[50])
    box.set_rightbottom(BoxPrev, bar_index[LOOKBACK], lowest[50])
    box.set_bgcolor(BoxPrev, color.new(color.red, 80))
ARGUMENTS
id (series box) Box object.
SEE ALSO
box.new
box.delete
box.delete

Deletes the specified box object. If it has already been deleted, does nothing.
box.delete(id) → void
ARGUMENTS
id (series box) A box object to delete.
SEE ALSO
box.new
box.get_bottom

Returns the price value of the bottom border of the box.
box.get_bottom(id) → series float
RETURNS
The price value.
ARGUMENTS
id (series box) A box object.
SEE ALSO
box.new
box.set_bottom
box.get_left

Returns the bar index or the UNIX time (depending on the last value used for 'xloc') of the left border of the box.
box.get_left(id) → series int
RETURNS
A bar index or a UNIX timestamp (in milliseconds).
ARGUMENTS
id (series box) A box object.
SEE ALSO
box.new
box.set_left
box.get_right

Returns the bar index or the UNIX time (depending on the last value used for 'xloc') of the right border of the box.
box.get_right(id) → series int
RETURNS
A bar index or a UNIX timestamp (in milliseconds).
ARGUMENTS
id (series box) A box object.
SEE ALSO
box.new
box.set_right
box.get_top

Returns the price value of the top border of the box.
box.get_top(id) → series float
RETURNS
The price value.
ARGUMENTS
id (series box) A box object.
SEE ALSO
box.new
box.set_top
box.new

Creates a new box object.
box.new(left, top, right, bottom, border_color, border_width, border_style, extend, xloc, bgcolor, text, text_size, text_color, text_halign, text_valign, text_wrap, text_font_family) → series box
EXAMPLE

//@version=5
indicator("box.new")
var b = box.new(time, open, time + 60 * 60 * 24, close, xloc=xloc.bar_time, border_style=line.style_dashed)
box.set_lefttop(b, time, 100)
box.set_rightbottom(b, time + 60 * 60 * 24, 500)
box.set_bgcolor(b, color.green)
RETURNS
The ID of a box object which may be used in box.set_*() and box.get_*() functions.
ARGUMENTS
left (series int) Bar index (if xloc = xloc.bar_index) or UNIX time (if xloc = xloc.bar_time) of the left border of the box. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
top (series int/float) Price of the top border of the box.
right (series int) Bar index (if xloc = xloc.bar_index) or UNIX time (if xloc = xloc.bar_time) of the right border of the box. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
bottom (series int/float) Price of the bottom border of the box.
border_color (series color) Color of the four borders. Optional. The default is color.blue.
border_width (series int) Width of the four borders, in pixels. Optional. The default is 1 pixel.
border_style (series string) Style of the four borders. Possible values: line.style_solid, line.style_dotted, line.style_dashed. Optional. The default value is line.style_solid.
extend (series string) When extend.none is used, the horizontal borders start at the left border and end at the right border. With extend.left or extend.right, the horizontal borders are extended indefinitely to the left or right of the box, respectively. With extend.both, the horizontal borders are extended on both sides. Optional. The default value is extend.none.
xloc (series string) Determines whether the arguments to 'left' and 'right' are a bar index or a time value. If xloc = xloc.bar_index, the arguments must be a bar index. If xloc = xloc.bar_time, the arguments must be a UNIX time. Possible values: xloc.bar_index and xloc.bar_time. Optional. The default is xloc.bar_index.
bgcolor (series color) Background color of the box. Optional. The default is color.blue.
text (series string) The text to be displayed inside the box. Optional. The default is empty string.
text_size (series string) The size of the text. An optional parameter, the default value is size.auto. Possible values: size.auto, size.tiny, size.small, size.normal, size.large, size.huge.
text_font_family (series string) The font family of the text. Optional. The default value is font.family_default. Possible values: font.family_default, font.family_monospace.
text_color (series color) The color of the text. Optional. The default is color.black.
text_halign (series string) The horizontal alignment of the box's text. Optional. The default value is text.align_center. Possible values: text.align_left, text.align_center, text.align_right.
text_valign (series string) The vertical alignment of the box's text. Optional. The default value is text.align_center. Possible values: text.align_top, text.align_center, text.align_bottom.
text_wrap (series string) Defines whether the text is presented in a single line, extending past the width of the box if necessary, or wrapped so every line is no wider than the box itself (and clipped by the bottom border of the box if the height of the resulting wrapped text is higher than the height of the box). Optional. The default value is text.wrap_none. Possible values: text.wrap_none, text.wrap_auto.
SEE ALSO
box.delete
box.get_left
box.get_top
box.get_right
box.get_bottom
box.set_top_left
box.set_left
box.set_top
box.set_bottom_right
box.set_right
box.set_bottom
box.set_border_color
box.set_bgcolor
box.set_border_width
box.set_border_style
box.set_extend
box.set_bgcolor

Sets the background color of the box.
box.set_bgcolor(id, color) → void
ARGUMENTS
id (series box) A box object.
color (series color) New background color.
SEE ALSO
box.new
box.set_border_color

Sets the border color of the box.
box.set_border_color(id, color) → void
ARGUMENTS
id (series box) A box object.
color (series color) New border color.
SEE ALSO
box.new
box.set_border_style

Sets the border style of the box.
box.set_border_style(id, style) → void
ARGUMENTS
id (series box) A box object.
style (series string) New border style.
SEE ALSO
box.new
box.set_border_width

Sets the border width of the box.
box.set_border_width(id, width) → void
ARGUMENTS
id (series box) A box object.
width (series int) Width of the four borders, in pixels.
SEE ALSO
box.new
box.set_bottom

Sets the bottom coordinate of the box.
box.set_bottom(id, bottom) → void
ARGUMENTS
id (series box) A box object.
bottom (series int/float) Price value of the bottom border.
SEE ALSO
box.new
box.get_bottom
box.set_extend

Sets extending type of the border of this box object. When extend.none is used, the horizontal borders start at the left border and end at the right border. With extend.left or extend.right, the horizontal borders are extended indefinitely to the left or right of the box, respectively. With extend.both, the horizontal borders are extended on both sides.
box.set_extend(id, extend) → void
ARGUMENTS
id (series box) A box object.
extend (series string) New extending type.
SEE ALSO
box.new
box.set_left

Sets the left coordinate of the box.
box.set_left(id, left) → void
ARGUMENTS
id (series box) A box object.
left (series int) Bar index or bar time of the left border. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
SEE ALSO
box.new
box.get_left
box.set_lefttop

Sets the left and top coordinates of the box.
box.set_lefttop(id, left, top) → void
ARGUMENTS
id (series box) A box object.
left (series int) Bar index or bar time of the left border.
top (series int/float) Price value of the top border.
SEE ALSO
box.new
box.get_left
box.get_top
box.set_right

Sets the right coordinate of the box.
box.set_right(id, right) → void
ARGUMENTS
id (series box) A box object.
right (series int) Bar index or bar time of the right border. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
SEE ALSO
box.new
box.get_right
box.set_rightbottom

Sets the right and bottom coordinates of the box.
box.set_rightbottom(id, right, bottom) → void
ARGUMENTS
id (series box) A box object.
right (series int) Bar index or bar time of the right border.
bottom (series int/float) Price value of the bottom border.
SEE ALSO
box.new
box.get_right
box.get_bottom
box.set_text

The function sets the text in the box.
box.set_text(id, text) → void
ARGUMENTS
id (series box) A box object.
text (series string) The text to be displayed inside the box.
SEE ALSO
box.set_text_color
box.set_text_size
box.set_text_valign
box.set_text_halign
box.set_text_color

The function sets the color of the text inside the box.
box.set_text_color(id, text_color) → void
ARGUMENTS
id (series box) A box object.
text_color (series color) The color of the text.
SEE ALSO
box.set_text
box.set_text_size
box.set_text_valign
box.set_text_halign
box.set_text_font_family

The function sets the font family of the text inside the box.
box.set_text_font_family(id, text_font_family) → void
EXAMPLE

//@version=5
indicator("Example of setting the box font")
if barstate.islastconfirmedhistory
    b = box.new(bar_index, open-ta.tr, bar_index-50, open-ta.tr*5, text="monospace")
    box.set_text_font_family(b, font.family_monospace)
ARGUMENTS
id (series box) A box object.
text_font_family (series string) The font family of the text. Possible values: font.family_default, font.family_monospace.
SEE ALSO
box.new
font.family_default
font.family_monospace
box.set_text_halign

The function sets the horizontal alignment of the box's text.
box.set_text_halign(id, text_halign) → void
ARGUMENTS
id (series box) A box object.
text_halign (series string) The horizontal alignment of a box's text. Possible values: text.align_left, text.align_center, text.align_right.
SEE ALSO
box.set_text
box.set_text_size
box.set_text_valign
box.set_text_color
box.set_text_size

The function sets the size of the box's text.
box.set_text_size(id, text_size) → void
ARGUMENTS
id (series box) A box object.
text_size (series string) The size of the text. Possible values: size.auto, size.tiny, size.small, size.normal, size.large, size.huge.
SEE ALSO
box.set_text
box.set_text_color
box.set_text_valign
box.set_text_halign
box.set_text_valign

The function sets the vertical alignment of a box's text.
box.set_text_valign(id, text_valign) → void
ARGUMENTS
id (series box) A box object.
text_valign (series string) The vertical alignment of the box's text. Possible values: text.align_top, text.align_center, text.align_bottom.
SEE ALSO
box.set_text
box.set_text_size
box.set_text_color
box.set_text_halign
box.set_text_wrap

The function sets the mode of wrapping of the text inside the box.
box.set_text_wrap(id, text_wrap) → void
ARGUMENTS
id (series box) A box object.
text_wrap (series string) The mode of the wrapping. Possible values: text.wrap_auto, text.wrap_none.
SEE ALSO
box.set_text
box.set_text_size
box.set_text_valign
box.set_text_halign
box.set_text_color
box.set_top

Sets the top coordinate of the box.
box.set_top(id, top) → void
ARGUMENTS
id (series box) A box object.
top (series int/float) Price value of the top border.
SEE ALSO
box.new
box.get_top
color

Casts na to color
color(x) → const color
color(x) → input color
color(x) → simple color
color(x) → series color
RETURNS
The value of the argument after casting to color.
SEE ALSO
float
int
bool
string
line
label
color.b

Retrieves the value of the color's blue component.
color.b(color) → series float
color.b(color) → const float
color.b(color) → input float
EXAMPLE

//@version=5
indicator("color.b", overlay=true)
plot(color.b(color.blue))
RETURNS
The value (0 to 255) of the color's blue component.
ARGUMENTS
color (series color) Color.
color.from_gradient

Based on the relative position of value in the bottom_value to top_value range, the function returns a color from the gradient defined by bottom_color to top_color.
color.from_gradient(value, bottom_value, top_value, bottom_color, top_color) → series color
EXAMPLE

//@version=5
indicator("color.from_gradient", overlay=true)
color1 = color.from_gradient(close, low, high, color.yellow, color.lime)
color2 = color.from_gradient(ta.rsi(close, 7), 0, 100, color.rgb(255, 0, 0), color.rgb(0, 255, 0, 50))
plot(close, color=color1)
plot(ta.rsi(close,7), color=color2)
RETURNS
A color calculated from the linear gradient between bottom_color to top_color.
ARGUMENTS
value (series int/float) Value to calculate the position-dependent color.
bottom_value (series int/float) Bottom position value corresponding to bottom_color.
top_value (series int/float) Top position value corresponding to top_color.
bottom_color (series color) Bottom position color.
top_color (series color) Top position color.
REMARKS
Using this function will have an impact on the colors displayed in the script's "Settings/Style" tab. See the User Manual for more information.
color.g

Retrieves the value of the color's green component.
color.g(color) → series float
color.g(color) → const float
color.g(color) → input float
EXAMPLE

//@version=5
indicator("color.g", overlay=true)
plot(color.g(color.green))
RETURNS
The value (0 to 255) of the color's green component.
ARGUMENTS
color (series color) Color.
color.new

Function color applies the specified transparency to the given color.
color.new(color, transp) → const color
color.new(color, transp) → series color
color.new(color, transp) → input color
EXAMPLE

//@version=5
indicator("color.new", overlay=true)
plot(close, color=color.new(color.red, 50))
RETURNS
Color with specified transparency.
ARGUMENTS
color (series color) Color to apply transparency to.
transp (series int/float) Possible values are from 0 (not transparent) to 100 (invisible).
REMARKS
Using arguments that are not constants (e.g., 'simple', 'input' or 'series') will have an impact on the colors displayed in the script's "Settings/Style" tab. See the User Manual for more information.
color.r

Retrieves the value of the color's red component.
color.r(color) → series float
color.r(color) → const float
color.r(color) → input float
EXAMPLE

//@version=5
indicator("color.r", overlay=true)
plot(color.r(color.red))
RETURNS
The value (0 to 255) of the color's red component.
ARGUMENTS
color (series color) Color.
color.rgb

Creates a new color with transparency using the RGB color model.
color.rgb(red, green, blue, transp) → series color
color.rgb(red, green, blue, transp) → const color
color.rgb(red, green, blue, transp) → input color
EXAMPLE

//@version=5
indicator("color.rgb", overlay=true)
plot(close, color=color.rgb(255, 0, 0, 50))
RETURNS
Color with specified transparency.
ARGUMENTS
red (series int/float) Red color component. Possible values are from 0 to 255.
green (series int/float) Green color component. Possible values are from 0 to 255.
blue (series int/float) Blue color component. Possible values are from 0 to 255.
transp (series int/float) Optional. Color transparency. Possible values are from 0 (opaque) to 100 (invisible). Default value is 0.
REMARKS
Using arguments that are not constants (e.g., 'simple', 'input' or 'series') will have an impact on the colors displayed in the script's "Settings/Style" tab. See the User Manual for more information.
color.t

Retrieves the color's transparency.
color.t(color) → series float
color.t(color) → const float
color.t(color) → input float
EXAMPLE

//@version=5
indicator("color.t", overlay=true)
plot(color.t(color.new(color.red, 50)))
RETURNS
The value (0-100) of the color's transparency.
ARGUMENTS
color (series color) Color.
dayofmonth

dayofmonth(time) → series int
dayofmonth(time, timezone) → series int
RETURNS
Day of month (in exchange timezone) for provided UNIX time.
ARGUMENTS
time (series int) UNIX time in milliseconds.
timezone (series string) Allows adjusting the returned value to a time zone specified in either UTC/GMT notation (e.g., "UTC-5", "GMT+0530") or as an IANA time zone database name (e.g., "America/New_York"). Optional. The default is syminfo.timezone.
REMARKS
UNIX time is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
Note that this function returns the day based on the time of the bar's open. For overnight sessions (e.g. EURUSD, where Monday session starts on Sunday, 17:00 UTC-4) this value can be lower by 1 than the day of the trading day.
SEE ALSO
dayofmonth
time
year
month
dayofweek
hour
minute
second
dayofweek

dayofweek(time) → series int
dayofweek(time, timezone) → series int
RETURNS
Day of week (in exchange timezone) for provided UNIX time.
ARGUMENTS
time (series int) UNIX time in milliseconds.
timezone (series string) Allows adjusting the returned value to a time zone specified in either UTC/GMT notation (e.g., "UTC-5", "GMT+0530") or as an IANA time zone database name (e.g., "America/New_York"). Optional. The default is syminfo.timezone.
REMARKS
Note that this function returns the day based on the time of the bar's open. For overnight sessions (e.g. EURUSD, where Monday session starts on Sunday, 17:00) this value can be lower by 1 than the day of the trading day.
UNIX time is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
SEE ALSO
dayofweek
time
year
month
dayofmonth
hour
minute
second
fill

Fills background between two plots or hlines with a given color.
fill(hline1, hline2, color, title, editable, fillgaps, display) → void
fill(plot1, plot2, color, title, editable, show_last, fillgaps, display) → void
fill(plot1, plot2, top_value, bottom_value, top_color, bottom_color, title, display, fillgaps, editable) → void
fill(hline1, hline2, top_value, bottom_value, top_color, bottom_color, title, display, fillgaps, editable) → void
Fill between two horizontal lines
EXAMPLE

//@version=5
indicator("Fill between hlines", overlay = false)
h1 = hline(20)
h2 = hline(10)
fill(h1, h2, color = color.new(color.blue, 90))
Fill between two plots
EXAMPLE

//@version=5
indicator("Fill between plots", overlay = true)
p1 = plot(open)
p2 = plot(close)
fill(p1, p2, color = color.new(color.green, 90))
Gradient fill between two horizontal lines
EXAMPLE

//@version=5
indicator("Gradient Fill between hlines", overlay = false)
topVal = input.int(100)
botVal = input.int(0)
topCol = input.color(color.red)
botCol = input.color(color.blue)
topLine = hline(100, color = topCol, linestyle = hline.style_solid)
botLine = hline(0,   color = botCol, linestyle = hline.style_solid)
fill(topLine, botLine, topVal, botVal, topCol, botCol)
ARGUMENTS
hline1 (hline) The first hline object. Required argument.
hline2 (hline) The second hline object. Required argument.
plot1 (plot) The first plot object. Required argument.
plot2 (plot) The second plot object. Required argument.
color (series color) Color of the background fill. You can use constants like 'color=color.red' or 'color=#ff001a' as well as complex expressions like 'color = close >= open ? color.green : color.red'. Optional argument.
title (const string) Title of the created fill object. Optional argument.
editable (const bool) If true then fill style will be editable in Format dialog. Default is true.
show_last (input int) If set, defines the number of bars (from the last bar back to the past) to fill on chart.
fillgaps (const bool) Controls continuing fills on gaps, i.e., when one of the plot() calls returns an na value. When true, the last fill will continue on gaps. The default is false.
display (input plot_simple_display) Controls where the fill is displayed. Possible values are: display.none, display.all. Default is display.all.
top_value (series int/float) Value where the gradient uses the `top_color`.
bottom_value (series int/float) Value where the gradient uses the `bottom_color`.
top_color (series color) Color of the gradient at the topmost value.
bottom_color (series color) Color of the gradient at the bottommost value.
SEE ALSO
plot
barcolor
bgcolor
hline
color.new
fixnan

For a given series replaces NaN values with previous nearest non-NaN value.
fixnan(source) → series float
fixnan(source) → series int
fixnan(source) → series bool
fixnan(source) → series color
RETURNS
Series without na gaps.
ARGUMENTS
source (series int/float/bool/color) Source used for the calculation.
SEE ALSO
na
na
nz
float

Casts na to float
float(x) → const float
float(x) → input float
float(x) → simple float
float(x) → series float
RETURNS
The value of the argument after casting to float.
SEE ALSO
int
bool
color
string
line
label
hline

Renders a horizontal line at a given fixed price level.
hline(price, title, color, linestyle, linewidth, editable, display) → hline
EXAMPLE

//@version=5
indicator("input.hline", overlay=true)
hline(3.14, title='Pi', color=color.blue, linestyle=hline.style_dotted, linewidth=2)

// You may fill the background between any two hlines with a fill() function:
h1 = hline(20)
h2 = hline(10)
fill(h1, h2, color=color.new(color.green, 90))
RETURNS
An hline object, that can be used in fill
ARGUMENTS
price (input int/float) Price value at which the object will be rendered. Required argument.
title (const string) Title of the object.
color (input color) Color of the rendered line. Must be a constant value (not an expression). Optional argument.
linestyle (input hline_style) Style of the rendered line. Possible values are: hline.style_solid, hline.style_dotted, hline.style_dashed. Optional argument.
linewidth (input int) Width of the rendered line. Default value is 1.
editable (const bool) If true then hline style will be editable in Format dialog. Default is true.
display (input plot_simple_display) Controls where the hline is displayed. Possible values are: display.none, display.all. Default is display.all.
SEE ALSO
fill
hour

hour(time) → series int
hour(time, timezone) → series int
RETURNS
Hour (in exchange timezone) for provided UNIX time.
ARGUMENTS
time (series int) UNIX time in milliseconds.
timezone (series string) Allows adjusting the returned value to a time zone specified in either UTC/GMT notation (e.g., "UTC-5", "GMT+0530") or as an IANA time zone database name (e.g., "America/New_York"). Optional. The default is syminfo.timezone.
REMARKS
UNIX time is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
SEE ALSO
hour
time
year
month
dayofmonth
dayofweek
minute
second
indicator

This declaration statement designates the script as an indicator and sets a number of indicator-related properties.
indicator(title, shorttitle, overlay, format, precision, scale, max_bars_back, timeframe, timeframe_gaps, explicit_plot_zorder, max_lines_count, max_labels_count, max_boxes_count) → void
EXAMPLE

//@version=5
indicator("My script", shorttitle="Script")
plot(close)
ARGUMENTS
title (const string) The title of the script. It is displayed on the chart when no `shorttitle` argument is used, and becomes the publication's default title when publishing the script.
shorttitle (const string) The script's display name on charts. If specified, it will replace the `title` argument in most chart-related windows. Optional. The default is the argument used for `title`.
overlay (const bool) If true, the indicator will be displayed over the chart. If false, it will be added in a separate pane. Optional. The default is false.
format (const string) Specifies the formatting of the script's displayed values. Possible values: format.inherit, format.price, format.volume, format.percent. Optional. The default is format.inherit.
precision (const int) Specifies the number of digits after the floating point of the script's displayed values. Must be a non-negative integer no greater than 16. If `format` is set to format.inherit and `precision` is specified, the format will instead be set to format.price. Optional. The default is inherited from the precision of the chart's symbol.
scale (const scale_type) The price scale used. Possible values: scale.right, scale.left, scale.none. The scale.none value can only be applied in combination with `overlay = true`. Optional. By default, the script uses the same scale as the chart.
max_bars_back (const int) The length of the historical buffer the script keeps for every variable and function, which determines how many past values can be referenced using the `[]` history-referencing operator. The required buffer size is automatically detected by the Pine Script™ runtime. Using this parameter is only necessary when a runtime error occurs because automatic detection fails. More information on the underlying mechanics of the historical buffer can be found in our Help Center. Optional. The default is 0.
timeframe (const string) Adds multi-timeframe functionality to simple scripts. When used, a "Timeframe" field will be added to the script's "Settings/Inputs" tab. The field's default value will be the argument supplied, whose format must conform to timeframe string specifications. To specify the chart's timeframe, use an empty string or the timeframe.period variable. The parameter cannot be used with scripts using Pine Script™ drawings. Optional. The default is timeframe.period.
timeframe_gaps (const bool) Specifies how the indicator's values are displayed on chart bars when the `timeframe` is higher than the chart's. If true, a value only appears on a chart bar when the higher `timeframe` value becomes available, otherwise na is returned (thus a "gap" occurs). With false, what would otherwise be gaps are filled with the latest known value returned, avoiding na values. When used, a "Gaps" checkbox will appear in the indicator's "Settings/Inputs" tab. Optional. The default is true.
explicit_plot_zorder (const bool) Specifies the order in which the script's plots, fills, and hlines are rendered. If true, plots are drawn in the order in which they appear in the script's code, each newer plot being drawn above the previous ones. This only applies to `plot*()` functions, fill, and hline. Optional. The default is false.
max_lines_count (const int) The number of last line drawings displayed. Possible values: 1-500. The count is approximate; more drawings than the specified count may be displayed. Optional. The default is 50.
max_labels_count (const int) The number of last label drawings displayed. Possible values: 1-500. The count is approximate; more drawings than the specified count may be displayed. Optional. The default is 50.
max_boxes_count (const int) The number of last box drawings displayed. Possible values: 1-500. The count is approximate; more drawings than the specified count may be displayed. Optional. The default is 50.
REMARKS
Every indicator script must have one indicator call.
SEE ALSO
strategy
library
input

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function automatically detects the type of the argument used for 'defval' and uses the corresponding input widget.
input(defval, title, tooltip, inline, group, display) → input bool
input(defval, title, tooltip, inline, group, display) → input color
input(defval, title, tooltip, inline, group, display) → input int
input(defval, title, tooltip, inline, group, display) → input float
input(defval, title, tooltip, inline, group, display) → input string
input(defval, title, inline, group, tooltip, display) → series float
EXAMPLE

//@version=5
indicator("input", overlay=true)
i_switch = input(true, "On/Off")
plot(i_switch ? open : na)

i_len = input(7, "Length")
i_src = input(close, "Source")
plot(ta.sma(i_src, i_len))

i_border = input(142.50, "Price Border")
hline(i_border)
bgcolor(close > i_border ? color.green : color.red)

i_col = input(color.red, "Plot Color")
plot(close, color=i_col)

i_text = input("Hello!", "Message")
l = label.new(bar_index, high, text=i_text)
label.delete(l[1])
RETURNS
Value of input variable.
ARGUMENTS
defval (const int/float/bool/string/color or source-type built-ins) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where script users can change it. Source-type built-ins are built-in series float variables that specify the source of the calculation: `close`, `hlc3`, etc.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
display (input plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default depends on the type of the value passed to `defval`: display.none for bool and color values, display.all for everything else.
REMARKS
Result of input function always should be assigned to a variable, see examples above.
SEE ALSO
input.bool
input.color
input.int
input.float
input.string
input.symbol
input.timeframe
input.text_area
input.session
input.source
input.time
input.bool

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a checkmark to the script's inputs.
input.bool(defval, title, tooltip, inline, group, confirm, display) → input bool
EXAMPLE

//@version=5
indicator("input.bool", overlay=true)
i_switch = input.bool(true, "On/Off")
plot(i_switch ? open : na)
RETURNS
Value of input variable.
ARGUMENTS
defval (const bool) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (input plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.none.
REMARKS
Result of input.bool function always should be assigned to a variable, see examples above.
SEE ALSO
input.int
input.float
input.string
input.text_area
input.symbol
input.timeframe
input.session
input.source
input.color
input.time
input
input.color

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a color picker that allows the user to select a color and transparency, either from a palette or a hex value.
input.color(defval, title, tooltip, inline, group, confirm, display) → input color
EXAMPLE

//@version=5
indicator("input.color", overlay=true)
i_col = input.color(color.red, "Plot Color")
plot(close, color=i_col)
RETURNS
Value of input variable.
ARGUMENTS
defval (const color) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (input plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.none.
REMARKS
Result of input.color function always should be assigned to a variable, see examples above.
SEE ALSO
input.bool
input.int
input.float
input.string
input.text_area
input.symbol
input.timeframe
input.session
input.source
input.time
input
input.float

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a field for a float input to the script's inputs.
input.float(defval, title, minval, maxval, step, tooltip, inline, group, confirm, display) → input float
input.float(defval, title, options, tooltip, inline, group, confirm, display) → input float
EXAMPLE

//@version=5
indicator("input.float", overlay=true)
i_angle1 = input.float(0.5, "Sin Angle", minval=-3.14, maxval=3.14, step=0.02)
plot(math.sin(i_angle1) > 0 ? close : open, "sin", color=color.green)

i_angle2 = input.float(0, "Cos Angle", options=[-3.14, -1.57, 0, 1.57, 3.14])
plot(math.cos(i_angle2) > 0 ? close : open, "cos", color=color.red)
RETURNS
Value of input variable.
ARGUMENTS
defval (const int/float) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where script users can change it. When a list of values is used with the `options` parameter, the value must be one of them.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
minval (const int/float) Minimal possible value of the input variable. Optional.
maxval (const int/float) Maximum possible value of the input variable. Optional.
step (const int/float) Step value used for incrementing/decrementing the input. Optional. The default is 1.
options (tuple of const int/float values: [val1, val2, ...]) A list of options to choose from a dropdown menu, separated by commas and enclosed in square brackets: [val1, val2, ...]. When using this parameter, the `minval`, `maxval` and `step` parameters cannot be used.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (input plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.all.
REMARKS
Result of input.float function always should be assigned to a variable, see examples above.
SEE ALSO
input.bool
input.int
input.string
input.text_area
input.symbol
input.timeframe
input.session
input.source
input.color
input.time
input
input.int

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a field for an integer input to the script's inputs.
input.int(defval, title, minval, maxval, step, tooltip, inline, group, confirm, display) → input int
input.int(defval, title, options, tooltip, inline, group, confirm, display) → input int
EXAMPLE

//@version=5
indicator("input.int", overlay=true)
i_len1 = input.int(10, "Length 1", minval=5, maxval=21, step=1)
plot(ta.sma(close, i_len1))

i_len2 = input.int(10, "Length 2", options=[5, 10, 21])
plot(ta.sma(close, i_len2))
RETURNS
Value of input variable.
ARGUMENTS
defval (const int) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where script users can change it. When a list of values is used with the `options` parameter, the value must be one of them.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
minval (const int) Minimal possible value of the input variable. Optional.
maxval (const int) Maximum possible value of the input variable. Optional.
step (const int) Step value used for incrementing/decrementing the input. Optional. The default is 1.
options (tuple of const int values: [val1, val2, ...]) A list of options to choose from a dropdown menu, separated by commas and enclosed in square brackets: [val1, val2, ...]. When using this parameter, the `minval`, `maxval` and `step` parameters cannot be used.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (input plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.all.
REMARKS
Result of input.int function always should be assigned to a variable, see examples above.
SEE ALSO
input.bool
input.float
input.string
input.text_area
input.symbol
input.timeframe
input.session
input.source
input.color
input.time
input
input.price

Adds a price input to the script's "Settings/Inputs" tab. Using `confirm = true` activates the interactive input mode where a price is selected by clicking on the chart.
input.price(defval, title, tooltip, inline, group, confirm, display) → input float
EXAMPLE

//@version=5
indicator("input.price", overlay=true)
price1 = input.price(title="Date", defval=42)
plot(price1)

price2 = input.price(54, title="Date")
plot(price2)
RETURNS
Value of input variable.
ARGUMENTS
defval (const int/float) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, the interactive input mode is enabled and the selection is done by clicking on the chart when the indicator is added to the chart, or by selecting the indicator and moving the selection after that. Optional. The default is false.
display (input plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.all.
REMARKS
When using interactive mode, a time input can be combined with a price input if both function calls use the same argument for their `inline` parameter.
SEE ALSO
input.bool
input.int
input.float
input.string
input.text_area
input.symbol
input.resolution
input.session
input.source
input.color
input
input.session

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds two dropdowns that allow the user to specify the beginning and the end of a session using the session selector and returns the result as a string.
input.session(defval, title, options, tooltip, inline, group, confirm, display) → input string
EXAMPLE

//@version=5
indicator("input.session", overlay=true)
i_sess = input.session("1300-1700", "Session", options=["0930-1600", "1300-1700", "1700-2100"])
t = time(timeframe.period, i_sess)
bgcolor(time == t ? color.green : na)
RETURNS
Value of input variable.
ARGUMENTS
defval (const string) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it. When a list of values is used with the `options` parameter, the value must be one of them.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
options (tuple of const string values: [val1, val2, ...]) A list of options to choose from.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (input plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.all.
REMARKS
Result of input.session function always should be assigned to a variable, see examples above.
SEE ALSO
input.bool
input.int
input.float
input.string
input.text_area
input.symbol
input.timeframe
input.source
input.color
input.time
input
input.source

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a dropdown that allows the user to select a source for the calculation, e.g. close, hl2, etc. The user can also select an output from another indicator on their chart as the source.
input.source(defval, title, tooltip, inline, group, display) → series float
EXAMPLE

//@version=5
indicator("input.source", overlay=true)
i_src = input.source(close, "Source")
plot(i_src)
RETURNS
Value of input variable.
ARGUMENTS
defval (open/high/low/close/hl2/hlc3/ohlc4/hlcc4) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
display (input plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.all.
REMARKS
Result of input.source function always should be assigned to a variable, see examples above.
SEE ALSO
input.bool
input.int
input.float
input.string
input.text_area
input.symbol
input.timeframe
input.session
input.color
input.time
input
input.string

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a field for a string input to the script's inputs.
input.string(defval, title, options, tooltip, inline, group, confirm, display) → input string
EXAMPLE

//@version=5
indicator("input.string", overlay=true)
i_text = input.string("Hello!", "Message")
l = label.new(bar_index, high, i_text)
label.delete(l[1])
RETURNS
Value of input variable.
ARGUMENTS
defval (const string) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it. When a list of values is used with the `options` parameter, the value must be one of them.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
options (tuple of const string values: [val1, val2, ...]) A list of options to choose from.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (input plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.all.
REMARKS
Result of input.string function always should be assigned to a variable, see examples above.
SEE ALSO
input.text_area
input.bool
input.int
input.float
input.symbol
input.timeframe
input.session
input.source
input.color
input.time
input
input.symbol

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a field that allows the user to select a specific symbol using the symbol search and returns that symbol, paired with its exchange prefix, as a string.
input.symbol(defval, title, tooltip, inline, group, confirm, display) → input string
EXAMPLE

//@version=5
indicator("input.symbol", overlay=true)
i_sym = input.symbol("DELL", "Symbol")
s = request.security(i_sym, 'D', close)
plot(s)
RETURNS
Value of input variable.
ARGUMENTS
defval (const string) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (input plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.all.
REMARKS
Result of input.symbol function always should be assigned to a variable, see examples above.
SEE ALSO
input.bool
input.int
input.float
input.string
input.text_area
input.timeframe
input.session
input.source
input.color
input.time
input
input.text_area

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a field for a multiline text input.
input.text_area(defval, title, tooltip, group, confirm, display) → input string
EXAMPLE

//@version=5
indicator("input.text_area")
i_text = input.text_area(defval = "Hello \nWorld!", title = "Message")
plot(close)
RETURNS
Value of input variable.
ARGUMENTS
defval (const string) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (input plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.none.
REMARKS
Result of input.text_area function always should be assigned to a variable, see examples above.
SEE ALSO
input.string
input.bool
input.int
input.float
input.symbol
input.timeframe
input.session
input.source
input.color
input.time
input
input.time

Adds a time input to the script's "Settings/Inputs" tab. This function adds two input widgets on the same line: one for the date and one for the time. The function returns a date/time value in UNIX format. Using `confirm = true` activates the interactive input mode where a point in time is selected by clicking on the chart.
input.time(defval, title, tooltip, inline, group, confirm, display) → input int
EXAMPLE

//@version=5
indicator("input.time", overlay=true)
i_date = input.time(timestamp("20 Jul 2021 00:00 +0300"), "Date")
l = label.new(i_date, high, "Date", xloc=xloc.bar_time)
label.delete(l[1])
RETURNS
Value of input variable.
ARGUMENTS
defval (const int) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it. The value can be a timestamp function, but only if it uses a date argument in const string format.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, the interactive input mode is enabled and the selection is done by clicking on the chart when the indicator is added to the chart, or by selecting the indicator and moving the selection after that. Optional. The default is false.
display (input plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.none.
REMARKS
When using interactive mode, a price input can be combined with a time input if both function calls use the same argument for their `inline` parameter.
SEE ALSO
input.bool
input.int
input.float
input.string
input.text_area
input.symbol
input.timeframe
input.session
input.source
input.color
input
input.timeframe

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a dropdown that allows the user to select a specific timeframe via the timeframe selector and returns it as a string. The selector includes the custom timeframes a user may have added using the chart's Timeframe dropdown.
input.timeframe(defval, title, options, tooltip, inline, group, confirm, display) → input string
EXAMPLE

//@version=5
indicator("input.timeframe", overlay=true)
i_res = input.timeframe('D', "Resolution", options=['D', 'W', 'M'])
s = request.security("AAPL", i_res, close)
plot(s)
RETURNS
Value of input variable.
ARGUMENTS
defval (const string) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it. When a list of values is used with the `options` parameter, the value must be one of them.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
options (tuple of const string values: [val1, val2, ...]) A list of options to choose from.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (input plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.all.
REMARKS
Result of input.timeframe function always should be assigned to a variable, see examples above.
SEE ALSO
input.bool
input.int
input.float
input.string
input.text_area
input.symbol
input.session
input.source
input.color
input.time
input
int

Casts na or truncates float value to int
int(x) → simple int
int(x) → input int
int(x) → const int
int(x) → series int
RETURNS
The value of the argument after casting to int.
SEE ALSO
float
bool
color
string
line
label
label

Casts na to label
label(x) → series label
RETURNS
The value of the argument after casting to label.
SEE ALSO
float
int
bool
color
string
line
label.copy

Clones the label object.
label.copy(id) → series label
EXAMPLE

//@version=5
indicator('Last 100 bars highest/lowest', overlay = true)
LOOKBACK = 100
highest = ta.highest(LOOKBACK)
highestBars = ta.highestbars(LOOKBACK)
lowest = ta.lowest(LOOKBACK)
lowestBars = ta.lowestbars(LOOKBACK)
if barstate.islastconfirmedhistory
    var labelHigh = label.new(bar_index + highestBars, highest, str.tostring(highest), color = color.green)
    var labelLow = label.copy(labelHigh)
    label.set_xy(labelLow, bar_index + lowestBars, lowest)
    label.set_text(labelLow, str.tostring(lowest))
    label.set_color(labelLow, color.red)
    label.set_style(labelLow, label.style_label_up)
RETURNS
New label ID object which may be passed to label.setXXX and label.getXXX functions.
ARGUMENTS
id (series label) Label object.
SEE ALSO
label.new
label.delete
label.delete

Deletes the specified label object. If it has already been deleted, does nothing.
label.delete(id) → void
ARGUMENTS
id (series label) Label object to delete.
SEE ALSO
label.new
label.get_text

Returns the text of this label object.
label.get_text(id) → series string
EXAMPLE

//@version=5
indicator("label.get_text")
my_label = label.new(time, open, text="Open bar text", xloc=xloc.bar_time)
a = label.get_text(my_label)
label.new(time, close, text = a + " new", xloc=xloc.bar_time)
RETURNS
String object containing the text of this label.
ARGUMENTS
id (series label) Label object.
SEE ALSO
label.new
label.get_x

Returns UNIX time or bar index (depending on the last xloc value set) of this label's position.
label.get_x(id) → series int
EXAMPLE

//@version=5
indicator("label.get_x")
my_label = label.new(time, open, text="Open bar text", xloc=xloc.bar_time)
a = label.get_x(my_label)
plot(time - label.get_x(my_label)) //draws zero plot
RETURNS
UNIX timestamp (in milliseconds) or bar index.
ARGUMENTS
id (series label) Label object.
SEE ALSO
label.new
label.get_y

Returns price of this label's position.
label.get_y(id) → series float
RETURNS
Floating point value representing price.
ARGUMENTS
id (series label) Label object.
SEE ALSO
label.new
label.new

Creates new label object.
label.new(x, y, text, xloc, yloc, color, style, textcolor, size, textalign, tooltip, text_font_family) → series label
EXAMPLE

//@version=5
indicator("label.new")
var label1 = label.new(bar_index, low, text="Hello, world!", style=label.style_circle)
label.set_x(label1, 0)
label.set_xloc(label1, time, xloc.bar_time)
label.set_color(label1, color.red)
label.set_size(label1, size.large)
RETURNS
Label ID object which may be passed to label.setXXX and label.getXXX functions.
ARGUMENTS
x (series int) Bar index (if xloc = xloc.bar_index) or bar UNIX time (if xloc = xloc.bar_time) of the label position. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
y (series int/float) Price of the label position. It is taken into account only if yloc=yloc.price.
text (series string) Label text. Default is empty string.
text_font_family (series string) The font family of the text. Optional. The default value is font.family_default. Possible values: font.family_default, font.family_monospace.
xloc (series string) See description of x argument. Possible values: xloc.bar_index and xloc.bar_time. Default is xloc.bar_index.
yloc (series string) Possible values are yloc.price, yloc.abovebar, yloc.belowbar. If yloc=yloc.price, y argument specifies the price of the label position. If yloc=yloc.abovebar, label is located above bar. If yloc=yloc.belowbar, label is located below bar. Default is yloc.price.
color (series color) Color of the label border and arrow
style (series string) Label style. Possible values: label.style_none, label.style_xcross, label.style_cross, label.style_triangleup, label.style_triangledown, label.style_flag, label.style_circle, label.style_arrowup, label.style_arrowdown, label.style_label_up, label.style_label_down, label.style_label_left, label.style_label_right, label.style_label_lower_left, label.style_label_lower_right, label.style_label_upper_left, label.style_label_upper_right, label.style_label_center, label.style_square, label.style_diamond, label.style_text_outline. Default is label.style_label_down.
textcolor (series color) Text color.
size (series string) Label size. Possible values: size.auto, size.tiny, size.small, size.normal, size.large, size.huge. Default value is size.normal.
textalign (series string) Label text alignment. Possible values: text.align_left, text.align_center, text.align_right. Default value is text.align_center.
tooltip (series string) Hover to see tooltip label.
SEE ALSO
label.delete
label.set_x
label.set_y
label.set_xy
label.set_xloc
label.set_yloc
label.set_color
label.set_textcolor
label.set_style
label.set_size
label.set_textalign
label.set_tooltip
label.set_color

Sets label border and arrow color.
label.set_color(id, color) → void
ARGUMENTS
id (series label) Label object.
color (series color) New label border and arrow color.
SEE ALSO
label.new
label.set_size

Sets arrow and text size of the specified label object.
label.set_size(id, size) → void
ARGUMENTS
id (series label) Label object.
size (series string) Possible values: size.auto, size.tiny, size.small, size.normal, size.large, size.huge. Default value is size.auto.
SEE ALSO
size.auto
size.tiny
size.small
size.normal
size.large
size.huge
label.new
label.set_style

Sets label style.
label.set_style(id, style) → void
ARGUMENTS
id (series label) Label object.
style (series string) New label style. Possible values: label.style_none, label.style_xcross, label.style_cross, label.style_triangleup, label.style_triangledown, label.style_flag, label.style_circle, label.style_arrowup, label.style_arrowdown, label.style_label_up, label.style_label_down, label.style_label_left, label.style_label_right, label.style_label_lower_left, label.style_label_lower_right, label.style_label_upper_left, label.style_label_upper_right, label.style_label_center, label.style_square, label.style_diamond, label.style_text_outline.
SEE ALSO
label.new
label.set_text

Sets label text
label.set_text(id, text) → void
ARGUMENTS
id (series label) Label object.
text (series string) New label text.
SEE ALSO
label.new
label.set_text_font_family

The function sets the font family of the text inside the label.
label.set_text_font_family(id, text_font_family) → void
EXAMPLE

//@version=5
indicator("Example of setting the label font")
if barstate.islastconfirmedhistory
    l = label.new(bar_index, 0, "monospace", yloc=yloc.abovebar)
    label.set_text_font_family(l, font.family_monospace)
ARGUMENTS
id (series label) A label object.
text_font_family (series string) The font family of the text. Possible values: font.family_default, font.family_monospace.
SEE ALSO
label.new
font.family_default
font.family_monospace
label.set_textalign

Sets the alignment for the label text.
label.set_textalign(id, textalign) → void
ARGUMENTS
id (series label) Label object.
textalign (series string) Label text alignment. Possible values: text.align_left, text.align_center, text.align_right.
SEE ALSO
text.align_left
text.align_center
text.align_right
label.new
label.set_textcolor

Sets color of the label text.
label.set_textcolor(id, textcolor) → void
ARGUMENTS
id (series label) Label object.
textcolor (series color) New text color.
SEE ALSO
label.new
label.set_tooltip

Sets the tooltip text.
label.set_tooltip(id, tooltip) → void
ARGUMENTS
id (series label) Label object.
tooltip (series string) Tooltip text.
SEE ALSO
label.new
label.set_x

Sets bar index or bar time (depending on the xloc) of the label position.
label.set_x(id, x) → void
ARGUMENTS
id (series label) Label object.
x (series int) New bar index or bar time of the label position. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
SEE ALSO
label.new
label.set_xloc

Sets x-location and new bar index/time value.
label.set_xloc(id, x, xloc) → void
ARGUMENTS
id (series label) Label object.
x (series int) New bar index or bar time of the label position.
xloc (series string) New x-location value.
SEE ALSO
xloc.bar_index
xloc.bar_time
label.new
label.set_xy

Sets bar index/time and price of the label position.
label.set_xy(id, x, y) → void
ARGUMENTS
id (series label) Label object.
x (series int) New bar index or bar time of the label position. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
y (series int/float) New price of the label position.
SEE ALSO
label.new
label.set_y

Sets price of the label position
label.set_y(id, y) → void
ARGUMENTS
id (series label) Label object.
y (series int/float) New price of the label position.
SEE ALSO
label.new
label.set_yloc

Sets new y-location calculation algorithm.
label.set_yloc(id, yloc) → void
ARGUMENTS
id (series label) Label object.
yloc (series string) New y-location value.
SEE ALSO
yloc.price
yloc.abovebar
yloc.belowbar
label.new
library

Declaration statement identifying a script as a library.
library(title, overlay) → void
EXAMPLE

//@version=5
// @description Math library
library("num_methods", overlay = true)
// Calculate "sinh()" from the float parameter `x`
export sinh(float x) =>
    (math.exp(x) - math.exp(-x)) / 2.0
plot(sinh(0))
ARGUMENTS
title (const string) The title of the library and its identifier. It cannot contain spaces, special characters or begin with a digit. It is used as the publication's default title, and to uniquely identify the library in the import statement, when another script uses it. It is also used as the script's name on the chart.
overlay (const bool) If true, the library will be added over the chart. If false, it will be added in a separate pane. Optional. The default is false.
SEE ALSO
indicator
strategy
line

Casts na to line
line(x) → series line
RETURNS
The value of the argument after casting to line.
SEE ALSO
float
int
bool
color
string
label
line.copy

Clones the line object.
line.copy(id) → series line
EXAMPLE

//@version=5
indicator('Last 100 bars price range', overlay = true)
LOOKBACK = 100
highest = ta.highest(LOOKBACK)
lowest = ta.lowest(LOOKBACK)
if barstate.islastconfirmedhistory
    var lineTop = line.new(bar_index[LOOKBACK], highest, bar_index, highest, color = color.green)
    var lineBottom = line.copy(lineTop)
    line.set_y1(lineBottom, lowest)
    line.set_y2(lineBottom, lowest)
    line.set_color(lineBottom, color.red)
RETURNS
New line ID object which may be passed to line.setXXX and line.getXXX functions.
ARGUMENTS
id (series line) Line object.
SEE ALSO
line.new
line.delete
line.delete

Deletes the specified line object. If it has already been deleted, does nothing.
line.delete(id) → void
ARGUMENTS
id (series line) Line object to delete.
SEE ALSO
line.new
line.get_price

Returns the price level of a line at a given bar index.
line.get_price(id, x) → series float
EXAMPLE

//@version=5
indicator("GetPrice", overlay=true)
var line l = na
if bar_index == 10
    l := line.new(0, high[5], bar_index, high)
plot(line.get_price(l, bar_index), color=color.green)
RETURNS
Price value of line 'id' at bar index 'x'.
ARGUMENTS
id (series line) Line object.
x (series int) Bar index for which price is required.
REMARKS
The line is considered to have been created using 'extend=extend.both'.
This function can only be called for lines created using 'xloc.bar_index'. If you try to call it for a line created with 'xloc.bar_time', it will generate an error.
SEE ALSO
line.new
line.get_x1

Returns UNIX time or bar index (depending on the last xloc value set) of the first point of the line.
line.get_x1(id) → series int
EXAMPLE

//@version=5
indicator("line.get_x1")
my_line = line.new(time, open, time + 60 * 60 * 24, close, xloc=xloc.bar_time)
a = line.get_x1(my_line)
plot(time - line.get_x1(my_line)) //draws zero plot
RETURNS
UNIX timestamp (in milliseconds) or bar index.
ARGUMENTS
id (series line) Line object.
SEE ALSO
line.new
line.get_x2

Returns UNIX time or bar index (depending on the last xloc value set) of the second point of the line.
line.get_x2(id) → series int
RETURNS
UNIX timestamp (in milliseconds) or bar index.
ARGUMENTS
id (series line) Line object.
SEE ALSO
line.new
line.get_y1

Returns price of the first point of the line.
line.get_y1(id) → series float
RETURNS
Price value.
ARGUMENTS
id (series line) Line object.
SEE ALSO
line.new
line.get_y2

Returns price of the second point of the line.
line.get_y2(id) → series float
RETURNS
Price value.
ARGUMENTS
id (series line) Line object.
SEE ALSO
line.new
line.new

Creates new line object.
line.new(x1, y1, x2, y2, xloc, extend, color, style, width) → series line
EXAMPLE

//@version=5
indicator("line.new")
var line1 = line.new(0, low, bar_index, high, extend=extend.right)
var line2 = line.new(time, open, time + 60 * 60 * 24, close, xloc=xloc.bar_time, style=line.style_dashed)
line.set_x2(line1, 0)
line.set_xloc(line1, time, time + 60 * 60 * 24, xloc.bar_time)
line.set_color(line2, color.green)
line.set_width(line2, 5)
RETURNS
Line ID object which may be passed to line.setXXX and line.getXXX functions.
ARGUMENTS
x1 (series int) Bar index (if xloc = xloc.bar_index) or bar UNIX time (if xloc = xloc.bar_time) of the first point of the line. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
y1 (series int/float) Price of the first point of the line.
x2 (series int) Bar index (if xloc = xloc.bar_index) or bar UNIX time (if xloc = xloc.bar_time) of the second point of the line. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
y2 (series int/float) Price of the second point of the line.
xloc (series string) See description of x1 argument. Possible values: xloc.bar_index and xloc.bar_time. Default is xloc.bar_index.
extend (series string) If extend=extend.none, draws segment starting at point (x1, y1) and ending at point (x2, y2). If extend is equal to extend.right or extend.left, draws a ray starting at point (x1, y1) or (x2, y2), respectively. If extend=extend.both, draws a straight line that goes through these points. Default value is extend.none.
color (series color) Line color.
style (series string) Line style. Possible values: line.style_solid, line.style_dotted, line.style_dashed, line.style_arrow_left, line.style_arrow_right, line.style_arrow_both.
width (series int) Line width in pixels.
SEE ALSO
line.delete
line.set_x1
line.set_y1
line.set_xy1
line.set_x2
line.set_y2
line.set_xy2
line.set_xloc
line.set_color
line.set_extend
line.set_style
line.set_width
line.set_color

Sets the line color
line.set_color(id, color) → void
ARGUMENTS
id (series line) Line object.
color (series color) New line color
SEE ALSO
line.new
line.set_extend

Sets extending type of this line object. If extend=extend.none, draws segment starting at point (x1, y1) and ending at point (x2, y2). If extend is equal to extend.right or extend.left, draws a ray starting at point (x1, y1) or (x2, y2), respectively. If extend=extend.both, draws a straight line that goes through these points.
line.set_extend(id, extend) → void
ARGUMENTS
id (series line) Line object.
extend (series string) New extending type.
SEE ALSO
extend.none
extend.right
extend.left
extend.both
line.new
line.set_style

Sets the line style
line.set_style(id, style) → void
ARGUMENTS
id (series line) Line object.
style (series string) New line style.
SEE ALSO
line.style_solid
line.style_dotted
line.style_dashed
line.style_arrow_left
line.style_arrow_right
line.style_arrow_both
line.new
line.set_width

Sets the line width.
line.set_width(id, width) → void
ARGUMENTS
id (series line) Line object.
width (series int) New line width in pixels.
SEE ALSO
line.new
line.set_x1

Sets bar index or bar time (depending on the xloc) of the first point.
line.set_x1(id, x) → void
ARGUMENTS
id (series line) Line object.
x (series int) Bar index or bar time. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
SEE ALSO
line.new
line.set_x2

Sets bar index or bar time (depending on the xloc) of the second point.
line.set_x2(id, x) → void
ARGUMENTS
id (series line) Line object.
x (series int) Bar index or bar time. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
SEE ALSO
line.new
line.set_xloc

Sets x-location and new bar index/time values.
line.set_xloc(id, x1, x2, xloc) → void
ARGUMENTS
id (series line) Line object.
x1 (series int) Bar index or bar time of the first point.
x2 (series int) Bar index or bar time of the second point.
xloc (series string) New x-location value.
SEE ALSO
xloc.bar_index
xloc.bar_time
line.new
line.set_xy1

Sets bar index/time and price of the first point.
line.set_xy1(id, x, y) → void
ARGUMENTS
id (series line) Line object.
x (series int) Bar index or bar time. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
y (series int/float) Price.
SEE ALSO
line.new
line.set_xy2

Sets bar index/time and price of the second point
line.set_xy2(id, x, y) → void
ARGUMENTS
id (series line) Line object.
x (series int) Bar index or bar time.
y (series int/float) Price.
SEE ALSO
line.new
line.set_y1

Sets price of the first point
line.set_y1(id, y) → void
ARGUMENTS
id (series line) Line object.
y (series int/float) Price.
SEE ALSO
line.new
line.set_y2

Sets price of the second point.
line.set_y2(id, y) → void
ARGUMENTS
id (series line) Line object.
y (series int/float) Price.
SEE ALSO
line.new
linefill

Casts na to linefill.
linefill(x) → series linefill
RETURNS
The value of the argument after casting to linefill.
SEE ALSO
float
int
bool
color
string
line
label
linefill.delete

Deletes the specified linefill object. If it has already been deleted, does nothing.
linefill.delete(id) → void
ARGUMENTS
id (series linefill) A linefill object.
linefill.get_line1

Returns the ID of the first line used in the `id` linefill.
linefill.get_line1(id) → series line
ARGUMENTS
id (series linefill) A linefill object.
linefill.get_line2

Returns the ID of the second line used in the `id` linefill.
linefill.get_line2(id) → series line
ARGUMENTS
id (series linefill) A linefill object.
linefill.new

Creates a new linefill object and displays it on the chart, filling the space between `line1` and `line2` with the color specified in `color`.
linefill.new(line1, line2, color) → series linefill
RETURNS
The ID of a linefill object that can be passed to other linefill.*() functions.
ARGUMENTS
line1 (series line) First line object.
line2 (series line) Second line object.
color (series color) The color used to fill the space between the lines.
REMARKS
If any line of the two is deleted, the linefill object is also deleted. If the lines are moved (e.g. via line.set_xy functions), the linefill object is also moved.
If both lines are extended in the same direction relative to the lines themselves (e.g. both have extend.right as the value of their `extend=` parameter), the space between line extensions will also be filled.
linefill.set_color

The function sets the color of the linefill object passed to it.
linefill.set_color(id, color) → void
ARGUMENTS
id (series linefill) A linefill object.
color (series color) The color of the linefill object.
math.abs

Absolute value of `number` is `number` if `number` >= 0, or -`number` otherwise.
math.abs(number) → simple int
math.abs(number) → input int
math.abs(number) → const int
math.abs(number) → series int
math.abs(number) → simple float
math.abs(number) → input float
math.abs(number) → const float
math.abs(number) → series float
RETURNS
The absolute value of `number`.
math.acos

The acos function returns the arccosine (in radians) of number such that cos(acos(y)) = y for y in range [-1, 1].
math.acos(angle) → simple float
math.acos(angle) → input float
math.acos(angle) → const float
math.acos(angle) → series float
RETURNS
The arc cosine of a value; the returned angle is in the range [0, Pi], or na if y is outside of range [-1, 1].
math.asin

The asin function returns the arcsine (in radians) of number such that sin(asin(y)) = y for y in range [-1, 1].
math.asin(angle) → simple float
math.asin(angle) → input float
math.asin(angle) → const float
math.asin(angle) → series float
RETURNS
The arcsine of a value; the returned angle is in the range [-Pi/2, Pi/2], or na if y is outside of range [-1, 1].
math.atan

The atan function returns the arctangent (in radians) of number such that tan(atan(y)) = y for any y.
math.atan(angle) → simple float
math.atan(angle) → input float
math.atan(angle) → const float
math.atan(angle) → series float
RETURNS
The arc tangent of a value; the returned angle is in the range [-Pi/2, Pi/2].
math.avg

Calculates average of all given series (elementwise).
math.avg(number0, number1, ...) → simple float
math.avg(number0, number1, ...) → series float
RETURNS
Average.
SEE ALSO
math.sum
ta.cum
ta.sma
math.ceil

The ceil function returns the smallest (closest to negative infinity) integer that is greater than or equal to the argument.
math.ceil(number) → simple int
math.ceil(number) → input int
math.ceil(number) → const int
math.ceil(number) → series int
RETURNS
The smallest integer greater than or equal to the given number.
SEE ALSO
math.floor
math.round
math.cos

The cos function returns the trigonometric cosine of an angle.
math.cos(angle) → simple float
math.cos(angle) → input float
math.cos(angle) → const float
math.cos(angle) → series float
RETURNS
The trigonometric cosine of an angle.
ARGUMENTS
angle (series int/float) Angle, in radians.
math.exp

The exp function of `number` is e raised to the power of `number`, where e is Euler's number.
math.exp(number) → simple float
math.exp(number) → input float
math.exp(number) → const float
math.exp(number) → series float
RETURNS
A value representing e raised to the power of `number`.
SEE ALSO
math.pow
math.floor

math.floor(number) → simple int
math.floor(number) → input int
math.floor(number) → const int
math.floor(number) → series int
RETURNS
The largest integer less than or equal to the given number.
SEE ALSO
math.ceil
math.round
math.log

Natural logarithm of any `number` > 0 is the unique y such that e^y = `number`.
math.log(number) → simple float
math.log(number) → input float
math.log(number) → const float
math.log(number) → series float
RETURNS
The natural logarithm of `number`.
SEE ALSO
math.log10
math.log10

The common (or base 10) logarithm of `number` is the power to which 10 must be raised to obtain the `number`. 10^y = `number`.
math.log10(number) → simple float
math.log10(number) → input float
math.log10(number) → const float
math.log10(number) → series float
RETURNS
The base 10 logarithm of `number`.
SEE ALSO
math.log
math.max

Returns the greatest of multiple values.
math.max(number0, number1, ...) → simple int
math.max(number0, number1, ...) → simple float
math.max(number0, number1, ...) → input int
math.max(number0, number1, ...) → input float
math.max(number0, number1, ...) → series int
math.max(number0, number1, ...) → series float
EXAMPLE

//@version=5
indicator("math.max", overlay=true)
plot(math.max(close, open))
plot(math.max(close, math.max(open, 42)))
RETURNS
The greatest of multiple given values.
SEE ALSO
math.min
math.min

Returns the smallest of multiple values.
math.min(number0, number1, ...) → simple int
math.min(number0, number1, ...) → simple float
math.min(number0, number1, ...) → input int
math.min(number0, number1, ...) → input float
math.min(number0, number1, ...) → series int
math.min(number0, number1, ...) → series float
EXAMPLE

//@version=5
indicator("math.min", overlay=true)
plot(math.min(close, open))
plot(math.min(close, math.min(open, 42)))
RETURNS
The smallest of multiple given values.
SEE ALSO
math.max
math.pow

Mathematical power function.
math.pow(base, exponent) → simple float
math.pow(base, exponent) → input float
math.pow(base, exponent) → const float
math.pow(base, exponent) → series float
EXAMPLE

//@version=5
indicator("math.pow", overlay=true)
plot(math.pow(close, 2))
RETURNS
`base` raised to the power of `exponent`. If `base` is a series, it is calculated elementwise.
ARGUMENTS
base (series int/float) Specify the base to use.
exponent (series int/float) Specifies the exponent.
SEE ALSO
math.sqrt
math.exp
math.random

Returns a pseudo-random value. The function will generate a different sequence of values for each script execution. Using the same value for the optional seed argument will produce a repeatable sequence.
math.random(min, max, seed) → series float
RETURNS
A random value.
ARGUMENTS
min (series int/float) The lower bound of the range of random values. The value is not included in the range. The default is 0.
max (series int/float) The upper bound of the range of random values. The value is not included in the range. The default is 1.
seed (series int) Optional argument. When the same seed is used, allows successive calls to the function to produce a repeatable set of values.
math.round

Returns the value of `number` rounded to the nearest integer, with ties rounding up. If the `precision` parameter is used, returns a float value rounded to that amount of decimal places.
math.round(number) → simple int
math.round(number) → input int
math.round(number) → const int
math.round(number) → series int
math.round(number, precision) → simple float
math.round(number, precision) → input float
math.round(number, precision) → const float
math.round(number, precision) → series float
RETURNS
The value of `number` rounded to the nearest integer, or according to precision.
ARGUMENTS
number (series int/float) The value to be rounded.
precision (series int) Optional argument. Decimal places to which `number` will be rounded. When no argument is supplied, rounding is to the nearest integer.
REMARKS
Note that for 'na' values function returns 'na'.
SEE ALSO
math.ceil
math.floor
math.round_to_mintick

Returns the value rounded to the symbol's mintick, i.e. the nearest value that can be divided by syminfo.mintick, without the remainder, with ties rounding up.
math.round_to_mintick(number) → simple float
math.round_to_mintick(number) → series float
RETURNS
The `number` rounded to tick precision.
ARGUMENTS
number (series int/float) The value to be rounded.
REMARKS
Note that for 'na' values function returns 'na'.
SEE ALSO
math.ceil
math.floor
math.sign

Sign (signum) of `number` is zero if `number` is zero, 1.0 if `number` is greater than zero, -1.0 if `number` is less than zero.
math.sign(number) → simple float
math.sign(number) → input float
math.sign(number) → const float
math.sign(number) → series float
RETURNS
The sign of the argument.
math.sin

The sin function returns the trigonometric sine of an angle.
math.sin(angle) → simple float
math.sin(angle) → input float
math.sin(angle) → const float
math.sin(angle) → series float
RETURNS
The trigonometric sine of an angle.
ARGUMENTS
angle (series int/float) Angle, in radians.
math.sqrt

Square root of any `number` >= 0 is the unique y >= 0 such that y^2 = `number`.
math.sqrt(number) → simple float
math.sqrt(number) → input float
math.sqrt(number) → const float
math.sqrt(number) → series float
RETURNS
The square root of `number`.
SEE ALSO
math.pow
math.sum

The sum function returns the sliding sum of last y values of x.
math.sum(source, length) → series float
RETURNS
Sum of `source` for `length` bars back.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
SEE ALSO
ta.cum
math.tan

The tan function returns the trigonometric tangent of an angle.
math.tan(angle) → simple float
math.tan(angle) → input float
math.tan(angle) → const float
math.tan(angle) → series float
RETURNS
The trigonometric tangent of an angle.
ARGUMENTS
angle (series int/float) Angle, in radians.
math.todegrees

Returns an approximately equivalent angle in degrees from an angle measured in radians.
math.todegrees(radians) → series float
RETURNS
The angle value in degrees.
ARGUMENTS
radians (series int/float) Angle in radians.
math.toradians

Returns an approximately equivalent angle in radians from an angle measured in degrees.
math.toradians(degrees) → series float
RETURNS
The angle value in radians.
ARGUMENTS
degrees (series int/float) Angle in degrees.
matrix.add_col

The function adds a column at the `column` index of the `id` matrix. The column can consist of `na` values, or an array can be used to provide values.
matrix.add_col(id, column) → void
matrix.add_col(id, column, array_id) → void
Adding a column to the matrix
EXAMPLE

//@version=5
indicator("`matrix.add_col()` Example 1")

// Create a 2x3 "int" matrix containing values `0`.
m = matrix.new<int>(2, 3, 0)

// Add a column  with `na` values to the matrix.
matrix.add_col(m)

// Display matrix elements.
if barstate.islastconfirmedhistory
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m))
Adding an array as a column to the matrix
EXAMPLE

//@version=5
indicator("`matrix.add_col()` Example 2")

if barstate.islastconfirmedhistory
    // Create an empty matrix object. 
    var m = matrix.new<int>()
    
    // Create an array with values `1` and `3`.
    var a = array.from(1, 3)
    
    // Add the `a` array as the first column of the empty matrix.
    matrix.add_col(m, 0, a)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m))
ARGUMENTS
id (any matrix type) A matrix object.
column (series int) The index of the column after which the new column will be inserted. Optional. The default value is matrix.columns.
array_id (any array type) An array to be inserted. Optional.
REMARKS
Rather than add columns to an empty matrix, it is far more efficient to declare a matrix with explicit dimensions and fill it with values. Adding a column is also much slower than adding a row with the matrix.add_row function.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.add_row
matrix.add_row

The function adds a row at the `row` index of the `id` matrix. The row can consist of `na` values, or an array can be used to provide values.
matrix.add_row(id, row) → void
matrix.add_row(id, row, array_id) → void
Adding a row to the matrix
EXAMPLE

//@version=5
indicator("`matrix.add_row()` Example 1")

// Create a 2x3 "int" matrix containing values `0`.
m = matrix.new<int>(2, 3, 0)

// Add a row with `na` values to the matrix.
matrix.add_row(m)

// Display matrix elements.
if barstate.islastconfirmedhistory
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m))
Adding an array as a row to the matrix
EXAMPLE

//@version=5
indicator("`matrix.add_row()` Example 2")

if barstate.islastconfirmedhistory
    // Create an empty matrix object. 
    var m = matrix.new<int>()
    
    // Create an array with values `1` and `2`.
    var a = array.from(1, 2)
    
    // Add the `a` array as the first row of the empty matrix.
    matrix.add_row(m, 0, a)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m))
ARGUMENTS
id (any matrix type) A matrix object.
row (series int) The index of the row after which the new row will be inserted. Optional. The default value is matrix.rows.
array_id (any array type) An array to be inserted. Optional.
REMARKS
Indexing of rows and columns starts at zero. Rather than add rows to an empty matrix, it is far more efficient to declare a matrix with explicit dimensions and fill it with values.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.add_col
matrix.avg

The function calculates the average of all elements in the matrix.
matrix.avg(id) → series float
matrix.avg(id) → series int
EXAMPLE

//@version=5
indicator("`matrix.avg()` Example")

// Create a 2x2 matrix.
var m = matrix.new<int>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0, 1)
matrix.set(m, 0, 1, 2)
matrix.set(m, 1, 0, 3)
matrix.set(m, 1, 1, 4)

// Get the average value of the matrix.
var x = matrix.avg(m)

plot(x, 'Matrix average value')
RETURNS
The average value from the `id` matrix.
ARGUMENTS
id (matrix<float>/matrix<int>) A matrix object.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.col

The function creates a one-dimensional array from the elements of a matrix column.
matrix.col(id, column) → type[]
EXAMPLE

//@version=5
indicator("`matrix.col()` Example", "", true)

// Create a 2x3 "float" matrix from `hlc3` values.
m = matrix.new<float>(2, 3, hlc3)

// Return an array with the values of the first column of matrix `m`.
a = matrix.col(m, 0)

// Plot the first value from the array `a`.
plot(array.get(a, 0))
RETURNS
An array ID containing the `column` values of the `id` matrix.
ARGUMENTS
id (any matrix type) A matrix object.
column (series int) Index of the required column.
REMARKS
Indexing of rows starts at 0.
SEE ALSO
matrix.new<type>
matrix.get
array.get
matrix.col
matrix.columns
matrix.columns

The function returns the number of columns in the matrix.
matrix.columns(id) → series int
EXAMPLE

//@version=5
indicator("`matrix.columns()` Example")

// Create a 2x6 matrix with values `0`.
var m = matrix.new<int>(2, 6, 0)

// Get the quantity of columns in matrix `m`.
var x = matrix.columns(m)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, "Columns: " + str.tostring(x) + "\n" + str.tostring(m))
RETURNS
The number of columns in the matrix `id`.
ARGUMENTS
id (any matrix type) A matrix object.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.col
matrix.row
matrix.rows
matrix.concat

The function appends the `m2` matrix to the `m1` matrix.
matrix.concat(id1, id2) → matrix<type>
EXAMPLE

//@version=5
indicator("`matrix.concat()` Example")

// Create a 2x4 "int" matrix containing values `0`.
m1 = matrix.new<int>(2, 4, 0)
// Create a 2x4 "int" matrix containing values `1`.
m2 = matrix.new<int>(2, 4, 1)

// Append matrix `m2` to `m1`.
matrix.concat(m1, m2)

// Display matrix elements.
if barstate.islastconfirmedhistory
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix Elements:")
    table.cell(t, 0, 1, str.tostring(m1))
RETURNS
Returns the `id1` matrix concatenated with the `id2` matrix.
ARGUMENTS
id1 (any matrix type) Matrix object to concatenate into.
id2 (any matrix type) Matrix object whose elements will be appended to `id1`.
REMARKS
The number of columns in both matrices must be identical.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.copy

The function creates a new matrix which is a copy of the original.
matrix.copy(id) → matrix<type>
EXAMPLE

//@version=5
indicator("`matrix.copy()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 "float" matrix with `1` values.
    var m1 = matrix.new<float>(2, 3, 1)
    
    // Copy the matrix to a new one.
    // Note that unlike what `matrix.copy()` does, 
    // the simple assignment operation `m2 = m1`
    // would NOT create a new copy of the `m1` matrix.
    // It would merely create a copy of its ID referencing the same matrix.
    var m2 = matrix.copy(m1)
    
    // Display using a table.
    var t = table.new(position.top_right, 5, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Matrix Copy:")
    table.cell(t, 1, 1, str.tostring(m2))
RETURNS
A new matrix object of the copied `id` matrix.
ARGUMENTS
id (any matrix type) A matrix object to copy.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.det

The function returns the determinant of a square matrix.
matrix.det(id) → series float
matrix.det(id) → series int
EXAMPLE

//@version=5
indicator("`matrix.det` Example")

// Create a 2x2 matrix.
var m = matrix.new<float>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0,  3)
matrix.set(m, 0, 1,  7)
matrix.set(m, 1, 0,  1)
matrix.set(m, 1, 1, -4)

// Get the determinant of the matrix. 
var x = matrix.det(m)

plot(x, 'Matrix determinant')
RETURNS
The determinant value of the `id` matrix.
ARGUMENTS
id (matrix<float>/matrix<int>) A matrix object.
REMARKS
Function calculation based on the LU decomposition algorithm.
SEE ALSO
matrix.new<type>
matrix.set
matrix.is_square
matrix.diff

The function returns a new matrix resulting from the subtraction between matrices `id1` and `id2`, or of matrix `id1` and an `id2` scalar (a numerical value).
matrix.diff(id1, id2) → matrix<int>
matrix.diff(id1, id2) → matrix<float>
Difference between two matrices
EXAMPLE

//@version=5
indicator("`matrix.diff()` Example 1")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix containing values `5`.
    var m1 = matrix.new<float>(2, 3, 5) 
    // Create a 2x3 matrix containing values `4`.
    var m2 = matrix.new<float>(2, 3, 4) 
    // Create a new matrix containing the difference between matrices `m1` and `m2`.
    var m3 = matrix.diff(m1, m2) 
    
    // Display using a table.
    var t = table.new(position.top_right, 1, 2, color.green)
    table.cell(t, 0, 0, "Difference between two matrices:")
    table.cell(t, 0, 1, str.tostring(m3))
Difference between a matrix and a scalar value
EXAMPLE

//@version=5
indicator("`matrix.diff()` Example 2")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix with values `4`.
    var m1 = matrix.new<float>(2, 3, 4)
    
    // Create a new matrix containing the difference between the `m1` matrix and the "int" value `1`.
    var m2 = matrix.diff(m1, 1)
    
    // Display using a table.
    var t = table.new(position.top_right, 1, 2, color.green)
    table.cell(t,  0, 0, "Difference between a matrix and a scalar:")
    table.cell(t,  0, 1, str.tostring(m2))
RETURNS
A new matrix object containing the difference between `id2` and `id1`.
ARGUMENTS
id1 (matrix<int>/matrix<float>) Matrix to subtract from.
id2 (series int/float/matrix<int>/matrix<float>) Matrix object or a scalar value to be subtracted.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.eigenvalues

The function returns an array containing the eigenvalues of a square matrix.
matrix.eigenvalues(id) → float[]
matrix.eigenvalues(id) → int[]
EXAMPLE

//@version=5
indicator("`matrix.eigenvalues()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix. 
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 2)
    matrix.set(m1, 0, 1, 4)
    matrix.set(m1, 1, 0, 6)
    matrix.set(m1, 1, 1, 8)
    
    // Get the eigenvalues of the matrix.
    tr = matrix.eigenvalues(m1)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Array of Eigenvalues:")
    table.cell(t, 1, 1, str.tostring(tr)) 
RETURNS
An array containing the eigenvalues of the `id` matrix.
ARGUMENTS
id (matrix<float>/matrix<int>) A matrix object.
REMARKS
The function is calculated using "The Implicit QL Algorithm".
SEE ALSO
matrix.new<type>
matrix.set
matrix.eigenvectors
matrix.eigenvectors

Returns a matrix of eigenvectors, in which each column is an eigenvector of the `id` matrix.
matrix.eigenvectors(id) → matrix<float>
matrix.eigenvectors(id) → matrix<int>
EXAMPLE

//@version=5
indicator("`matrix.eigenvectors()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix 
    var m1 = matrix.new<int>(2, 2, 1)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 2)
    matrix.set(m1, 0, 1, 4)
    matrix.set(m1, 1, 0, 6)
    matrix.set(m1, 1, 1, 8)
    
    // Get the eigenvectors of the matrix.
    m2 = matrix.eigenvectors(m1)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix Elements:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Matrix Eigenvectors:")
    table.cell(t, 1, 1, str.tostring(m2))  
RETURNS
A new matrix containing the eigenvectors of the `id` matrix.
ARGUMENTS
id (matrix<float>/matrix<int>) A matrix object.
REMARKS
The function is calculated using "The Implicit QL Algorithm".
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.eigenvalues
matrix.elements_count

The function returns the total number of all matrix elements.
matrix.elements_count(id) → series int
ARGUMENTS
id (any matrix type) A matrix object.
SEE ALSO
matrix.new<type>
matrix.columns
matrix.rows
matrix.fill

The function fills a rectangular area of the `id` matrix defined by the indices `from_column` to `to_column` (not including it) and `from_row` to `to_row`(not including it) with the `value`.
matrix.fill(id, value, from_row, to_row, from_column, to_column) → void
EXAMPLE

//@version=5
indicator("`matrix.fill()` Example")

// Create a 4x5 "int" matrix containing values `0`.
m = matrix.new<float>(4, 5, 0)

// Fill the intersection of rows 1 to 2 and columns 2 to 3 of the matrix with `hl2` values.
matrix.fill(m, hl2, 0, 2, 1, 3)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m))
ARGUMENTS
id (any matrix type) A matrix object.
value (series <type of the matrix's elements>) The value to fill with.
from_column (series int) Column index from which the fill will begin (inclusive). Optional. The default value is 0.
to_column (series int) Column index where the fill will end (non inclusive). Optional. The default value is matrix.columns.
from_row (series int) Row index from which the fill will begin (inclusive). Optional. The default value is 0.
to_row (series int) Row index where the fill will end (not inclusive). Optional. The default value is matrix.rows.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.get

The function returns the element with the specified index of the matrix.
matrix.get(id, row, column) → <matrix_type>
EXAMPLE

//@version=5
indicator("`matrix.get()` Example", "", true)

// Create a 2x3 "float" matrix from the `hl2` values.
m = matrix.new<float>(2, 3, hl2)

// Return the value of the element at index [0, 0] of matrix `m`.
x = matrix.get(m, 0, 0)

plot(x)
RETURNS
The value of the element at the `row` and `column` index of the `id` matrix.
ARGUMENTS
id (any matrix type) A matrix object.
row (series int) Index of the required row.
column (series int) Index of the required column.
REMARKS
Indexing of the rows and columns starts at zero.
SEE ALSO
matrix.new<type>
matrix.set
matrix.columns
matrix.rows
matrix.inv

The function returns the inverse of a square matrix.
matrix.inv(id) → matrix<float>
matrix.inv(id) → matrix<int>
EXAMPLE

//@version=5
indicator("`matrix.inv()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix. 
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)
    
    // Inverse of the matrix.
    var m2 = matrix.inv(m1)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Inverse matrix:")
    table.cell(t, 1, 1, str.tostring(m2))
RETURNS
A new matrix, which is the inverse of the `id` matrix.
ARGUMENTS
id (matrix<float>/matrix<int>) A matrix object.
REMARKS
The function is calculated using the LU decomposition algorithm.
SEE ALSO
matrix.new<type>
matrix.set
matrix.pinv
matrix.copy
str.tostring
matrix.is_antidiagonal

The function determines if the matrix is anti-diagonal (all elements outside the secondary diagonal are zero).
matrix.is_antidiagonal(id) → series bool
RETURNS
Returns true if the `id` matrix is ​​anti-diagonal, false otherwise.
ARGUMENTS
id (matrix<float>/matrix<int>) Matrix object to test.
REMARKS
Returns false with non-square matrices.
SEE ALSO
matrix.new<type>
matrix.set
matrix.is_square
matrix.is_identity
matrix.is_diagonal
matrix.is_antisymmetric

The function determines if a matrix is antisymmetric (its transpose equals its negative).
matrix.is_antisymmetric(id) → series bool
RETURNS
Returns true, if the `id` matrix is antisymmetric, false otherwise.
ARGUMENTS
id (matrix<float>/matrix<int>) Matrix object to test.
REMARKS
Returns false with non-square matrices.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.is_square
matrix.is_binary

The function determines if the matrix is binary (when all elements of the matrix are 0 or 1).
matrix.is_binary(id) → series bool
RETURNS
Returns true if the `id` matrix is binary, false otherwise.
ARGUMENTS
id (matrix<float>/matrix<int>) Matrix object to test.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.is_diagonal

The function determines if the matrix is diagonal (all elements outside the main diagonal are zero).
matrix.is_diagonal(id) → series bool
RETURNS
Returns true if the `id` matrix is diagonal, false otherwise.
ARGUMENTS
id (matrix<float>/matrix<int>) Matrix object to test.
REMARKS
Returns false with non-square matrices.
SEE ALSO
matrix.new<type>
matrix.set
matrix.is_square
matrix.is_identity
matrix.is_antidiagonal
matrix.is_identity

The function determines if a matrix is an identity matrix (elements with ones on the main diagonal and zeros elsewhere).
matrix.is_identity(id) → series bool
RETURNS
Returns true if `id` is an identity matrix, false otherwise.
ARGUMENTS
id (matrix<float>/matrix<int>) Matrix object to test.
REMARKS
Returns false with non-square matrices.
SEE ALSO
matrix.new<type>
matrix.is_square
matrix.is_diagonal
matrix.is_square

The function determines if the matrix is square (it has the same number of rows and columns).
matrix.is_square(id) → series bool
RETURNS
Returns true if the `id` matrix is square, false otherwise.
ARGUMENTS
id (any matrix type) Matrix object to test.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.is_stochastic

The function determines if the matrix is stochastic.
matrix.is_stochastic(id) → series bool
RETURNS
Returns true if the `id` matrix is stochastic, false otherwise.
ARGUMENTS
id (matrix<float>/matrix<int>) Matrix object to test.
SEE ALSO
matrix.new<type>
matrix.set
matrix.is_symmetric

The function determines if a square matrix is symmetric (elements are symmetric with respect to the main diagonal).
matrix.is_symmetric(id) → series bool
RETURNS
Returns true if the `id` matrix is symmetric, false otherwise.
ARGUMENTS
id (matrix<float>/matrix<int>) Matrix object to test.
REMARKS
Returns false with non-square matrices.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.is_square
matrix.is_triangular

The function determines if the matrix is triangular (if all elements above or below the main diagonal are zero).
matrix.is_triangular(id) → series bool
RETURNS
Returns true if the `id` matrix is triangular, false otherwise.
ARGUMENTS
id (matrix<float>/matrix<int>) Matrix object to test.
REMARKS
Returns false with non-square matrices.
SEE ALSO
matrix.new<type>
matrix.set
matrix.is_square
matrix.is_zero

The function determines if all elements of the matrix are zero.
matrix.is_zero(id) → series bool
RETURNS
Returns true if all elements of the `id` matrix are zero, false otherwise.
ARGUMENTS
id (matrix<float>/matrix<int>) Matrix object to check.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.kron

The function returns the Kronecker product for the `id1` and `id2` matrices.
matrix.kron(id1, id2) → matrix<float>
matrix.kron(id1, id2) → matrix<int>
EXAMPLE

//@version=5
indicator("`matrix.kron()` Example")

// Display using a table.
if barstate.islastconfirmedhistory
    // Create two matrices with default values `1` and `2`. 
    var m1 = matrix.new<float>(2, 2, 1) 
    var m2 = matrix.new<float>(2, 2, 2) 
    
    // Calculate the Kronecker product of the matrices.
    var m3 = matrix.kron(m1, m2) 
    
    // Display matrix elements.
    var t = table.new(position.top_right, 5, 2, color.green)
    table.cell(t, 0, 0, "Matrix 1:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 1, "⊗")
    table.cell(t, 2, 0, "Matrix 2:")
    table.cell(t, 2, 1, str.tostring(m2))
    table.cell(t, 3, 1, "=")
    table.cell(t, 4, 0, "Kronecker product:")
    table.cell(t, 4, 1, str.tostring(m3))
RETURNS
A new matrix containing the Kronecker product of `id1` and `id2`.
ARGUMENTS
id1 (matrix<float>/matrix<int>) First matrix object.
id2 (matrix<float>/matrix<int>) Second matrix object.
SEE ALSO
matrix.new<type>
matrix.mult
str.tostring
table.new
matrix.max

The function returns the largest value from the matrix elements.
matrix.max(id) → series float
matrix.max(id) → series int
EXAMPLE

//@version=5
indicator("`matrix.max()` Example")

// Create a 2x2 matrix.
var m = matrix.new<int>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0, 1)
matrix.set(m, 0, 1, 2)
matrix.set(m, 1, 0, 3)
matrix.set(m, 1, 1, 4)

// Get the maximum value in the matrix.
var x = matrix.max(m)

plot(x, 'Matrix maximum value')
RETURNS
The maximum value from the `id` matrix.
ARGUMENTS
id (matrix<float>/matrix<int>) A matrix object.
SEE ALSO
matrix.new<type>
matrix.min
matrix.avg
matrix.sort
matrix.median

The function calculates the median ("the middle" value) of matrix elements.
matrix.median(id) → series float
matrix.median(id) → series int
EXAMPLE

//@version=5
indicator("`matrix.median()` Example")

// Create a 2x2 matrix.
m = matrix.new<int>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0, 1)
matrix.set(m, 0, 1, 2)
matrix.set(m, 1, 0, 3)
matrix.set(m, 1, 1, 4)

// Get the median of the matrix.
x = matrix.median(m)

plot(x, 'Median of the matrix')
ARGUMENTS
id (matrix<float>/matrix<int>) A matrix object.
REMARKS
Note that na elements of the matrix are not considered when calculating the median.
SEE ALSO
matrix.new<type>
matrix.mode
matrix.sort
matrix.avg
matrix.min

The function returns the smallest value from the matrix elements.
matrix.min(id) → series float
matrix.min(id) → series int
EXAMPLE

//@version=5
indicator("`matrix.min()` Example")

// Create a 2x2 matrix.
var m = matrix.new<int>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0, 1)
matrix.set(m, 0, 1, 2)
matrix.set(m, 1, 0, 3)
matrix.set(m, 1, 1, 4)

// Get the minimum value from the matrix.
var x = matrix.min(m)

plot(x, 'Matrix minimum value')
RETURNS
The smallest value from the `id` matrix.
ARGUMENTS
id (matrix<float>/matrix<int>) A matrix object.
SEE ALSO
matrix.new<type>
matrix.max
matrix.avg
matrix.sort
matrix.mode

The function calculates the mode) of the matrix, which is the most frequently occurring value from the matrix elements. When there are multiple values occurring equally frequently, the function returns the smallest of those values.
matrix.mode(id) → series float
matrix.mode(id) → series int
EXAMPLE

//@version=5
indicator("`matrix.mode()` Example")

// Create a 2x2 matrix.
var m = matrix.new<int>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0, 0)
matrix.set(m, 0, 1, 0)
matrix.set(m, 1, 0, 1)
matrix.set(m, 1, 1, 1)

// Get the mode of the matrix.
var x = matrix.mode(m)

plot(x, 'Mode of the matrix')
RETURNS
The most frequently occurring value from the `id` matrix. If none exists, returns the smallest value instead.
ARGUMENTS
id (matrix<float>/matrix<int>) A matrix object.
REMARKS
Note that na elements of the matrix are not considered when calculating the mode.
SEE ALSO
matrix.new<type>
matrix.set
matrix.median
matrix.sort
matrix.avg
matrix.mult

The function returns a new matrix resulting from the product between the matrices `id1` and `id2`, or between an `id1` matrix and an `id2` scalar (a numerical value), or between an `id1` matrix and an `id2` vector (an array of values).
matrix.mult(id1, id2) → matrix<int>
matrix.mult(id1, id2) → matrix<float>
matrix.mult(id1, id2) → int[]
matrix.mult(id1, id2) → float[]
Product of two matrices
EXAMPLE

//@version=5
indicator("`matrix.mult()` Example 1")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 6x2 matrix containing values `5`.
    var m1 = matrix.new<float>(6, 2, 5) 
    // Create a 2x3 matrix containing values `4`.
    // Note that it must have the same quantity of rows as there are columns in the first matrix.
    var m2 = matrix.new<float>(2, 3, 4) 
    // Create a new matrix from the multiplication of the two matrices.
    var m3 = matrix.mult(m1, m2) 
    
    // Display using a table.
    var t = table.new(position.top_right, 1, 2, color.green)
    table.cell(t, 0, 0, "Product of two matrices:")
    table.cell(t, 0, 1, str.tostring(m3))
Product of a matrix and a scalar
EXAMPLE

//@version=5
indicator("`matrix.mult()` Example 2")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix containing values `4`.
    var m1 = matrix.new<float>(2, 3, 4) 
    
    // Create a new matrix from the product of the two matrices.
    scalar = 5
    var m2 = matrix.mult(m1, scalar) 
    
    // Display using a table.
    var t = table.new(position.top_right, 5, 2, color.green)
    table.cell(t, 0, 0, "Matrix 1:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 1, "x")
    table.cell(t, 2, 0, "Scalar:")
    table.cell(t, 2, 1, str.tostring(scalar))
    table.cell(t, 3, 1, "=")
    table.cell(t, 4, 0, "Matrix 2:")
    table.cell(t, 4, 1, str.tostring(m2))
Product of a matrix and an array vector
EXAMPLE

//@version=5
indicator("`matrix.mult()` Example 3")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix containing values `4`.
    var m1 = matrix.new<int>(2, 3, 4)
    
    // Create an array of three elements.
    var int[] a = array.from(1, 1, 1)
    
    // Create a new matrix containing the product of the `m1` matrix and the `a` array.
    var m3 = matrix.mult(m1, a) 
    
    // Display using a table.
    var t = table.new(position.top_right, 5, 2, color.green)
    table.cell(t, 0, 0, "Matrix 1:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 1, "x")
    table.cell(t, 2, 0, "Value:")
    table.cell(t, 2, 1, str.tostring(a, " "))
    table.cell(t, 3, 1, "=")
    table.cell(t, 4, 0, "Matrix 3:")
    table.cell(t, 4, 1, str.tostring(m3))
RETURNS
A new matrix object containing the product of `id2` and `id1`.
ARGUMENTS
id1 (matrix<int>/matrix<float>) First matrix object.
id2 (series int/float/matrix<int>/matrix<float>/int[]/float[]) Second matrix object, value or array.
SEE ALSO
matrix.new<type>
matrix.sum
matrix.diff
matrix.new<type>

The function creates a new matrix object. A matrix is a two-dimensional data structure containing rows and columns. All elements in the matrix must be of the type specified in the type template ("<type>").
matrix.new<type>(rows, columns, initial_value) → matrix<type>
Create a matrix of elements with the same initial value
EXAMPLE

//@version=5
indicator("`matrix.new<type>()` Example 1")

// Create a 2x3 (2 rows x 3 columns) "int" matrix with values zero.
var m = matrix.new<int>(2, 3, 0)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m))
Create a matrix from array values
EXAMPLE

//@version=5
indicator("`matrix.new<type>()` Example 2")

// Function to create a matrix whose rows are filled with array values.
matrixFromArray(int rows, int columns, array<float> data) =>
    m = matrix.new<float>(rows, columns)
    for i = 0 to rows <= 0 ? na : rows - 1
        for j = 0 to columns <= 0 ? na : columns - 1
            matrix.set(m, i, j, array.get(data, i * columns + j))
    m
    
// Create a 3x3 matrix from an array of values.
var m1 = matrixFromArray(3, 3, array.from(1, 2, 3, 4, 5, 6, 7, 8, 9))
// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m1))
Create a matrix from an `input.text_area()` field
EXAMPLE

//@version=5
indicator("`matrix.new<type>()` Example 3")

// Function to create a matrix from a text string.
// Values in a row must be separated by a space. Each line is one row.
matrixFromInputArea(stringOfValues) =>
    var rowsArray = str.split(stringOfValues, "\n")
    var rows = array.size(rowsArray)
    var cols = array.size(str.split(array.get(rowsArray, 0), " "))
    var matrix = matrix.new<float>(rows, cols, na) 
    row = 0
    for rowString in rowsArray
        col = 0
        values = str.split(rowString, " ")
        for val in values
            matrix.set(matrix, row, col, str.tonumber(val))
            col += 1
        row += 1
    matrix


stringInput = input.text_area("1 2 3\n4 5 6\n7 8 9")
var m = matrixFromInputArea(stringInput)    

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m))
Create matrix from random values
EXAMPLE

//@version=5
indicator("`matrix.new<type>()` Example 4")

// Function to create a matrix with random values (0.0 to 1.0).
matrixRandom(int rows, int columns)=>
    result = matrix.new<float>(rows, columns)
    for i = 0 to rows - 1
        for j = 0 to columns - 1
            matrix.set(result, i, j, math.random())
    result

// Create a 2x3 matrix with random values.
var m = matrixRandom(2, 3)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m))
RETURNS
The ID of the new matrix object.
ARGUMENTS
rows (series int) Initial row count of the matrix. Optional. The default value is 0.
columns (series int) Initial column count of the matrix. Optional. The default value is 0.
initial_value (<matrix_type>) Initial value of all matrix elements. Optional. The default is 'na'.
SEE ALSO
matrix.set
matrix.fill
matrix.columns
matrix.rows
array.new<type>
matrix.pinv

The function returns the pseudoinverse of a matrix.
matrix.pinv(id) → matrix<float>
matrix.pinv(id) → matrix<int>
EXAMPLE

//@version=5
indicator("`matrix.pinv()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix. 
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)
    
    // Pseudoinverse of the matrix.
    var m2 = matrix.pinv(m1)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Pseudoinverse matrix:")
    table.cell(t, 1, 1, str.tostring(m2))
RETURNS
A new matrix containing the pseudoinverse of the `id` matrix.
ARGUMENTS
id (matrix<float>/matrix<int>) A matrix object.
REMARKS
The function is calculated using a Moore–Penrose inverse formula based on singular-value decomposition of a matrix. For non-singular square matrices this function returns the result of matrix.inv.
SEE ALSO
matrix.new<type>
matrix.set
matrix.inv
matrix.pow

The function calculates the product of the matrix by itself `power` times.
matrix.pow(id, power) → matrix<float>
matrix.pow(id, power) → matrix<int>
EXAMPLE

//@version=5
indicator("`matrix.pow()` Example")

// Display using a table.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix. 
    var m1 = matrix.new<int>(2, 2, 2)
    // Calculate the power of three of the matrix.
    var m2 = matrix.pow(m1, 3)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Matrix³:")
    table.cell(t, 1, 1, str.tostring(m2))
RETURNS
The product of the `id` matrix by itself `power` times.
ARGUMENTS
id (matrix<float>/matrix<int>) A matrix object.
power (series int) The number of times the matrix will be multiplied by itself.
SEE ALSO
matrix.new<type>
matrix.set
matrix.mult
matrix.rank

The function calculates the rank) of the matrix.
matrix.rank(id) → series int
EXAMPLE

//@version=5
indicator("`matrix.rank()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix. 
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)
    
    // Get the rank of the matrix. 
    r = matrix.rank(m1)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Rank of the matrix:")
    table.cell(t, 1, 1, str.tostring(r)) 
RETURNS
The rank of the `id` matrix.
ARGUMENTS
id (any matrix type) A matrix object.
SEE ALSO
matrix.new<type>
matrix.set
str.tostring
matrix.remove_col

The function removes the column at `column` index of the `id` matrix and returns an array containing the removed column's values.
matrix.remove_col(id, column) → type[]
EXAMPLE

//@version=5
indicator("matrix_remove_col", overlay = true)

// Create a 2x2 matrix with ones.
var matrixOrig = matrix.new<int>(2, 2, 1)

// Set values to the 'matrixOrig' matrix.
matrix.set(matrixOrig, 0, 1, 2)
matrix.set(matrixOrig, 1, 0, 3)
matrix.set(matrixOrig, 1, 1, 4)


// Create a copy of the 'matrixOrig' matrix.
matrixCopy = matrix.copy(matrixOrig)

// Remove the first column from the `matrixCopy` matrix.
arr = matrix.remove_col(matrixCopy, 0)

// Display matrix elements.
if barstate.islastconfirmedhistory
    var t = table.new(position.top_right, 3, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(matrixOrig))
    table.cell(t, 1, 0, "Removed Elements:")
    table.cell(t, 1, 1, str.tostring(arr))
    table.cell(t, 2, 0, "Result Matrix:")
    table.cell(t, 2, 1, str.tostring(matrixCopy))
RETURNS
An array containing the elements of the column removed from the `id` matrix.
ARGUMENTS
id (any matrix type) A matrix object.
column (series int) The index of the column to be removed. Optional. The default value is matrix.columns.
REMARKS
Indexing of rows and columns starts at zero. It is far more efficient to declare matrices with explicit dimensions than to build them by adding or removing columns. Deleting a column is also much slower than deleting a row with the matrix.remove_row function.
SEE ALSO
matrix.new<type>
matrix.set
matrix.copy
matrix.remove_row
matrix.remove_row

The function removes the row at `row` index of the `id` matrix and returns an array containing the removed row's values.
matrix.remove_row(id, row) → type[]
EXAMPLE

//@version=5
indicator("matrix_remove_row", overlay = true)

// Create a 2x2 "int" matrix containing values `1`.
var matrixOrig = matrix.new<int>(2, 2, 1)

// Set values to the 'matrixOrig' matrix.
matrix.set(matrixOrig, 0, 1, 2)
matrix.set(matrixOrig, 1, 0, 3)
matrix.set(matrixOrig, 1, 1, 4)

// Create a copy of the 'matrixOrig' matrix.
matrixCopy = matrix.copy(matrixOrig)

// Remove the first row from the matrix `matrixCopy`.
arr = matrix.remove_row(matrixCopy, 0)

// Display matrix elements.
if barstate.islastconfirmedhistory
    var t = table.new(position.top_right, 3, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(matrixOrig))
    table.cell(t, 1, 0, "Removed Elements:")
    table.cell(t, 1, 1, str.tostring(arr))
    table.cell(t, 2, 0, "Result Matrix:")
    table.cell(t, 2, 1, str.tostring(matrixCopy))
RETURNS
An array containing the elements of the row removed from the `id` matrix.
ARGUMENTS
id (any matrix type) A matrix object.
row (series int) The index of the row to be deleted. Optional. The default value is matrix.rows.
REMARKS
Indexing of rows and columns starts at zero. It is far more efficient to declare matrices with explicit dimensions than to build them by adding or removing rows.
SEE ALSO
matrix.new<type>
matrix.set
matrix.copy
matrix.remove_col
matrix.reshape

The function rebuilds the `id` matrix to `rows` x `cols` dimensions.
matrix.reshape(id, rows, columns) → void
EXAMPLE

//@version=5
indicator("`matrix.reshape()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix.
    var m1 = matrix.new<float>(2, 3)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 0, 2, 3)
    matrix.set(m1, 1, 0, 4)
    matrix.set(m1, 1, 1, 5)
    matrix.set(m1, 1, 2, 6)
    
    // Copy the matrix to a new one.
    var m2 = matrix.copy(m1)
    
    // Reshape the copy to a 3x2.
    matrix.reshape(m2, 3, 2)
    
    // Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Reshaped matrix:")
    table.cell(t, 1, 1, str.tostring(m2))
ARGUMENTS
id (any matrix type) A matrix object.
rows (series int) The number of rows of the reshaped matrix.
columns (series int) The number of columns of the reshaped matrix.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.add_row
matrix.add_col
matrix.reverse

The function reverses the order of rows and columns in the matrix `id`. The first row and first column become the last, and the last become the first.
matrix.reverse(id) → void
EXAMPLE

//@version=5
indicator("`matrix.reverse()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Copy the matrix to a new one.
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)
    
    // Copy matrix elements to a new matrix.
    var m2 = matrix.copy(m1)
    
    // Reverse the `m2` copy of the original matrix. 
    matrix.reverse(m2)
    
    // Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Reversed matrix:")
    table.cell(t, 1, 1, str.tostring(m2))
ARGUMENTS
id (any matrix type) A matrix object.
SEE ALSO
matrix.new<type>
matrix.set
matrix.columns
matrix.rows
matrix.reshape
matrix.row

The function creates a one-dimensional array from the elements of a matrix row.
matrix.row(id, row) → type[]
EXAMPLE

//@version=5
indicator("`matrix.row()` Example", "", true)

// Create a 2x3 "float" matrix from `hlc3` values.
m = matrix.new<float>(2, 3, hlc3)

// Return an array with the values of the first row of the matrix.
a = matrix.row(m, 0)

// Plot the first value from the array `a`.
plot(array.get(a, 0))
RETURNS
An array ID containing the `row` values of the `id` matrix.
ARGUMENTS
id (any matrix type) A matrix object.
row (series int) Index of the required row.
REMARKS
Indexing of rows starts at 0.
SEE ALSO
matrix.new<type>
matrix.get
array.get
matrix.col
matrix.rows
matrix.rows

The function returns the number of rows in the matrix.
matrix.rows(id) → series int
EXAMPLE

//@version=5
indicator("`matrix.rows()` Example")

// Create a 2x6 matrix with values `0`.
var m = matrix.new<int>(2, 6, 0)

// Get the quantity of rows in the matrix.
var x = matrix.rows(m)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, "Rows: " + str.tostring(x) + "\n" + str.tostring(m))
RETURNS
The number of rows in the matrix `id`.
ARGUMENTS
id (any matrix type) A matrix object.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.row
matrix.set

The function assigns `value` to the element at the `row` and `column` of the `id` matrix.
matrix.set(id, row, column, value) → void
EXAMPLE

//@version=5
indicator("`matrix.set()` Example")

// Create a 2x3 "int" matrix containing values `4`.
m = matrix.new<int>(2, 3, 4)

// Replace the value of element at row 1 and column 2 with value `3`.
matrix.set(m, 0, 1, 3)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m))
ARGUMENTS
id (any matrix type) A matrix object.
row (series int) The row index of the element to be modified.
column (series int) The column index of the element to be modified.
value (series <type of the matrix's elements>) The new value to be set.
SEE ALSO
matrix.new<type>
matrix.get
matrix.columns
matrix.rows
matrix.sort

The function rearranges the rows in the `id` matrix following the sorted order of the values in the `column`.
matrix.sort(id, column, order) → void
EXAMPLE

//@version=5
indicator("`matrix.sort()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix. 
    var m1 = matrix.new<float>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 3)
    matrix.set(m1, 0, 1, 4)
    matrix.set(m1, 1, 0, 1)
    matrix.set(m1, 1, 1, 2)
    
    // Copy the matrix to a new one.
    var m2 = matrix.copy(m1)
    // Sort the rows of `m2` using the default arguments (first column and ascending order).
    matrix.sort(m2)
    
    // Display using a table.
    if barstate.islastconfirmedhistory
        var t = table.new(position.top_right, 2, 2, color.green)
        table.cell(t, 0, 0, "Original matrix:")
        table.cell(t, 0, 1, str.tostring(m1))
        table.cell(t, 1, 0, "Sorted matrix:")
        table.cell(t, 1, 1, str.tostring(m2))
ARGUMENTS
id (matrix<int>/matrix<float>/matrix<string>) A matrix object to be sorted.
column (series int) Index of the column whose sorted values determine the new order of rows. Optional. The default value is 0.
order (simple sort_order) The sort order. Possible values: order.ascending (default), order.descending.
SEE ALSO
matrix.new<type>
matrix.max
matrix.min
matrix.avg
matrix.submatrix

The function extracts a submatrix of the `id` matrix within the specified indices.
matrix.submatrix(id, from_row, to_row, from_column, to_column) → matrix<type>
EXAMPLE

//@version=5
indicator("`matrix.submatrix()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix matrix with values `0`.
    var m1 = matrix.new<int>(2, 3, 0)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 0, 2, 3)
    matrix.set(m1, 1, 0, 4)
    matrix.set(m1, 1, 1, 5)
    matrix.set(m1, 1, 2, 6)
    
    // Create a 2x2 submatrix of the `m1` matrix.
    var m2 = matrix.submatrix(m1, 0, 2, 1, 3)
    
    // Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Submatrix:")
    table.cell(t, 1, 1, str.tostring(m2))
RETURNS
A new matrix object containing the submatrix of the `id` matrix defined by the `from_row`, `to_row`, `from_column` and `to_column` indices.
ARGUMENTS
id (any matrix type) A matrix object.
from_column (series int) Index of the column from which the extraction will begin (inclusive). Optional. The default value is 0.
to_column (series int) Index of the column where the extraction will end (non inclusive). Optional. The default value is matrix.columns.
from_row (series int) Index of the row from which the extraction will begin (inclusive). Optional. The default value is 0.
to_row (series int) Index of the row where the extraction will end (non inclusive). Optional. The default value is matrix.rows.
REMARKS
Indexing of the rows and columns starts at zero.
SEE ALSO
matrix.new<type>
matrix.set
matrix.row
matrix.col
matrix.reshape
matrix.sum

The function returns a new matrix resulting from the sum of two matrices `id1` and `id2`, or of an `id1` matrix and an `id2` scalar (a numerical value).
matrix.sum(id1, id2) → matrix<int>
matrix.sum(id1, id2) → matrix<float>
Sum of two matrices
EXAMPLE

//@version=5
indicator("`matrix.sum()` Example 1")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix containing values `5`.
    var m1 = matrix.new<float>(2, 3, 5) 
    // Create a 2x3 matrix containing values `4`.
    var m2 = matrix.new<float>(2, 3, 4) 
    // Create a new matrix that sums matrices `m1` and `m2`.
    var m3 = matrix.sum(m1, m2) 
    
    // Display using a table.
    var t = table.new(position.top_right, 1, 2, color.green)
    table.cell(t, 0, 0, "Sum of two matrices:")
    table.cell(t, 0, 1, str.tostring(m3))
Sum of a matrix and scalar
EXAMPLE

//@version=5
indicator("`matrix.sum()` Example 2")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix with values `4`.
    var m1 = matrix.new<float>(2, 3, 4)
    
    // Create a new matrix containing the sum of the `m1` matrix with the "int" value `1`.
    var m2 = matrix.sum(m1, 1)
    
    // Display using a table.
    var t = table.new(position.top_right, 1, 2, color.green)
    table.cell(t, 0, 0, "Sum of a matrix and a scalar:")
    table.cell(t, 0, 1, str.tostring(m2))
RETURNS
A new matrix object containing the sum of `id2` and `id1`.
ARGUMENTS
id1 (matrix<int>/matrix<float>) First matrix object.
id2 (series int/float/matrix<int>/matrix<float>) Second matrix object, or scalar value.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.swap_columns

The function swaps the columns at the index `column1` and `column2` in the `id` matrix.
matrix.swap_columns(id, column1, column2) → void
EXAMPLE

//@version=5
indicator("`matrix.swap_columns()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix with ‘na’ values.
    var m1 = matrix.new<int>(2, 2, na)    
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)
    
    // Copy the matrix to a new one.
    var m2 = matrix.copy(m1)
    
    // Swap the first and second columns of the matrix copy.
    matrix.swap_columns(m2, 0, 1)

    // Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Swapped columns in copy:")
    table.cell(t, 1, 1, str.tostring(m2))
ARGUMENTS
id (any matrix type) A matrix object.
column1 (series int) Index of the first column to be swapped.
column2 (series int) Index of the second column to be swapped.
REMARKS
Indexing of the rows and columns starts at zero.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.swap_rows

The function swaps the rows at the index `row1` and `row2` in the `id` matrix.
matrix.swap_rows(id, row1, row2) → void
EXAMPLE

//@version=5
indicator("`matrix.swap_rows()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 3x2 matrix with ‘na’ values.
    var m1 = matrix.new<int>(3, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)
    matrix.set(m1, 2, 0, 5)
    matrix.set(m1, 2, 1, 6)
    
    // Copy the matrix to a new one.
    var m2 = matrix.copy(m1)
    
    // Swap the first and second rows of the matrix copy.
    matrix.swap_rows(m2, 0, 1)
    
    // Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Swapped rows in copy:")
    table.cell(t, 1, 1, str.tostring(m2))
ARGUMENTS
id (any matrix type) A matrix object.
row1 (series int) Index of the first row to be swapped.
row2 (series int) Index of the second row to be swapped.
REMARKS
Indexing of the rows and columns starts at zero.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.swap_columns
matrix.trace

The function calculates the trace) of a matrix (the sum of the main diagonal's elements).
matrix.trace(id) → series float
matrix.trace(id) → series int
EXAMPLE

//@version=5
indicator("`matrix.trace()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix. 
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)
    
    // Get the trace of the matrix.
    tr = matrix.trace(m1)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Trace of the matrix:")
    table.cell(t, 1, 1, str.tostring(tr))
RETURNS
The trace of the `id` matrix.
ARGUMENTS
id (matrix<float>/matrix<int>) A matrix object.
SEE ALSO
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.transpose

The function creates a new, transposed version of the `id`. This interchanges the row and column index of each element.
matrix.transpose(id) → matrix<type>
EXAMPLE

//@version=5
indicator("`matrix.transpose()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix. 
    var m1 = matrix.new<float>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)
    
    // Create a transpose of the matrix.
    var m2 = matrix.transpose(m1)
    
    // Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Transposed matrix:")
    table.cell(t, 1, 1, str.tostring(m2))
RETURNS
A new matrix containing the transposed version of the `id` matrix.
ARGUMENTS
id (any matrix type) A matrix object.
SEE ALSO
matrix.new<type>
matrix.set
matrix.columns
matrix.rows
matrix.reshape
matrix.reverse
max_bars_back

Function sets the maximum number of bars that is available for historical reference of a given built-in or user variable. When operator '[]' is applied to a variable - it is a reference to a historical value of that variable.
If an argument of an operator '[]' is a compile time constant value (e.g. 'v[10]', 'close[500]') then there is no need to use 'max_bars_back' function for that variable. Pine Script™ compiler will use that constant value as history buffer size.
If an argument of an operator '[]' is a value, calculated at runtime (e.g. 'v[i]' where 'i' - is a series variable) then Pine Script™ attempts to autodetect the history buffer size at runtime. Sometimes it fails and the script crashes at runtime because it eventually refers to historical values that are out of the buffer. In that case you should use 'max_bars_back' to fix that problem manually.
max_bars_back(var, num) → void
EXAMPLE

//@version=5
indicator("max_bars_back")
close_() => close
depth() => 400
d = depth()
v = close_()
max_bars_back(v, 500)
out = if bar_index > 0
    v[d]
else
    v
plot(out)
RETURNS
void
ARGUMENTS
var (series int/float/bool/color/label/line) Series variable identifier for which history buffer should be resized. Possible values are: 'open', 'high', 'low', 'close', 'volume', 'time', or any user defined variable id.
num (const int) History buffer size which is the number of bars that could be referenced for variable 'var'.
REMARKS
At the moment 'max_bars_back' cannot be applied to built-ins like 'hl2', 'hlc3', 'ohlc4'. Please use multiple 'max_bars_back' calls as workaround here (e.g. instead of a single ‘max_bars_back(hl2, 100)’ call you should call the function twice: ‘max_bars_back(high, 100), max_bars_back(low, 100)’).
If the indicator or strategy 'max_bars_back' parameter is used, all variables in the indicator are affected. This may result in excessive memory usage and cause runtime problems. When possible (i.e. when the cause is a variable rather than a function), please use the max_bars_back function instead.
SEE ALSO
indicator
minute

minute(time) → series int
minute(time, timezone) → series int
RETURNS
Minute (in exchange timezone) for provided UNIX time.
ARGUMENTS
time (series int) UNIX time in milliseconds.
timezone (series string) Allows adjusting the returned value to a time zone specified in either UTC/GMT notation (e.g., "UTC-5", "GMT+0530") or as an IANA time zone database name (e.g., "America/New_York"). Optional. The default is syminfo.timezone.
REMARKS
UNIX time is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
SEE ALSO
minute
time
year
month
dayofmonth
dayofweek
hour
second
month

month(time) → series int
month(time, timezone) → series int
RETURNS
Month (in exchange timezone) for provided UNIX time.
ARGUMENTS
time (series int) UNIX time in milliseconds.
timezone (series string) Allows adjusting the returned value to a time zone specified in either UTC/GMT notation (e.g., "UTC-5", "GMT+0530") or as an IANA time zone database name (e.g., "America/New_York"). Optional. The default is syminfo.timezone.
REMARKS
UNIX time is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
Note that this function returns the month based on the time of the bar's open. For overnight sessions (e.g. EURUSD, where Monday session starts on Sunday, 17:00 UTC-4) this value can be lower by 1 than the month of the trading day.
SEE ALSO
month
time
year
dayofmonth
dayofweek
hour
minute
second
na

Tests if `x` is na.
na(x) → simple bool
na(x) → series bool
EXAMPLE

//@version=5
indicator("na")
// Use the `na()` function to test for `na`.
plot(na(close[1]) ? close : close[1])
// ALTERNATIVE
// `nz()` also tests `close[1]` for `na`. It returns `close[1]` if it is not `na`, and `close` if it is.
plot(nz(close[1], close))
RETURNS
Returns true if `x` is na, false otherwise.
ARGUMENTS
x (series int/float/bool/color/string/label/line/box/table/linefill) Value to be tested.
SEE ALSO
na
fixnan
nz
nz

Replaces NaN values with zeros (or given value) in a series.
nz(source, replacement) → simple int
nz(source, replacement) → simple float
nz(source, replacement) → simple color
nz(source, replacement) → simple bool
nz(source, replacement) → series int
nz(source, replacement) → series float
nz(source, replacement) → series color
nz(source, replacement) → series bool
nz(source) → simple int
nz(source) → simple float
nz(source) → simple color
nz(source) → simple bool
nz(source) → series int
nz(source) → series float
nz(source) → series color
nz(source) → series bool
EXAMPLE

//@version=5
indicator("nz", overlay=true)
plot(nz(ta.sma(close, 100)))
RETURNS
The value of `source` if it is not `na`. If the value of `source` is `na`, returns zero, or the `replacement` argument when one is used.
ARGUMENTS
source (series int/float/bool/color) Series of values to process.
replacement (series int/float/bool/color) Value that will replace all ‘na’ values in the `source` series.
SEE ALSO
na
na
fixnan
plot

Plots a series of data on the chart.
plot(series, title, color, linewidth, style, trackprice, histbase, offset, join, editable, show_last, display) → plot
EXAMPLE

//@version=5
indicator("plot")
plot(high+low, title='Title', color=color.new(#00ffaa, 70), linewidth=2, style=plot.style_area, offset=15, trackprice=true)

// You may fill the background between any two plots with a fill() function:
p1 = plot(open)
p2 = plot(close)
fill(p1, p2, color=color.new(color.green, 90))
RETURNS
A plot object, that can be used in fill
ARGUMENTS
series (series int/float) Series of data to be plotted. Required argument.
title (const string) Title of the plot.
color (series color) Color of the plot. You can use constants like 'color=color.red' or 'color=#ff001a' as well as complex expressions like 'color = close >= open ? color.green : color.red'. Optional argument.
linewidth (input int) Width of the plotted line. Default value is 1. Not applicable to every style.
style (input plot_style) Type of plot. Possible values are: plot.style_line, plot.style_stepline, plot.style_stepline_diamond, plot.style_histogram, plot.style_cross, plot.style_area, plot.style_columns, plot.style_circles, plot.style_linebr, plot.style_areabr, plot.style_steplinebr. Default value is plot.style_line.
trackprice (input bool) If true then a horizontal price line will be shown at the level of the last indicator value. Default is false.
histbase (input int/float) The price value used as the reference level when rendering plot with plot.style_histogram, plot.style_columns or plot.style_area style. Default is 0.0.
offset (series int) Shifts the plot to the left or to the right on the given number of bars. Default is 0.
join (input bool) If true then plot points will be joined with line, applicable only to plot.style_cross and plot.style_circles styles. Default is false.
editable (const bool) If true then plot style will be editable in Format dialog. Default is true.
show_last (input int) If set, defines the number of bars (from the last bar back to the past) to plot on chart.
display (input plot_display) Controls where the plot's information is displayed. Display options support addition and subtraction, meaning that using `display.all - display.status_line` will display the plot's information everywhere except in the script's status line. `display.price_scale + display.status_line` will display the plot only in the price scale and status line. When `display` arguments such as `display.price_scale` have user-controlled chart settings equivalents, the relevant plot information will only appear when all settings allow for it. Possible values: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Optional. The default is display.all.
SEE ALSO
plotshape
plotchar
plotarrow
barcolor
bgcolor
fill
plotarrow

Plots up and down arrows on the chart. Up arrow is drawn at every indicator positive value, down arrow is drawn at every negative value. If indicator returns na then no arrow is drawn. Arrows has different height, the more absolute indicator value the longer arrow is drawn.
plotarrow(series, title, colorup, colordown, offset, minheight, maxheight, editable, show_last, display) → void
EXAMPLE

//@version=5
indicator("plotarrow example", overlay=true)
codiff = close - open
plotarrow(codiff, colorup=color.new(color.teal,40), colordown=color.new(color.orange, 40))
ARGUMENTS
series (series int/float) Series of data to be plotted as arrows. Required argument.
title (const string) Title of the plot.
colorup (series color) Color of the up arrows. Optional argument.
colordown (series color) Color of the down arrows. Optional argument.
offset (series int) Shifts arrows to the left or to the right on the given number of bars. Default is 0.
minheight (input int) Minimal possible arrow height in pixels. Default is 5.
maxheight (input int) Maximum possible arrow height in pixels. Default is 100.
editable (const bool) If true then plotarrow style will be editable in Format dialog. Default is true.
show_last (input int) If set, defines the number of arrows (from the last bar back to the past) to plot on chart.
display (input plot_display) Controls where the plot's information is displayed. Display options support addition and subtraction, meaning that using `display.all - display.status_line` will display the plot's information everywhere except in the script's status line. `display.price_scale + display.status_line` will display the plot only in the price scale and status line. When `display` arguments such as `display.price_scale` have user-controlled chart settings equivalents, the relevant plot information will only appear when all settings allow for it. Possible values: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Optional. The default is display.all.
REMARKS
Use plotarrow function in conjunction with 'overlay=true' indicator parameter!
SEE ALSO
plot
plotshape
plotchar
barcolor
bgcolor
plotbar

Plots ohlc bars on the chart.
plotbar(open, high, low, close, title, color, editable, show_last, display) → void
EXAMPLE

//@version=5
indicator("plotbar example", overlay=true)
plotbar(open, high, low, close, title='Title', color = open < close ? color.green : color.red)
ARGUMENTS
open (series int/float) Open series of data to be used as open values of bars. Required argument.
high (series int/float) High series of data to be used as high values of bars. Required argument.
low (series int/float) Low series of data to be used as low values of bars. Required argument.
close (series int/float) Close series of data to be used as close values of bars. Required argument.
title (const string) Title of the plotbar. Optional argument.
color (series color) Color of the ohlc bars. You can use constants like 'color=color.red' or 'color=#ff001a' as well as complex expressions like 'color = close >= open ? color.green : color.red'. Optional argument.
editable (const bool) If true then plotbar style will be editable in Format dialog. Default is true.
show_last (input int) If set, defines the number of bars (from the last bar back to the past) to plot on chart.
display (input plot_display) Controls where the plot's information is displayed. Display options support addition and subtraction, meaning that using `display.all - display.status_line` will display the plot's information everywhere except in the script's status line. `display.price_scale + display.status_line` will display the plot only in the price scale and status line. When `display` arguments such as `display.price_scale` have user-controlled chart settings equivalents, the relevant plot information will only appear when all settings allow for it. Possible values: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Optional. The default is display.all.
REMARKS
Even if one value of open, high, low or close equal NaN then bar no draw.
The maximal value of open, high, low or close will be set as 'high', and the minimal value will be set as 'low'.
SEE ALSO
plotcandle
plotcandle

Plots candles on the chart.
plotcandle(open, high, low, close, title, color, wickcolor, editable, show_last, bordercolor, display) → void
EXAMPLE

//@version=5
indicator("plotcandle example", overlay=true)
plotcandle(open, high, low, close, title='Title', color = open < close ? color.green : color.red, wickcolor=color.black)
ARGUMENTS
open (series int/float) Open series of data to be used as open values of candles. Required argument.
high (series int/float) High series of data to be used as high values of candles. Required argument.
low (series int/float) Low series of data to be used as low values of candles. Required argument.
close (series int/float) Close series of data to be used as close values of candles. Required argument.
title (const string) Title of the plotcandles. Optional argument.
color (series color) Color of the candles. You can use constants like 'color=color.red' or 'color=#ff001a' as well as complex expressions like 'color = close >= open ? color.green : color.red'. Optional argument.
wickcolor (series color) The color of the wick of candles. An optional argument.
editable (const bool) If true then plotcandle style will be editable in Format dialog. Default is true.
show_last (input int) If set, defines the number of candles (from the last bar back to the past) to plot on chart.
bordercolor (series color) The border color of candles. An optional argument.
display (input plot_display) Controls where the plot's information is displayed. Display options support addition and subtraction, meaning that using `display.all - display.status_line` will display the plot's information everywhere except in the script's status line. `display.price_scale + display.status_line` will display the plot only in the price scale and status line. When `display` arguments such as `display.price_scale` have user-controlled chart settings equivalents, the relevant plot information will only appear when all settings allow for it. Possible values: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Optional. The default is display.all.
REMARKS
Even if one value of open, high, low or close equal NaN then bar no draw.
The maximal value of open, high, low or close will be set as 'high', and the minimal value will be set as 'low'.
SEE ALSO
plotbar
plotchar

Plots visual shapes using any given one Unicode character on the chart.
plotchar(series, title, char, location, color, offset, text, textcolor, editable, size, show_last, display) → void
EXAMPLE

//@version=5
indicator("plotchar example", overlay=true)
data = close >= open
plotchar(data, char='❄')
ARGUMENTS
series (series bool) Series of data to be plotted as shapes. Series is treated as a series of boolean values for all location values except location.absolute. Required argument.
title (const string) Title of the plot.
char (input string) Character to use as a visual shape.
location (input string) Location of shapes on the chart. Possible values are: location.abovebar, location.belowbar, location.top, location.bottom, location.absolute. Default value is location.abovebar.
color (series color) Color of the shapes. You can use constants like 'color=color.red' or 'color=#ff001a' as well as complex expressions like 'color = close >= open ? color.green : color.red'. Optional argument.
offset (series int) Shifts shapes to the left or to the right on the given number of bars. Default is 0.
text (const string) Text to display with the shape. You can use multiline text, to separate lines use '\n' escape sequence. Example: 'line one\nline two'.
textcolor (series color) Color of the text. You can use constants like 'textcolor=color.red' or 'textcolor=#ff001a' as well as complex expressions like 'textcolor = close >= open ? color.green : color.red'. Optional argument.
editable (const bool) If true then plotchar style will be editable in Format dialog. Default is true.
show_last (input int) If set, defines the number of chars (from the last bar back to the past) to plot on chart.
size (const string) Size of characters on the chart. Possible values are: size.auto, size.tiny, size.small, size.normal, size.large, size.huge. Default is size.auto.
display (input plot_display) Controls where the plot's information is displayed. Display options support addition and subtraction, meaning that using `display.all - display.status_line` will display the plot's information everywhere except in the script's status line. `display.price_scale + display.status_line` will display the plot only in the price scale and status line. When `display` arguments such as `display.price_scale` have user-controlled chart settings equivalents, the relevant plot information will only appear when all settings allow for it. Possible values: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Optional. The default is display.all.
REMARKS
Use plotchar function in conjunction with 'overlay=true' indicator parameter!
SEE ALSO
plot
plotshape
plotarrow
barcolor
bgcolor
plotshape

Plots visual shapes on the chart.
plotshape(series, title, style, location, color, offset, text, textcolor, editable, size, show_last, display) → void
EXAMPLE

//@version=5
indicator("plotshape example 1", overlay=true)
data = close >= open
plotshape(data, style=shape.xcross)
ARGUMENTS
series (series bool) Series of data to be plotted as shapes. Series is treated as a series of boolean values for all location values except location.absolute. Required argument.
title (const string) Title of the plot.
style (input string) Type of plot. Possible values are: shape.xcross, shape.cross, shape.triangleup, shape.triangledown, shape.flag, shape.circle, shape.arrowup, shape.arrowdown, shape.labelup, shape.labeldown, shape.square, shape.diamond. Default value is shape.xcross.
location (input string) Location of shapes on the chart. Possible values are: location.abovebar, location.belowbar, location.top, location.bottom, location.absolute. Default value is location.abovebar.
color (series color) Color of the shapes. You can use constants like 'color=color.red' or 'color=#ff001a' as well as complex expressions like 'color = close >= open ? color.green : color.red'. Optional argument.
offset (series int) Shifts shapes to the left or to the right on the given number of bars. Default is 0.
text (const string) Text to display with the shape. You can use multiline text, to separate lines use '\n' escape sequence. Example: 'line one\nline two'.
textcolor (series color) Color of the text. You can use constants like 'textcolor=color.red' or 'textcolor=#ff001a' as well as complex expressions like 'textcolor = close >= open ? color.green : color.red'. Optional argument.
editable (const bool) If true then plotshape style will be editable in Format dialog. Default is true.
show_last (input int) If set, defines the number of shapes (from the last bar back to the past) to plot on chart.
size (const string) Size of shapes on the chart. Possible values are: size.auto, size.tiny, size.small, size.normal, size.large, size.huge. Default is size.auto.
display (input plot_display) Controls where the plot's information is displayed. Display options support addition and subtraction, meaning that using `display.all - display.status_line` will display the plot's information everywhere except in the script's status line. `display.price_scale + display.status_line` will display the plot only in the price scale and status line. When `display` arguments such as `display.price_scale` have user-controlled chart settings equivalents, the relevant plot information will only appear when all settings allow for it. Possible values: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Optional. The default is display.all.
REMARKS
Use plotshape function in conjunction with 'overlay=true' indicator parameter!
SEE ALSO
plot
plotchar
plotarrow
barcolor
bgcolor
request.currency_rate

Provides a daily rate that can be used to convert a value expressed in the `from` currency to another in the `to` currency.
request.currency_rate(from, to, ignore_invalid_currency) → series float
EXAMPLE

//@version=5
indicator("Close in British Pounds")
rate = request.currency_rate(syminfo.currency, "GBP")
plot(close * rate)
ARGUMENTS
from (simple string) The currency in which the value to be converted is expressed. Possible values: a three-letter string with the currency code in the ISO 4217 format (e.g. "USD"), or one of the built-in variables that return currency codes, like syminfo.currency or currency.USD.
to (simple string) The currency in which the value is to be converted. Possible values: a three-letter string with the currency code in the ISO 4217 format (e.g. "USD"), or one of the built-in variables that return currency codes, like syminfo.currency or currency.USD.
ignore_invalid_currency (simple bool) Determines the behavior of the function if a conversion rate between the two currencies cannot be calculated: if false, the script will halt and return a runtime error; if true, the function will return na and execution will continue. Optional. The default is false.
REMARKS
If `from` and `to` arguments are equal, function returns 1. Please note that using this variable/function can cause indicator repainting.
request.dividends

Requests dividends data for the specified symbol.
request.dividends(ticker, field, gaps, lookahead, ignore_invalid_symbol, currency) → series float
EXAMPLE

//@version=5
indicator("request.dividends")
s1 = request.dividends("NASDAQ:BELFA")
plot(s1)
s2 = request.dividends("NASDAQ:BELFA", dividends.net, gaps=barmerge.gaps_on, lookahead=barmerge.lookahead_on)
plot(s2)
RETURNS
Requested series, or n/a if there is no dividends data for the specified symbol.
ARGUMENTS
ticker (simple string) Symbol. Note that the symbol should be passed with a prefix. For example: "NASDAQ:AAPL" instead of "AAPL". Using syminfo.ticker will cause an error. Use syminfo.tickerid instead.
field (simple string) Input string. Possible values include: dividends.net, dividends.gross. Default value is dividends.gross.
gaps (simple barmerge_gaps) Merge strategy for the requested data (requested data automatically merges with the main series OHLC data). Possible values: barmerge.gaps_on, barmerge.gaps_off. barmerge.gaps_on - requested data is merged with possible gaps (na values). barmerge.gaps_off - requested data is merged continuously without gaps, all the gaps are filled with the previous nearest existing values. Default value is barmerge.gaps_off.
lookahead (simple barmerge_lookahead) Merge strategy for the requested data position. Possible values: barmerge.lookahead_on, barmerge.lookahead_off. Default value is barmerge.lookahead_off starting from version 3. Note that behavour is the same on real-time, and differs only on history.
ignore_invalid_symbol (input bool) An optional parameter. Determines the behavior of the function if the specified symbol is not found: if false, the script will halt and return a runtime error; if true, the function will return na and execution will continue. The default value is false.
currency (simple string) Currency into which the symbol's currency-related dividends values (e.g. dividends.gross) are to be converted. The conversion rates used are based on the FX_IDC pairs' daily rates of the previous day (relative to the bar where the calculation is done). Optional. The default is syminfo.currency. Possible values: a three-letter string with the currency code in the ISO 4217 format (e.g. "USD") or one of the constants in the currency.* namespace, e.g. currency.USD.
SEE ALSO
request.earnings
request.splits
request.security
syminfo.tickerid
request.earnings

Requests earnings data for the specified symbol.
request.earnings(ticker, field, gaps, lookahead, ignore_invalid_symbol, currency) → series float
EXAMPLE

//@version=5
indicator("request.earnings")
s1 = request.earnings("NASDAQ:BELFA")
plot(s1)
s2 = request.earnings("NASDAQ:BELFA", earnings.actual, gaps=barmerge.gaps_on, lookahead=barmerge.lookahead_on)
plot(s2)
RETURNS
Requested series, or n/a if there is no earnings data for the specified symbol.
ARGUMENTS
ticker (simple string) Symbol. Note that the symbol should be passed with a prefix. For example: "NASDAQ:AAPL" instead of "AAPL". Using syminfo.ticker will cause an error. Use syminfo.tickerid instead.
field (simple string) Input string. Possible values include: earnings.actual, earnings.estimate, earnings.standardized. Default value is earnings.actual.
gaps (simple barmerge_gaps) Merge strategy for the requested data (requested data automatically merges with the main series OHLC data). Possible values: barmerge.gaps_on, barmerge.gaps_off. barmerge.gaps_on - requested data is merged with possible gaps (na values). barmerge.gaps_off - requested data is merged continuously without gaps, all the gaps are filled with the previous nearest existing values. Default value is barmerge.gaps_off.
lookahead (simple barmerge_lookahead) Merge strategy for the requested data position. Possible values: barmerge.lookahead_on, barmerge.lookahead_off. Default value is barmerge.lookahead_off starting from version 3. Note that behavour is the same on real-time, and differs only on history.
ignore_invalid_symbol (input bool) An optional parameter. Determines the behavior of the function if the specified symbol is not found: if false, the script will halt and return a runtime error; if true, the function will return na and execution will continue. The default value is false.
currency (simple string) Currency into which the symbol's currency-related earnings values (e.g. earnings.actual) are to be converted. The conversion rates used are based on the FX_IDC pairs' daily rates of the previous day (relative to the bar where the calculation is done). Optional. The default is syminfo.currency. Possible values: a three-letter string with the currency code in the ISO 4217 format (e.g. "USD") or one of the constants in the currency.* namespace, e.g. currency.USD.
SEE ALSO
request.dividends
request.splits
request.security
syminfo.tickerid
request.economic

Requests economic data for a symbol. Economic data includes information such as the state of a country's economy (GDP, inflation rate, etc.) or of a particular industry (steel production, ICU beds, etc.).
request.economic(country_code, field, gaps, ignore_invalid_symbol) → series float
EXAMPLE

//@version=5
indicator("US GDP")
e = request.economic("US", "GDP")
plot(e)
RETURNS
Requested series.
ARGUMENTS
country_code (simple string) The code of the country (e.g. "US") or the region (e.g. "EU") for which the economic data is requested. The Help Center article lists the countries and their codes. The countries for which information is available vary with metrics. The Help Center article for each metric lists the countries for which the metric is available.
field (simple string) The code of the requested economic metric (e.g., "GDP"). The Help Center article lists the metrics and their codes.
gaps (simple barmerge_gaps) Specifies how the returned values are merged on chart bars. Possible values: barmerge.gaps_off, barmerge.gaps_on. With barmerge.gaps_on, a value only appears on the current chart bar when it first becomes available from the function's context, otherwise na is returned (thus a "gap" occurs). With barmerge.gaps_off, what would otherwise be gaps are filled with the latest known value returned, avoiding na values. Optional. The default is barmerge.gaps_off.
ignore_invalid_symbol (input bool) Determines the behavior of the function if the specified symbol is not found: if false, the script will halt and return a runtime error; if true, the function will return na and execution will continue. Optional. The default is false.
REMARKS
Economic data can also be accessed from charts, just like a regular symbol. Use "ECONOMIC" as the exchange name and `{country_code}{field}` as the ticker. The name of US GDP data is thus "ECONOMIC:USGDP".
SEE ALSO
request.financial
request.quandl
request.financial

Requests financial series for symbol.
request.financial(symbol, financial_id, period, gaps, ignore_invalid_symbol, currency) → series float
EXAMPLE

//@version=5
indicator("request.financial")
f = request.financial("NASDAQ:MSFT", "ACCOUNTS_PAYABLE", "FY")
plot(f)
RETURNS
Requested series.
ARGUMENTS
symbol (simple string) Symbol. Note that the symbol should be passed with a prefix. For example: "NASDAQ:AAPL" instead of "AAPL".
financial_id (simple string) Financial identifier. You can find the list of available ids via our Help Center.
period (simple string) Reporting period. Possible values are "TTM", "FY", "FQ".
gaps (simple barmerge_gaps) Merge strategy for the requested data (requested data automatically merges with the main series: OHLC data). Possible values include: barmerge.gaps_on, barmerge.gaps_off. barmerge.gaps_on - requested data is merged with possible gaps (na values). barmerge.gaps_off - requested data is merged continuously without gaps, all the gaps are filled with the previous, nearest existing values. Default value is barmerge.gaps_off.
ignore_invalid_symbol (input bool) An optional parameter. Determines the behavior of the function if the specified symbol is not found: if false, the script will halt and return a runtime error; if true, the function will return na and execution will continue. The default value is false.
currency (simple string) Currency into which the symbol's financial metrics (e.g. Net Income) are to be converted. The conversion rates used are based on the FX_IDC pairs' daily rates of the previous day (relative to the bar where the calculation is done). Optional. The default is syminfo.currency. Possible values: a three-letter string with the currency code in the ISO 4217 format (e.g. "USD") or one of the constants in the currency.* namespace, e.g. currency.USD.
SEE ALSO
request.security
syminfo.tickerid
request.quandl

Requests Nasdaq Data Link (formerly Quandl) data for a symbol.
request.quandl(ticker, gaps, index, ignore_invalid_symbol) → series float
EXAMPLE

//@version=5
indicator("request.quandl")
f = request.quandl("CFTC/SB_FO_ALL", barmerge.gaps_off, 0)
plot(f)
RETURNS
Requested series.
ARGUMENTS
ticker (simple string) Symbol. Note that the name of a time series and Quandl data feed should be divided by a forward slash. For example: "CFTC/SB_FO_ALL".
gaps (simple barmerge_gaps) Merge strategy for the requested data (requested data automatically merges with the main series: OHLC data). Possible values include: barmerge.gaps_on, barmerge.gaps_off. barmerge.gaps_on - requested data is merged with possible gaps (na values). barmerge.gaps_off - requested data is merged continuously without gaps, all the gaps are filled with the previous, nearest existing values. Default value is barmerge.gaps_off.
index (simple int) A Quandl time-series column index.
ignore_invalid_symbol (input bool) An optional parameter. Determines the behavior of the function if the specified symbol is not found: if false, the script will halt and return a runtime error; if true, the function will return na and execution will continue. The default value is false.
REMARKS
You can learn more about how to find ticker and index values in our Help Center.
SEE ALSO
request.security
syminfo.tickerid
request.security

Requests data from another symbol and/or timeframe.
request.security(symbol, timeframe, expression, gaps, lookahead, ignore_invalid_symbol, currency) → series <type>
EXAMPLE

//@version=5
indicator("Simple `request.security()` calls")
// Returns 1D close of the current symbol.
dailyClose = request.security(syminfo.tickerid, "1D", close)
plot(dailyClose)

// Returns the close of "AAPL" from the same timeframe as currently open on the chart.
aaplClose = request.security("AAPL", timeframe.period, close)
plot(aaplClose)
EXAMPLE

//@version=5
indicator("Advanced `request.security()` calls")
// This calculates a 10-period moving average on the active chart.
sma = ta.sma(close, 10)
// This sends the `sma` calculation for execution in the context of the "AAPL" symbol at a "240" (4 hours) timeframe.
aaplSma = request.security("AAPL", "240", sma)
plot(aaplSma)

// To avoid differences on historical and realtime bars, you can use this technique, which only returns a value from the higher timeframe on the bar after it completes:
indexHighTF = barstate.isrealtime ? 1 : 0
indexCurrTF = barstate.isrealtime ? 0 : 1
nonRepaintingClose = request.security(syminfo.tickerid, "1D", close[indexHighTF])[indexCurrTF]
plot(nonRepaintingClose, "Non-repainting close")

// Returns the 1H close of "AAPL", extended session included. The value is dividend-adjusted.
extendedTicker = ticker.modify("NASDAQ:AAPL", session = session.extended, adjustment = adjustment.dividends)
aaplExtAdj = request.security(extendedTicker, "60", close)
plot(aaplExtAdj)

// Returns the result of a user-defined function.
// The `max` variable is mutable, but we can pass it to `request.security()` because it is wrapped in a function.
allTimeHigh(source) =>
    var max = source
    max := math.max(max, source)
allTimeHigh1D = request.security(syminfo.tickerid, "1D", allTimeHigh(high))

// By using a tuple `expression`, we obtain several values with only one `request.security()` call.
[open1D, high1D, low1D, close1D, ema1D] = request.security(syminfo.tickerid, "1D", [open, high, low, close, ta.ema(close, 10)])
plotcandle(open1D, high1D, low1D, close1D)
plot(ema1D)

// Returns an array containing the OHLC values of the chart's symbol from the 1D timeframe.
ohlcArray = request.security(syminfo.tickerid, "1D", array.from(open, high, low, close))
plotcandle(array.get(ohlcArray, 0), array.get(ohlcArray, 1), array.get(ohlcArray, 2), array.get(ohlcArray, 3))
RETURNS
A result determined by `expression`.
ARGUMENTS
symbol (simple string) Symbol to request the data from. Use syminfo.tickerid to request data from the chart's symbol. To request data with additional parameters (extended sessions, dividend adjustments, or a non-standard chart type like Heikin Ashi or Renko), a custom ticker identifier must first be created using functions in the `ticker.*` namespace.
timeframe (simple string) Timeframe of the requested data. To use the chart's timeframe, use an empty string or the timeframe.period variable. Valid timeframe strings are documented in the User Manual's Timeframes page.
expression (variable, function, array or matrix of series int/float/bool/string/color, or a tuple of these) An expression to be calculated and returned from the request.security call's context. It can be a built-in variable like close, an expression such as `ta.sma(close, 100)`, a non-mutable user-defined variable previously calculated in the script, a function call that does not use PineScript™ drawings, an array, a matrix, or a tuple. Mutable variables are not allowed, unless they are enclosed in the body of a function used in the expression.
gaps (simple barmerge_gaps) Specifies how the returned values are merged on chart bars. Possible values: barmerge.gaps_on, barmerge.gaps_off. With barmerge.gaps_on a value only appears on the current chart bar when it first becomes available from the function's context, otherwise na is returned (thus a "gap" occurs). With barmerge.gaps_off what would otherwise be gaps are filled with the latest known value returned, avoiding na values. Optional. The default is barmerge.gaps_off.
lookahead (simple barmerge_lookahead) On historical bars only, returns data from the timeframe before it elapses. Possible values: barmerge.lookahead_on, barmerge.lookahead_off. Has no effect on realtime values. Optional. The default is barmerge.lookahead_off starting from Pine Script™ v3. The default is barmerge.lookahead_on in v1 and v2. WARNING: Using barmerge.lookahead_on at timeframes higher than the chart's without offsetting the `expression` argument like in `close[1]` will introduce future leak in scripts, as the function will then return the `close` price before it is actually known in the current context. As is explained in the User Manual's page on Repainting this will produce misleading results.
ignore_invalid_symbol (input bool) Determines the behavior of the function if the specified symbol is not found: if false, the script will halt and throw a runtime error; if true, the function will return na and execution will continue. Optional. The default is false.
currency (simple string) Currency into which values expressed in currency units (open, high, low, close, etc.) or expressions using such values are to be converted. The conversion rates used are based on the FX_IDC pairs' daily rates of the previous day (relative to the bar where the calculation is done). Possible values: a three-letter string with the currency code in the ISO 4217 format (e.g. "USD") or one of the constants in the currency.* namespace, e.g. currency.USD. Note that literal values such as `200` are not converted. Optional. The default is syminfo.currency.
REMARKS
Pine Script™ code using this function may calculate differently on historical and realtime bars, leading to repainting.
A single script can have no more than 40 calls to `request.*()` functions.
SEE ALSO
syminfo.ticker
syminfo.tickerid
timeframe.period
ticker.new
ticker.modify
request.security_lower_tf
request.dividends
request.earnings
request.splits
request.financial
request.quandl
request.security_lower_tf

Requests data from a specified symbol from a lower timeframe than the chart's. The function returns an array containing one element for each closed lower timeframe intrabar inside the current chart's bar. On a 5-minute chart using a `timeframe` argument of "1", the size of the array will usually be 5, with each array element representing the value of `expression` on a 1-minute intrabar, ordered sequentially in time.
request.security_lower_tf(symbol, timeframe, expression, ignore_invalid_symbol, currency) → array<type>
EXAMPLE

//@version=5
indicator("`request.security_lower_tf()` Example", overlay = true)

// If the current chart timeframe is set to 120 minutes, then the `arrayClose` array will contain two 'close' values from the 60 minute timeframe for each bar.
arrClose = request.security_lower_tf(syminfo.tickerid, "60", close)

if bar_index == last_bar_index - 1
    label.new(bar_index, high, str.tostring(arrClose))
RETURNS
An array of a type determined by `expression`, or a tuple of these.
ARGUMENTS
symbol (simple string) Symbol to request the data from. Use syminfo.tickerid to request data from the chart's symbol. To request data with additional parameters (extended sessions, dividend adjustments, or a non-standard chart type like Heikin Ashi or Renko), a custom ticker identifier must first be created using functions in the `ticker.*` namespace.
timeframe (simple string) Timeframe of the requested data. To use the chart's timeframe, use an empty string or the timeframe.period variable. Valid timeframe strings are documented in the User Manual's Timeframes page.
expression (variable or function of series int/float/bool/string/color, or a tuple of these) An expression to be calculated and returned from the function call's context. It can be a built-in variable like close, an expression such as `ta.sma(close, 100)`, a non-mutable user-defined variable previously calculated in the script, a function call that does not use PineScript™ drawings, arrays or matrices, or a tuple. Mutable variables are not allowed, unless they are enclosed in the body of a function used in the expression.
ignore_invalid_symbol (const bool) Determines the behavior of the function if the specified symbol is not found: if false, the script will halt and throw a runtime error; if true, the function will return na and execution will continue. Optional. The default is false.
currency (simple string) Currency into which values expressed in currency units (open, high, low, close, etc.) or expressions using such values are to be converted. The conversion rates used are based on the FX_IDC pairs' daily rates of the previous day (relative to the bar where the calculation is done). Possible values: a three-letter string with the currency code in the ISO 4217 format (e.g. "USD") or one of the constants in the currency.* namespace, e.g. currency.USD. Note that literal values such as `200` are not converted. Optional. The default is syminfo.currency.
REMARKS
Pine Script™ code using this function may calculate differently on historical and real-time bars, leading to repainting.
Please note that spreads (e.g., “AAPL+MSFT*TSLA”) will not always return reliable data with this function.
A single script can have no more than 40 calls to `request.*()` functions.
A maximum of 100,000 lower timeframe bars can be accessed by this function. The number of chart bars for which lower timeframe data is available will thus vary with the requested lower timeframe.
SEE ALSO
request.security
syminfo.ticker
syminfo.tickerid
timeframe.period
ticker.new
request.dividends
request.earnings
request.splits
request.financial
request.quandl
request.splits

Requests splits data for the specified symbol.
request.splits(ticker, field, gaps, lookahead, ignore_invalid_symbol) → series float
EXAMPLE

//@version=5
indicator("request.splits")
s1 = request.splits("NASDAQ:BELFA", splits.denominator)
plot(s1)
s2 = request.splits("NASDAQ:BELFA", splits.denominator, gaps=barmerge.gaps_on, lookahead=barmerge.lookahead_on)
plot(s2)
RETURNS
Requested series, or n/a if there is no splits data for the specified symbol.
ARGUMENTS
ticker (simple string) Symbol. Note that the symbol should be passed with a prefix. For example: "NASDAQ:AAPL" instead of "AAPL". Using syminfo.ticker will cause an error. Use syminfo.tickerid instead.
field (simple string) Input string. Possible values include: splits.denominator, splits.numerator.
gaps (simple barmerge_gaps) Merge strategy for the requested data (requested data automatically merges with the main series OHLC data). Possible values: barmerge.gaps_on, barmerge.gaps_off. barmerge.gaps_on - requested data is merged with possible gaps (na values). barmerge.gaps_off - requested data is merged continuously without gaps, all the gaps are filled with the previous nearest existing values. Default value is barmerge.gaps_off.
lookahead (simple barmerge_lookahead) Merge strategy for the requested data position. Possible values: barmerge.lookahead_on, barmerge.lookahead_off. Default value is barmerge.lookahead_off starting from version 3. Note that behavour is the same on real-time, and differs only on history.
ignore_invalid_symbol (input bool) An optional parameter. Determines the behavior of the function if the specified symbol is not found: if false, the script will halt and return a runtime error; if true, the function will return na and execution will continue. The default value is false.
SEE ALSO
request.earnings
request.dividends
request.security
syminfo.tickerid
runtime.error

When called, causes a runtime error with the error message specified in the `message` argument.
runtime.error(message) → void
ARGUMENTS
message (series string) Error message.
second

second(time) → series int
second(time, timezone) → series int
RETURNS
Second (in exchange timezone) for provided UNIX time.
ARGUMENTS
time (series int) UNIX time in milliseconds.
timezone (series string) Allows adjusting the returned value to a time zone specified in either UTC/GMT notation (e.g., "UTC-5", "GMT+0530") or as an IANA time zone database name (e.g., "America/New_York"). Optional. The default is syminfo.timezone.
REMARKS
UNIX time is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
SEE ALSO
second
time
year
month
dayofmonth
dayofweek
hour
minute
str.contains

Returns true if the `source` string contains the `str` substring, false otherwise.
str.contains(source, str) → const bool
str.contains(source, str) → simple bool
str.contains(source, str) → series bool
EXAMPLE

//@version=5
indicator("str.contains")
// If the current chart is a continuous futures chart, e.g “BTC1!”, then the function will return true, false otherwise.
var isFutures = str.contains(syminfo.tickerid, "!")
plot(isFutures ? 1 : 0)
RETURNS
True if the `str` was found in the `source` string, false otherwise.
ARGUMENTS
source (series string) Source string.
str (series string) The substring to search for.
SEE ALSO
str.pos
str.endswith

Returns true if the `source` string ends with the substring specified in `str`, false otherwise.
str.endswith(source, str) → const bool
str.endswith(source, str) → simple bool
str.endswith(source, str) → series bool
RETURNS
True if the `source` string ends with the substring specified in `str`, false otherwise.
ARGUMENTS
source (series string) Source string.
str (series string) The substring to search for.
SEE ALSO
str.startswith
str.format

Converts the formatting string and value(s) into a formatted string. The formatting string can contain literal text and one placeholder in curly braces {} for each value to be formatted. Each placeholder consists of the index of the required argument (beginning at 0) that will replace it, and an optional format specifier. The index represents the position of that argument in the str.format argument list.
str.format(formatString, arg0, arg1, ...) → simple string
str.format(formatString, arg0, arg1, ...) → series string
EXAMPLE

//@version=5
indicator("str.format", overlay=true)
// The format specifier inside the curly braces accepts certain modifiers:
// - Specify the number of decimals to display:
s1 = str.format("{0,number,#.#}", 1.34) // returns: 1.3
label.new(bar_index, close, text=s1)
// - Round a float value to an integer:
s2 = str.format("{0,number,integer}", 1.34) // returns: 1
label.new(bar_index - 1, close, text=s2)
// - Display a number in currency:
s3 = str.format("{0,number,currency}", 1.34) // returns: $1.34
label.new(bar_index - 2, close, text=s3)
// - Display a number as a percentage:
s4 = str.format("{0,number,percent}", 0.5) // returns: 50%
label.new(bar_index - 3, close, text=s4)
// EXAMPLES WITH SEVERAL ARGUMENTS
// returns: Number 1 is not equal to 4
s5 = str.format("Number {0} is not {1} to {2}", 1, "equal", 4)
label.new(bar_index - 4, close, text=s5)
// returns: 1.34 != 1.3
s6 = str.format("{0} != {0, number, #.#}", 1.34)
label.new(bar_index - 5, close, text=s6)
// returns: 1 is equal to 1, but 2 is equal to 2
s7 = str.format("{0, number, integer} is equal to 1, but {1, number, integer} is equal to 2", 1.34, 1.52)
label.new(bar_index - 6, close, text=s7)
// returns: The cash turnover amounted to $1,340,000.00
s8 = str.format("The cash turnover amounted to {0, number, currency}", 1340000)
label.new(bar_index - 7, close, text=s8)
// returns: Expected return is 10% - 20%
s9 = str.format("Expected return is {0, number, percent} - {1, number, percent}", 0.1, 0.2)
label.new(bar_index - 8, close, text=s9)
RETURNS
The formatted string.
ARGUMENTS
formatString (series string) Format string.
arg0, arg1, ... (series int/float/bool/string/na/int[]/float[]/bool[]/string[]) Values to format.
REMARKS
Any curly braces within an unquoted pattern must be balanced. For example, "ab {0} de" and "ab '}' de" are valid patterns, but "ab {0'}' de", "ab } de" and "''{''" are not.
str.format_time

Converts the `time` timestamp into a string formatted according to `format` and `timezone`.
str.format_time(time, format, timezone) → series string
str.format_time(time, timezone) → series string
EXAMPLE

//@version=5
indicator("str.format_time")
if timeframe.change("1D")
    formattedTime = str.format_time(time, "yyyy-MM-dd HH:mm", syminfo.timezone)
    label.new(bar_index, high, formattedTime)
RETURNS
The formatted string.
ARGUMENTS
time (series int) UNIX time, in milliseconds.
format (series string) A format string specifying the date/time representation of the `time` in the returned string. All letters used in the string, except those escaped by single quotation marks `'`, are considered formatting tokens and will be used as a formatting instruction. Refer to the Remarks section for a list of the most useful tokens. Optional. The default is "yyyy-MM-dd'T'HH:mm:ssZ", which represents the ISO 8601 standard.
timezone (series string) Allows adjusting the returned value to a time zone specified in either UTC/GMT notation (e.g., "UTC-5", "GMT+0530") or as an IANA time zone database name (e.g., "America/New_York"). Optional. The default is syminfo.timezone.
REMARKS
The `M`, `d`, `h`, `H`, `m` and `s` tokens can all be doubled to generate leading zeroes. For example, the month of January will display as `1` with `M`, or `01` with `MM`.
The most frequently used formatting tokens are:
y - Year. Use `yy` to output the last two digits of the year or `yyyy` to output all four. Year 2000 will be `00` with `yy` or `2000` with `yyyy`.
M - Month. Not to be confused with lowercase `m`, which stands for minute.
d - Day of the month.
a - AM/PM postfix.
h - Hour in the 12-hour format. The last hour of the day will be `11` in this format.
H - Hour in the 24-hour format. The last hour of the day will be `23` in this format.
m - Minute.
s - Second.
S - Fractions of a second.
Z - Timezone, the HHmm offset from UTC, preceded by either `+` or `-`.
str.length

Returns an integer corresponding to the amount of chars in that string.
str.length(string) → const int
str.length(string) → simple int
str.length(string) → series int
RETURNS
The number of chars in source string.
ARGUMENTS
string (series string) Source string.
str.lower

Returns a new string with all letters converted to lowercase.
str.lower(source) → const string
str.lower(source) → simple string
str.lower(source) → series string
RETURNS
A new string with all letters converted to lowercase.
ARGUMENTS
source (series string) String to be converted.
SEE ALSO
str.upper
str.match

Returns the new substring of the `source` string if it matches a `regex` regular expression, an empty string otherwise.
str.match(source, regex) → simple string
str.match(source, regex) → series string
EXAMPLE

//@version=5
indicator("str.match")

s = input.string("It's time to sell some NASDAQ:AAPL!")

// finding first substring that matches regular expression "[\w]+:[\w]+"
var string tickerid = str.match(s, "[\\w]+:[\\w]+")

if barstate.islastconfirmedhistory
    label.new(bar_index, high, text = tickerid) // "NASDAQ:AAPL"
RETURNS
The new substring of the `source` string if it matches a `regex` regular expression, an empty string otherwise.
ARGUMENTS
source (series string) Source string.
regex (series string) The regular expression to which this string is to be matched.
REMARKS
Function returns first occurrence of the regular expression in the `source` string.
The backslash "\" symbol in the`regex` string needs to be escaped with additional backslash, e.g. "\\d" stands for regular expression "\d".
SEE ALSO
str.contains
str.pos

Returns the position of the first occurrence of the `str` string in the `source` string, 'na' otherwise.
str.pos(source, str) → const int
str.pos(source, str) → simple int
str.pos(source, str) → series int
RETURNS
Position of the `str` string in the `source` string.
ARGUMENTS
source (series string) Source string.
str (series string) The substring to search for.
REMARKS
Strings indexing starts at 0.
SEE ALSO
str.contains
str.replace

Returns a new string with the Nth occurrence of the `target` string replaced by the `replacement` string, where N is specified in `occurrence`.
str.replace(source, target, replacement, occurrence) → const string
str.replace(source, target, replacement, occurrence) → simple string
str.replace(source, target, replacement, occurrence) → series string
EXAMPLE

//@version=5
indicator("str.replace")
var source = "FTX:BTCUSD / FTX:BTCEUR"

// Replace first occurrence of "FTX" with "BINANCE" replacement string
var newSource = str.replace(source, "FTX",  "BINANCE", 0)

if barstate.islastconfirmedhistory
    // Display "BINANCE:BTCUSD / FTX:BTCEUR"
    label.new(bar_index, high, text = newSource)
RETURNS
Processed string.
ARGUMENTS
source (series string) Source string.
target (series string) String to be replaced.
replacement (series string) String to be inserted instead of the target string.
occurrence (series int) N-th occurrence of the target string to replace. Indexing starts at 0 for the first match. Optional. Default value is 0.
SEE ALSO
str.replace_all
str.replace_all

Replaces each occurrence of the target string in the source string with the replacement string.
str.replace_all(source, target, replacement) → simple string
str.replace_all(source, target, replacement) → series string
RETURNS
Processed string.
ARGUMENTS
source (series string) Source string.
target (series string) String to be replaced.
replacement (series string) String to be substituted for each occurrence of target string.
str.split

Divides a string into an array of substrings and returns its array id.
str.split(string, separator) → string[]
RETURNS
The id of an array of strings.
ARGUMENTS
string (series string) Source string.
separator (series string) The string separating each substring.
str.startswith

Returns true if the `source` string starts with the substring specified in `str`, false otherwise.
str.startswith(source, str) → const bool
str.startswith(source, str) → simple bool
str.startswith(source, str) → series bool
RETURNS
True if the `source` string starts with the substring specified in `str`, false otherwise.
ARGUMENTS
source (series string) Source string.
str (series string) The substring to search for.
SEE ALSO
str.endswith
str.substring

Returns a new string that is a substring of the `source` string. The substring begins with the character at the index specified by `begin_pos` and extends to 'end_pos - 1' of the `source` string.
str.substring(source, begin_pos) → const string
str.substring(source, begin_pos) → simple string
str.substring(source, begin_pos) → series string
str.substring(source, begin_pos, end_pos) → const string
str.substring(source, begin_pos, end_pos) → simple string
str.substring(source, begin_pos, end_pos) → series string
EXAMPLE

//@version=5
indicator("str.substring", overlay = true)
sym= input.symbol("NASDAQ:AAPL")
pos = str.pos(sym, ":")  // Get position of ":" character
tkr= str.substring(sym, pos+1) // "AAPL"
if barstate.islastconfirmedhistory
    label.new(bar_index, high, text = tkr)
RETURNS
The substring extracted from the source string.
ARGUMENTS
source (series string) Source string from which to extract the substring.
begin_pos (series int) The beginning position of the extracted substring. It is inclusive (the extracted substring includes the character at that position).
end_pos (series int) The ending position. It is exclusive (the extracted string does NOT include that position's character). Optional. The default is the length of the `source` string.
REMARKS
Strings indexing starts from 0. If `begin_pos` is equal to `end_pos`, the function returns an empty string.
SEE ALSO
str.contains
str.tonumber

Converts a value represented in `string` to its "float" equivalent.
str.tonumber(string) → series float
str.tonumber(string) → const float
str.tonumber(string) → input float
str.tonumber(string) → simple float
RETURNS
A "float" equivalent of the value in `string`. If the value is not a properly formed integer or floating point value, the function returns na.
ARGUMENTS
string (series string) String containing the representation of an integer or floating point value.
str.tostring

str.tostring(value) → series string
str.tostring(value, format) → series string
str.tostring(value) → simple string
str.tostring(value, format) → simple string
RETURNS
The string representation of the `value` argument.
If the `value` argument is a string, it is returned as is.
When the `value` is na, the function returns the string "NaN".
ARGUMENTS
value (series int/float/bool/string/matrix<float>/matrix<int>/matrix<string>/matrix<bool>/int[]/float[]/bool[]/string[]) Value or array ID whose elements are converted to a string.
format (series string) Format string. Accepts these format.* constants: format.mintick, format.percent, format.volume. Optional. The default value is '#.##########'.
REMARKS
The formatting of float values will also round those values when necessary, e.g. str.tostring(3.99, '#') will return "4".
To display trailing zeros, use '0' instead of '#'. For example, '#.000'.
When using format.mintick, the value will be rounded to the nearest number that can be divided by syminfo.mintick without the remainder. The string is returned with trailing zeroes.
If the x argument is a string, the same string value will be returned.
Bool type arguments return "true" or "false".
When x is na, the function returns "NaN".
str.upper

Returns a new string with all letters converted to uppercase.
str.upper(source) → const string
str.upper(source) → simple string
str.upper(source) → series string
RETURNS
A new string with all letters converted to uppercase.
ARGUMENTS
source (series string) String to be converted.
SEE ALSO
str.lower
strategy

This declaration statement designates the script as a strategy and sets a number of strategy-related properties.
strategy(title, shorttitle, overlay, format, precision, scale, pyramiding, calc_on_order_fills, calc_on_every_tick, max_bars_back, backtest_fill_limits_assumption, default_qty_type, default_qty_value, initial_capital, currency, slippage, commission_type, commission_value, process_orders_on_close, close_entries_rule, margin_long, margin_short, explicit_plot_zorder, max_lines_count, max_labels_count, max_boxes_count, risk_free_rate, use_bar_magnifier) → void
EXAMPLE

//@version=5
strategy("My strategy", overlay = true, margin_long = 100, margin_short = 100)

// Enter long by market if current open is greater than previous high.
if open > high[1]
    strategy.entry("Long", strategy.long, 1)
// Generate a full exit bracket (profit 10 points, loss 5 points per contract) from the entry named "Long".
strategy.exit("Exit", "Long", profit = 10, loss = 5)
ARGUMENTS
title (const string) The title of the script. It is displayed on the chart when no `shorttitle` argument is used, and becomes the publication's default title when publishing the script.
shorttitle (const string) The script's display name on charts. If specified, it will replace the `title` argument in most chart-related windows. Optional. The default is the argument used for `title`.
overlay (const bool) If true, the strategy will be displayed over the chart. If false, it will be added in a separate pane. Strategy-specific labels that display entries and exits will be displayed over the main chart regardless of this setting. Optional. The default is false.
format (const string) Specifies the formatting of the script's displayed values. Possible values: format.inherit, format.price, format.volume, format.percent. Optional. The default is format.inherit.
precision (const int) Specifies the number of digits after the floating point of the script's displayed values. Must be a non-negative integer no greater than 16. If `format` is set to format.inherit and `precision` is specified, the format will instead be set to format.price. Optional. The default is inherited from the precision of the chart's symbol.
scale (const scale_type) The price scale used. Possible values: scale.right, scale.left, scale.none. The scale.none value can only be applied in combination with `overlay = true`. Optional. By default, the script uses the same scale as the chart.
pyramiding (const int) The maximum number of entries allowed in the same direction. If the value is 0, only one entry order in the same direction can be opened, and additional entry orders are rejected. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is 0.
calc_on_order_fills (const bool) Specifies whether the strategy should be recalculated after an order is filled. If true, the strategy recalculates after an order is filled, as opposed to recalculating only when the bar closes. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is false.
calc_on_every_tick (const bool) Specifies whether the strategy should be recalculated on each realtime tick. If true, when the strategy is running on a realtime bar, it will recalculate on each chart update. If false, the strategy only calculates when the realtime bar closes. The argument used does not affect strategy calculation on historical data. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is false.
max_bars_back (const int) The length of the historical buffer the script keeps for every variable and function, which determines how many past values can be referenced using the `[]` history-referencing operator. The required buffer size is automatically detected by the Pine Script™ runtime. Using this parameter is only necessary when a runtime error occurs because automatic detection fails. More information on the underlying mechanics of the historical buffer can be found in our Help Center. Optional. The default is 0.
backtest_fill_limits_assumption (const int) Limit order execution threshold in ticks. When it is used, limit orders are only filled if the market price exceeds the order's limit level by the specified number of ticks. Optional. The default is 0.
default_qty_type (const string) Specifies the units used for `default_qty_value`. Possible values are: strategy.fixed for contracts/shares/lots, strategy.cash for currency amounts, or strategy.percent_of_equity for a percentage of available equity. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is strategy.fixed.
default_qty_value (const int/float) The default quantity to trade, in units determined by the argument used with the `default_qty_type` parameter. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is 1.
initial_capital (const int/float) The amount of funds initially available for the strategy to trade, in units of `currency`. Optional. The default is 1000000.
currency (const string) Currency used by the strategy in currency-related calculations. Market positions are still opened by converting `currency` into the chart symbol's currency. The conversion rates used are based on the FX_IDC pairs' daily rates of the previous day (relative to the bar where the calculation is done). This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is currency.NONE, in which case the chart's currency is used. Possible values: one of the constants in the `currency.*` namespace, e.g. currency.USD.
slippage (const int) Slippage expressed in ticks. This value is added to or subtracted from the fill price of market/stop orders to make the fill price less favorable for the strategy. E.g., if syminfo.mintick is 0.01 and `slippage` is set to 5, a long market order will enter at 5 * 0.01 = 0.05 points above the actual price. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is 0.
commission_type (const string) Determines what the number passed to the `commission_value` expresses: strategy.commission.percent for a percentage of the cash volume of the order, strategy.commission.cash_per_contract for currency per contract, strategy.commission.cash_per_order for currency per order. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is strategy.commission.percent.
commission_value (const int/float) Commission applied to the strategy's orders in units determined by the argument passed to the `commission_type` parameter. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is 0.
process_orders_on_close (const bool) When set to true, generates an additional attempt to execute orders after a bar closes and strategy calculations are completed. If the orders are market orders, the broker emulator executes them before the next bar's open. If the orders are price-dependent, they will only be filled if the price conditions are met. This option is useful if you wish to close positions on the current bar. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is false.
close_entries_rule (const string) Determines the order in which trades are closed. Possible values are: "FIFO" (First-In, First-Out) if the earliest exit order must close the earliest entry order, or "ANY" if the orders are closed based on the `from_entry` parameter of the strategy.exit function. "FIFO" can only be used with stocks, futures and US forex (NFA Compliance Rule 2-43b), while "ANY" is allowed in non-US forex. Optional. The default is "FIFO".
margin_long (const int/float) Margin long is the percentage of the purchase price of a security that must be covered by cash or collateral for long positions. Must be a non-negative number. The logic used to simulate margin calls is explained in the Help Center. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is 0, in which case the strategy does not enforce any limits on position size.
margin_short (const int/float) Margin short is the percentage of the purchase price of a security that must be covered by cash or collateral for short positions. Must be a non-negative number. The logic used to simulate margin calls is explained in the Help Center. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is 0, in which case the strategy does not enforce any limits on position size.
explicit_plot_zorder (const bool) Specifies the order in which the script's plots, fills, and hlines are rendered. If true, plots are drawn in the order in which they appear in the script's code, each newer plot being drawn above the previous ones. This only applies to `plot*()` functions, fill, and hline. Optional. The default is false.
max_lines_count (const int) The number of last line drawings displayed. Possible values: 1-500. Optional. The default is 50.
max_labels_count (const int) The number of last label drawings displayed. Possible values: 1-500. Optional. The default is 50.
max_boxes_count (const int) The number of last box drawings displayed. Possible values: 1-500. Optional. The default is 50.
risk_free_rate (const int/float) The risk-free rate of return is the annual percentage change in the value of an investment with minimal or zero risk. It is used to calculate the Sharpe and Sortino ratios. Optional. The default is 2.
use_bar_magnifier (const bool) When true, the Broker Emulator uses lower timeframe data during history backtesting to achieve more realistic results. Optional. The default is false. Only Premium accounts have access to this feature.
fill_orders_on_standard_ohlc (const bool) When true, forces the strategy to fill orders on Heikin Ashi charts using actual OHLC values. Optional. The default is false.
REMARKS
You can learn more about strategies in our User Manual.
Every strategy script must have one strategy call.
Strategies using `calc_on_every_tick = true` parameter may calculate differently on historical and realtime bars, which causes repainting.
Strategies always use the chart's prices to enter and exit positions. Using them on non-standard chart types (Heikin Ashi, Renko, etc.) will produce misleading results, as their prices are synthetic. Backtesting on non-standard charts is thus not recommended.
SEE ALSO
indicator
library
strategy.cancel

It is a command to cancel/deactivate pending orders by referencing their names, which were generated by the functions: strategy.order, strategy.entry and strategy.exit.
strategy.cancel(id) → void
EXAMPLE

//@version=5
strategy(title = "simple order cancellation example")
conditionForBuy = open > high[1]
if conditionForBuy
    strategy.entry("long", strategy.long, 1, limit = low) // enter long using limit order at low price of current bar if conditionForBuy is true
if not conditionForBuy
    strategy.cancel("long") // cancel the entry order with name "long" if conditionForBuy is false
ARGUMENTS
id (series string) A required parameter. The order identifier. It is possible to cancel an order by referencing its identifier.
strategy.cancel_all

It is a command to cancel/deactivate all pending orders, which were generated by the functions: strategy.order, strategy.entry and strategy.exit.
strategy.cancel_all() → void
EXAMPLE

//@version=5
strategy(title = "simple all orders cancellation example")
conditionForBuy1 = open > high[1]
if conditionForBuy1
    strategy.entry("long entry 1", strategy.long, 1, limit = low) // enter long by limit if conditionForBuy1 is true
conditionForBuy2 = conditionForBuy1 and open[1] > high[2]
if conditionForBuy2
    strategy.entry("long entry 2", strategy.long, 1, limit = ta.lowest(low, 2)) // enter long by limit if conditionForBuy2 is true
conditionForStopTrading = open < ta.lowest(low, 2)
if conditionForStopTrading
    strategy.cancel_all() // cancel both limit orders if the conditon conditionForStopTrading is true
strategy.close

It is a command to exit from the entry with the specified ID. If there were multiple entry orders with the same ID, all of them are exited at once. If there are no open entries with the specified ID by the moment the command is triggered, the command will not come into effect. The command uses market order. Every entry is closed by a separate market order.
strategy.close(id, comment, qty, qty_percent, alert_message, immediately) → void
EXAMPLE

//@version=5
strategy("closeEntry Demo", overlay=false)
if open > close
    strategy.entry("buy", strategy.long)
if open < close
    strategy.close("buy", qty_percent = 50, comment = "close buy entry for 50%")
plot(strategy.position_size)
ARGUMENTS
id (series string) A required parameter. The order identifier. It is possible to close an order by referencing its identifier.
comment (series string) An optional parameter. Additional notes on the order.
qty (series int/float) An optional parameter. Number of contracts/shares/lots/units to exit a trade with. The default value is 'NaN'.
qty_percent (series int/float) Defines the percentage (0-100) of the position to close. Its priority is lower than that of the 'qty' parameter. Optional. The default is 100.
alert_message (series string) An optional parameter which replaces the {{strategy.order.alert_message}} placeholder when it is used in the "Create Alert" dialog box's "Message" field.
immediately (series bool) An optional parameter. If true, the closing order will be executed on the tick where it has been placed, ignoring the strategy parameters that restrict the order execution to the open of the next bar. The default is false.
disable_alert (series bool) If true when the function creates an order, the strategy alert will not fire upon the execution of that order. The parameter accepts a 'series bool' argument, allowing users to control which orders will trigger alerts when they fill. Optional. The default is false.
strategy.close_all

Exits the current market position, making it flat.
strategy.close_all(comment, alert_message, immediately) → void
EXAMPLE

//@version=5
strategy("closeAll Demo", overlay=false)
if open > close
    strategy.entry("buy", strategy.long)
if open < close
    strategy.close_all(comment = "close all entries")
plot(strategy.position_size)
ARGUMENTS
comment (series string) An optional parameter. Additional notes on the order.
alert_message (series string) An optional parameter which replaces the {{strategy.order.alert_message}} placeholder when it is used in the "Create Alert" dialog box's "Message" field.
immediately (series bool) An optional parameter. If true, the closing order will be executed on the tick where it has been placed, ignoring the strategy parameters that restrict the order execution to the open of the next bar. The default is false.
disable_alert (series bool) If true when the function creates an order, the strategy alert will not fire upon the execution of that order. The parameter accepts a 'series bool' argument, allowing users to control which orders will trigger alerts when they fill. Optional. The default is false.
strategy.closedtrades.commission

Returns the sum of entry and exit fees paid in the closed trade.
strategy.closedtrades.commission(trade_num) → series float
EXAMPLE

//@version=5
strategy("`strategy.closedtrades.commission` Example", commission_type = strategy.commission.percent, commission_value = 0.1)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Plot total fees for the latest closed trade.
plot(strategy.closedtrades.commission(strategy.closedtrades - 1))
ARGUMENTS
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
SEE ALSO
strategy
strategy.closedtrades.entry_bar_index

Returns the bar_index of the closed trade's entry.
strategy.closedtrades.entry_bar_index(trade_num) → series int
EXAMPLE

//@version=5
strategy("strategy.closedtrades.entry_bar_index Example")
// Enter long trades on three rising bars; exit on two falling bars.
if ta.rising(close, 3)
    strategy.entry("Long", strategy.long)
if ta.falling(close, 2)
    strategy.close("Long")
// Function that calculates the average amount of bars in a trade.
avgBarsPerTrade() =>
    sumBarsPerTrade = 0
    for tradeNo = 0 to strategy.closedtrades - 1
        // Loop through all closed trades, starting with the oldest.
        sumBarsPerTrade += strategy.closedtrades.exit_bar_index(tradeNo) - strategy.closedtrades.entry_bar_index(tradeNo) + 1
    result = nz(sumBarsPerTrade / strategy.closedtrades)
plot(avgBarsPerTrade())
ARGUMENTS
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
SEE ALSO
strategy.closedtrades.exit_bar_index
strategy.closedtrades.entry_comment

Returns the comment message of the closed trade's entry, or na if there is no entry with this `trade_num`.
strategy.closedtrades.entry_comment(trade_num) → series string
EXAMPLE

//@version=5
strategy("`strategy.closedtrades.entry_comment()` Example", overlay = true)

stopPrice = open * 1.01

longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))

if (longCondition)
    strategy.entry("Long", strategy.long, stop = stopPrice, comment = str.tostring(stopPrice, "#.####"))
    strategy.exit("EXIT", trail_points = 1000, trail_offset = 0)

var testTable = table.new(position.top_right, 1, 3, color.orange, border_width = 1)

if barstate.islastconfirmedhistory or barstate.isrealtime
    table.cell(testTable, 0, 0, 'Last closed trade:')
    table.cell(testTable, 0, 1, "Order stop price value: " + strategy.closedtrades.entry_comment(strategy.closedtrades - 1))
    table.cell(testTable, 0, 2, "Actual Entry Price: " + str.tostring(strategy.closedtrades.entry_price(strategy.closedtrades - 1)))
ARGUMENTS
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
SEE ALSO
strategy
strategy.entry
strategy.closedtrades
strategy.closedtrades.entry_id

Returns the id of the closed trade's entry.
strategy.closedtrades.entry_id(trade_num) → series string
EXAMPLE

//@version=5
strategy("strategy.closedtrades.entry_id Example", overlay = true)

// Enter a short position and close at the previous to last bar.
if bar_index == 1
    strategy.entry("Short at bar #" + str.tostring(bar_index), strategy.short)
if bar_index == last_bar_index - 2
    strategy.close_all()
    
// Display ID of the last entry position.
if barstate.islastconfirmedhistory
    label.new(last_bar_index, high, "Last Entry ID is: " + strategy.closedtrades.entry_id(strategy.closedtrades - 1))
RETURNS
Returns the id of the closed trade's entry.
ARGUMENTS
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
REMARKS
The function returns na if trade_num is not in the range: 0 to strategy.closedtrades-1.
SEE ALSO
strategy.closedtrades.entry_bar_index
strategy.closedtrades.entry_price

Returns the price of the closed trade's entry.
strategy.closedtrades.entry_price(trade_num) → series float
EXAMPLE

//@version=5
strategy("strategy.closedtrades.entry_price Example 1")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Return the entry price for the latest  entry.
entryPrice = strategy.closedtrades.entry_price(strategy.closedtrades - 1)

plot(entryPrice, "Long entry price")
EXAMPLE

// Calculates the average profit percentage for all closed trades.
//@version=5
strategy("strategy.closedtrades.entry_price Example 2")

// Strategy calls to create single short and long trades
if bar_index == last_bar_index - 15
    strategy.entry("Long Entry",  strategy.long)
else if bar_index == last_bar_index - 10
    strategy.close("Long Entry")
    strategy.entry("Short", strategy.short)
else if bar_index == last_bar_index - 5
    strategy.close("Short")

// Calculate profit for both closed trades.
profitPct = 0.0
for tradeNo = 0 to strategy.closedtrades - 1
    entryP = strategy.closedtrades.entry_price(tradeNo)
    exitP = strategy.closedtrades.exit_price(tradeNo)
    profitPct += (exitP - entryP) / entryP * strategy.closedtrades.size(tradeNo) * 100
    
// Calculate average profit percent for both closed trades.
avgProfitPct = nz(profitPct / strategy.closedtrades)

plot(avgProfitPct)
ARGUMENTS
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
SEE ALSO
strategy.closedtrades.entry_price
strategy.closedtrades.exit_price
strategy.closedtrades.size
strategy.closedtrades
strategy.closedtrades.entry_time

Returns the UNIX time of the closed trade's entry.
strategy.closedtrades.entry_time(trade_num) → series int
EXAMPLE

//@version=5
strategy("strategy.closedtrades.entry_time Example", overlay = true)

// Enter long trades on three rising bars; exit on two falling bars.
if ta.rising(close, 3)
    strategy.entry("Long", strategy.long)
if ta.falling(close, 2)
    strategy.close("Long")

// Calculate the average trade duration 
avgTradeDuration() =>
    sumTradeDuration = 0
    for i = 0 to strategy.closedtrades - 1
        sumTradeDuration += strategy.closedtrades.exit_time(i) - strategy.closedtrades.entry_time(i)
    result = nz(sumTradeDuration / strategy.closedtrades)

// Display average duration converted to seconds and formatted using 2 decimal points
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(avgTradeDuration() / 1000, "#.##") + " seconds")
ARGUMENTS
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
SEE ALSO
strategy.opentrades.entry_time
strategy.closedtrades.exit_bar_index

Returns the bar_index of the closed trade's exit.
strategy.closedtrades.exit_bar_index(trade_num) → series int
EXAMPLE

//@version=5
strategy("strategy.closedtrades.exit_bar_index Example 1")

// Strategy calls to place a single short trade. We enter the trade at the first bar and exit the trade at 10 bars before the last chart bar.
if bar_index == 0
    strategy.entry("Short",  strategy.short)
if bar_index == last_bar_index - 10
    strategy.close("Short")

// Calculate the amount of bars since the last closed trade.
barsSinceClosed = strategy.closedtrades > 0 ? bar_index - strategy.closedtrades.exit_bar_index(strategy.closedtrades - 1) : na

plot(barsSinceClosed, "Bars since last closed trade")
EXAMPLE

// Calculates the average amount of bars per trade.
//@version=5
strategy("strategy.closedtrades.exit_bar_index Example 2")

// Enter long trades on three rising bars; exit on two falling bars.
if ta.rising(close, 3)
    strategy.entry("Long", strategy.long)
if ta.falling(close, 2)
    strategy.close("Long")

// Function that calculates the average amount of bars per trade.
avgBarsPerTrade() =>
    sumBarsPerTrade = 0
    for tradeNo = 0 to strategy.closedtrades - 1
        // Loop through all closed trades, starting with the oldest.
        sumBarsPerTrade += strategy.closedtrades.exit_bar_index(tradeNo) - strategy.closedtrades.entry_bar_index(tradeNo) + 1
    result = nz(sumBarsPerTrade / strategy.closedtrades)

plot(avgBarsPerTrade())
ARGUMENTS
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
SEE ALSO
bar_index
strategy.closedtrades.exit_comment

Returns the comment message of the closed trade's exit, or na if there is no entry with this `trade_num`.
strategy.closedtrades.exit_comment(trade_num) → series string
EXAMPLE

//@version=5
strategy("`strategy.closedtrades.exit_comment()` Example", overlay = true)

longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))
if (longCondition)
    strategy.entry("Long", strategy.long)
    strategy.exit("Exit", stop = open * 0.95, limit = close * 1.05, trail_points = 100, trail_offset = 0, comment_profit = "TP", comment_loss = "SL", comment_trailing = "TRAIL")

exitStats() =>
    int slCount = 0
    int tpCount = 0
    int trailCount = 0
    
    if strategy.closedtrades > 0
        for i = 0 to strategy.closedtrades - 1
            switch strategy.closedtrades.exit_comment(i)
                "TP"    => tpCount    += 1
                "SL"    => slCount    += 1
                "TRAIL" => trailCount += 1
    [slCount, tpCount, trailCount]

var testTable = table.new(position.top_right, 1, 4, color.orange, border_width = 1)

if barstate.islastconfirmedhistory
    [slCount, tpCount, trailCount] = exitStats()
    table.cell(testTable, 0, 0, "Closed trades (" + str.tostring(strategy.closedtrades) +") stats:")
    table.cell(testTable, 0, 1, "Stop Loss: " + str.tostring(slCount))
    table.cell(testTable, 0, 2, "Take Profit: " + str.tostring(tpCount))
    table.cell(testTable, 0, 3, "Trailing Stop: " + str.tostring(trailCount))
ARGUMENTS
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
SEE ALSO
strategy
strategy.closedtrades.exit_id

Returns the id of the closed trade's exit.
strategy.closedtrades.exit_id(trade_num) → series string
EXAMPLE

//@version=5
strategy("strategy.closedtrades.exit_id Example", overlay = true)

// Strategy calls to create single short and long trades
if bar_index == last_bar_index - 15
    strategy.entry("Long Entry",  strategy.long)
else if bar_index == last_bar_index - 10
    strategy.entry("Short Entry", strategy.short)
    
// When a new open trade is detected then we create the exit strategy corresponding with the matching entry id
// We detect the correct entry id by determining if a position is long or short based on the position quantity
if ta.change(strategy.opentrades)
    posSign = strategy.opentrades.size(strategy.opentrades - 1)
    strategy.exit(posSign > 0 ? "SL Long Exit" : "SL Short Exit", strategy.opentrades.entry_id(strategy.opentrades - 1), stop = posSign > 0 ? high - ta.tr : low + ta.tr)

// When a new closed trade is detected then we place a label above the bar with the exit info
if ta.change(strategy.closedtrades)
    msg = "Trade closed by: " + strategy.closedtrades.exit_id(strategy.closedtrades - 1)
    label.new(bar_index, high + (3 * ta.tr), msg)
RETURNS
Returns the id of the closed trade's exit.
ARGUMENTS
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
REMARKS
The function returns na if trade_num is not in the range: 0 to strategy.closedtrades-1.
SEE ALSO
strategy.closedtrades.exit_bar_index
strategy.closedtrades.exit_price

Returns the price of the closed trade's exit.
strategy.closedtrades.exit_price(trade_num) → series float
EXAMPLE

//@version=5
strategy("strategy.closedtrades.exit_price Example 1")

// We are creating a long trade every 5 bars
if bar_index % 5 == 0
    strategy.entry("Long",  strategy.long)
strategy.close("Long")

// Return the exit price from the latest closed trade.
exitPrice = strategy.closedtrades.exit_price(strategy.closedtrades - 1)

plot(exitPrice, "Long exit price")
EXAMPLE

// Calculates the average profit percentage for all closed trades.
//@version=5
strategy("strategy.closedtrades.exit_price Example 2")

// Strategy calls to create single short and long trades.
if bar_index == last_bar_index - 15
    strategy.entry("Long Entry",  strategy.long)
else if bar_index == last_bar_index - 10
    strategy.close("Long Entry")
    strategy.entry("Short", strategy.short)
else if bar_index == last_bar_index - 5
    strategy.close("Short")

// Calculate profit for both closed trades.
profitPct = 0.0
for tradeNo = 0 to strategy.closedtrades - 1
    entryP = strategy.closedtrades.entry_price(tradeNo)
    exitP = strategy.closedtrades.exit_price(tradeNo)
    profitPct += (exitP - entryP) / entryP * strategy.closedtrades.size(tradeNo) * 100
    
// Calculate average profit percent for both closed trades.
avgProfitPct = nz(profitPct / strategy.closedtrades)

plot(avgProfitPct)
ARGUMENTS
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
SEE ALSO
strategy.closedtrades.entry_price
strategy.closedtrades.exit_time

Returns the UNIX time of the closed trade's exit.
strategy.closedtrades.exit_time(trade_num) → series int
EXAMPLE

//@version=5
strategy("strategy.closedtrades.exit_time Example 1")

// Enter long trades on three rising bars; exit on two falling bars.
if ta.rising(close, 3)
    strategy.entry("Long", strategy.long)
if ta.falling(close, 2)
    strategy.close("Long")

// Calculate the average trade duration. 
avgTradeDuration() =>
    sumTradeDuration = 0
    for i = 0 to strategy.closedtrades - 1
        sumTradeDuration += strategy.closedtrades.exit_time(i) - strategy.closedtrades.entry_time(i)
    result = nz(sumTradeDuration / strategy.closedtrades)

// Display average duration converted to seconds and formatted using 2 decimal points.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(avgTradeDuration() / 1000, "#.##") + " seconds")
EXAMPLE

// Reopens a closed trade after X seconds.
//@version=5
strategy("strategy.closedtrades.exit_time Example 2")

// Strategy calls to emulate a single long trade at the first bar.
if bar_index == 0
    strategy.entry("Long", strategy.long)

reopenPositionAfter(timeSec) =>
    if strategy.closedtrades > 0
        if time - strategy.closedtrades.exit_time(strategy.closedtrades - 1) >= timeSec * 1000
            strategy.entry("Long", strategy.long)

// Reopen last closed position after 120 sec.                
reopenPositionAfter(120)

if ta.change(strategy.opentrades)
    strategy.exit("Long", stop = low * 0.9, profit = high * 2.5)
ARGUMENTS
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
SEE ALSO
strategy.closedtrades.entry_time
strategy.closedtrades.max_drawdown

Returns the maximum drawdown of the closed trade, i.e., the maximum possible loss during the trade.
strategy.closedtrades.max_drawdown(trade_num) → series float
EXAMPLE

//@version=5
strategy("`strategy.closedtrades.max_drawdown` Example")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Get the biggest max trade drawdown value from all of the closed trades.
maxTradeDrawDown() =>
    maxDrawdown = 0.0
    for tradeNo = 0 to strategy.closedtrades - 1
        maxDrawdown := math.max(maxDrawdown, strategy.closedtrades.max_drawdown(tradeNo))
    result = maxDrawdown

plot(maxTradeDrawDown(), "Biggest max drawdown")
ARGUMENTS
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
REMARKS
The function returns na if trade_num is not in the range: 0 to strategy.closedtrades - 1.
SEE ALSO
strategy.opentrades.max_drawdown
strategy.closedtrades.max_runup

Returns the maximum run up of the closed trade, i.e., the maximum possible profit during the trade.
strategy.closedtrades.max_runup(trade_num) → series float
EXAMPLE

//@version=5
strategy("`strategy.closedtrades.max_runup` Example")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Get the biggest max trade runup value from all of the closed trades.
maxTradeRunUp() =>
    maxRunup = 0.0
    for tradeNo = 0 to strategy.closedtrades - 1
        maxRunup := math.max(maxRunup, strategy.closedtrades.max_runup(tradeNo))
    result = maxRunup

plot(maxTradeRunUp(), "Max trade runup")
ARGUMENTS
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
SEE ALSO
strategy.opentrades.max_runup
strategy.closedtrades.profit

Returns the profit/loss of the closed trade. Losses are expressed as negative values.
strategy.closedtrades.profit(trade_num) → series float
EXAMPLE

//@version=5
strategy("`strategy.closedtrades.profit` Example")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Calculate average gross profit by adding the difference between gross profit and commission.
avgGrossProfit() =>
    sumGrossProfit = 0.0
    for tradeNo = 0 to strategy.closedtrades - 1
        sumGrossProfit += strategy.closedtrades.profit(tradeNo) - strategy.closedtrades.commission(tradeNo)
    result = nz(sumGrossProfit / strategy.closedtrades)
    
plot(avgGrossProfit(), "Average gross profit")
ARGUMENTS
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
SEE ALSO
strategy.opentrades.profit
strategy.closedtrades.size

Returns the direction and the number of contracts traded in the closed trade. If the value is > 0, the market position was long. If the value is < 0, the market position was short.
strategy.closedtrades.size(trade_num) → series float
EXAMPLE

//@version=5
strategy("`strategy.closedtrades.size` Example 1")

// We calculate the max amt of shares we can buy.
amtShares = math.floor(strategy.equity / close)
// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long, qty = amtShares)
if bar_index % 20 == 0
    strategy.close("Long")

// Plot the number of contracts traded in the last closed trade.     
plot(strategy.closedtrades.size(strategy.closedtrades - 1), "Number of contracts traded")
EXAMPLE

// Calculates the average profit percentage for all closed trades.
//@version=5
strategy("`strategy.closedtrades.size` Example 2")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")


// Calculate profit for both closed trades.
profitPct = 0.0
for tradeNo = 0 to strategy.closedtrades - 1
    entryP = strategy.closedtrades.entry_price(tradeNo)
    exitP = strategy.closedtrades.exit_price(tradeNo)
    profitPct += (exitP - entryP) / entryP * strategy.closedtrades.size(tradeNo) * 100
    
// Calculate average profit percent for both closed trades.
avgProfitPct = nz(profitPct / strategy.closedtrades)

plot(avgProfitPct)
ARGUMENTS
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
SEE ALSO
strategy.opentrades.size
strategy.convert_to_account

Converts the value from the currency that the symbol on the chart is traded in (syminfo.currency) to the currency used by the strategy (strategy.account_currency).
strategy.convert_to_account(value) → series float
EXAMPLE

//@version=5
strategy("`strategy.convert_to_account` Example 1", currency = currency.EUR)

plot(close, "Close price using default currency")
plot(strategy.convert_to_account(close), "Close price converted to strategy currency")
EXAMPLE

// Calculates the "Buy and hold return" using your account's currency.
//@version=5
strategy("`strategy.convert_to_account` Example 2", currency = currency.EUR)

dateInput = input.time(timestamp("20 Jul 2021 00:00 +0300"), "From Date", confirm = true)

buyAndHoldReturnPct(fromDate) =>
    if time >= fromDate
        money = close * syminfo.pointvalue
        var initialBal = strategy.convert_to_account(money)
        (strategy.convert_to_account(money) - initialBal) / initialBal * 100
        
plot(buyAndHoldReturnPct(dateInput))
ARGUMENTS
value (series int/float) The value to be converted.
SEE ALSO
strategy
strategy.convert_to_symbol

Converts the value from the currency used by the strategy (strategy.account_currency) to the currency that the symbol on the chart is traded in (syminfo.currency).
strategy.convert_to_symbol(value) → series float
EXAMPLE

//@version=5
strategy("`strategy.convert_to_symbol` Example", currency = currency.EUR)

// Calculate the max qty we can buy using current chart's currency.
calcContracts(accountMoney) =>
    math.floor(strategy.convert_to_symbol(accountMoney) / syminfo.pointvalue / close)

// Return max qty we can buy using 300 euros
qt = calcContracts(300)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars using our custom qty.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long, qty = qt)
if bar_index % 20 == 0
    strategy.close("Long")
ARGUMENTS
value (series int/float) The value to be converted.
SEE ALSO
strategy
strategy.entry

It is a command to enter market position. If an order with the same ID is already pending, it is possible to modify the order. If there is no order with the specified ID, a new order is placed. To deactivate an entry order, the command strategy.cancel or strategy.cancel_all should be used. In comparison to the function strategy.order, the function strategy.entry is affected by pyramiding and it can reverse market position correctly. If both 'limit' and 'stop' parameters are 'NaN', the order type is market order.
strategy.entry(id, direction, qty, limit, stop, oca_name, oca_type, comment, alert_message, disable_alert) → void
strategy.entry(id, direction, qty, limit, stop, oca_name, oca_type, comment, alert_message) → void
EXAMPLE

//@version=5
strategy(title = "simple strategy entry example")
if open > high[1]
    strategy.entry("enter long", strategy.long, 1) // enter long by market if current open great then previous high
if open < low[1]
    strategy.entry("enter short", strategy.short, 1) // enter short by market if current open less then previous low
ARGUMENTS
id (series string) A required parameter. The order identifier. It is possible to cancel or modify an order by referencing its identifier.
direction (series strategy_direction) A required parameter. Market position direction: 'strategy.long' is for long, 'strategy.short' is for short.
qty (series int/float) An optional parameter. Number of contracts/shares/lots/units to trade. The default value is 'NaN'.
limit (series int/float) An optional parameter. Limit price of the order. If it is specified, the order type is either 'limit', or 'stop-limit'. 'NaN' should be specified for any other order type.
stop (series int/float) An optional parameter. Stop price of the order. If it is specified, the order type is either 'stop', or 'stop-limit'. 'NaN' should be specified for any other order type.
oca_name (series string) An optional parameter. Name of the OCA group the order belongs to. If the order should not belong to any particular OCA group, there should be an empty string.
oca_type (input string) An optional parameter. Type of the OCA group. The allowed values are: strategy.oca.none - the order should not belong to any particular OCA group; strategy.oca.cancel - the order should belong to an OCA group, where as soon as an order is filled, all other orders of the same group are cancelled; strategy.oca.reduce - the order should belong to an OCA group, where if X number of contracts of an order is filled, number of contracts for each other order of the same OCA group is decreased by X.
comment (series string) An optional parameter. Additional notes on the order.
alert_message (series string) An optional parameter which replaces the {{strategy.order.alert_message}} placeholder when it is used in the "Create Alert" dialog box's "Message" field.
disable_alert (series bool) If true when the function creates an order, the strategy alert will not fire upon the execution of that order. The parameter accepts a 'series bool' argument, allowing users to control which orders will trigger alerts when they fill. Optional. The default is false.
strategy.exit

It is a command to exit either a specific entry, or whole market position. If an order with the same ID is already pending, it is possible to modify the order. If an entry order was not filled, but an exit order is generated, the exit order will wait till entry order is filled and then the exit order is placed. To deactivate an exit order, the command strategy.cancel or strategy.cancel_all should be used. If the function strategy.exit is called once, it exits a position only once. If you want to exit multiple times, the command strategy.exit should be called multiple times. If you use a stop loss and a trailing stop, their order type is 'stop', so only one of them is placed (the one that is supposed to be filled first). If all the following parameters 'profit', 'limit', 'loss', 'stop', 'trail_points', 'trail_offset' are 'NaN', the command will fail. To use market order to exit, the command strategy.close or strategy.close_all should be used.
strategy.exit(id, from_entry, qty, qty_percent, profit, limit, loss, stop, trail_price, trail_points, trail_offset, oca_name, comment, comment_profit, comment_loss, comment_trailing, alert_message, alert_profit, alert_loss, alert_trailing, disable_alert) → void
strategy.exit(id, from_entry, qty, qty_percent, profit, limit, loss, stop, trail_price, trail_points, trail_offset, oca_name, comment, comment_profit, comment_loss, comment_trailing, alert_message, alert_profit, alert_loss, alert_trailing) → void
EXAMPLE

//@version=5
strategy(title = "simple strategy exit example")
if open > high[1]
    strategy.entry("long", strategy.long, 1) // enter long by market if current open great then previous high
strategy.exit("exit", "long", profit = 10, loss = 5) // generate full exit bracket (profit 10 points, loss 5 points per contract) from entry with name "long"
ARGUMENTS
id (series string) A required parameter. The order identifier. It is possible to cancel or modify an order by referencing its identifier.
from_entry (series string) An optional parameter. The identifier of a specific entry order to exit from it. To exit all entries an empty string should be used. The default values is empty string.
qty (series int/float) An optional parameter. Number of contracts/shares/lots/units to exit a trade with. The default value is 'NaN'.
qty_percent (series int/float) Defines the percentage of (0-100) the position to close. Its priority is lower than that of the 'qty' parameter. Optional. The default is 100.
profit (series int/float) An optional parameter. Profit target (specified in ticks). If it is specified, a limit order is placed to exit market position when the specified amount of profit (in ticks) is reached. The default value is 'NaN'.
limit (series int/float) An optional parameter. Profit target (requires a specific price). If it is specified, a limit order is placed to exit market position at the specified price (or better). Priority of the parameter 'limit' is higher than priority of the parameter 'profit' ('limit' is used instead of 'profit', if its value is not 'NaN'). The default value is 'NaN'.
loss (series int/float) An optional parameter. Stop loss (specified in ticks). If it is specified, a stop order is placed to exit market position when the specified amount of loss (in ticks) is reached. The default value is 'NaN'.
stop (series int/float) An optional parameter. Stop loss (requires a specific price). If it is specified, a stop order is placed to exit market position at the specified price (or worse). Priority of the parameter 'stop' is higher than priority of the parameter 'loss' ('stop' is used instead of 'loss', if its value is not 'NaN'). The default value is 'NaN'.
trail_price (series int/float) An optional parameter. Trailing stop activation level (requires a specific price). If it is specified, a trailing stop order will be placed when the specified price level is reached. The offset (in ticks) to determine initial price of the trailing stop order is specified in the 'trail_offset' parameter: X ticks lower than activation level to exit long position; X ticks higher than activation level to exit short position. The default value is 'NaN'.
trail_points (series int/float) An optional parameter. Trailing stop activation level (profit specified in ticks). If it is specified, a trailing stop order will be placed when the calculated price level (specified amount of profit) is reached. The offset (in ticks) to determine initial price of the trailing stop order is specified in the 'trail_offset' parameter: X ticks lower than activation level to exit long position; X ticks higher than activation level to exit short position. The default value is 'NaN'.
trail_offset (series int/float) An optional parameter. Trailing stop price (specified in ticks). The offset in ticks to determine initial price of the trailing stop order: X ticks lower than 'trail_price' or 'trail_points' to exit long position; X ticks higher than 'trail_price' or 'trail_points' to exit short position. The default value is 'NaN'.
oca_name (series string) An optional parameter. Name of the OCA group (oca_type = strategy.oca.reduce) the profit target, the stop loss / the trailing stop orders belong to. If the name is not specified, it will be generated automatically.
comment (series string) Additional notes on the order. If specified, displays near the order marker on the chart. Optional. The default is na.
comment_profit (series string) Additional notes on the order if the exit was triggered by crossing `profit` or `limit` specifically. If specified, supercedes the `comment` parameter and displays near the order marker on the chart. Optional. The default is na.
comment_loss (series string) Additional notes on the order if the exit was triggered by crossing `stop` or `loss` specifically. If specified, supercedes the `comment` parameter and displays near the order marker on the chart. Optional. The default is na.
comment_trailing (series string) Additional notes on the order if the exit was triggered by crossing `trail_offset` specifically. If specified, supercedes the `comment` parameter and displays near the order marker on the chart. Optional. The default is na.
alert_message (series string) Text that will replace the '{{strategy.order.alert_message}}' placeholder when one is used in the "Message" field of the "Create Alert" dialog. Optional. The default is na.
alert_profit (series string) Text that will replace the '{{strategy.order.alert_message}}' placeholder when one is used in the "Message" field of the "Create Alert" dialog. Only replaces the text if the exit was triggered by crossing `profit` or `limit` specifically. Optional. The default is na.
alert_loss (series string) Text that will replace the '{{strategy.order.alert_message}}' placeholder when one is used in the "Message" field of the "Create Alert" dialog. Only replaces the text if the exit was triggered by crossing `stop` or `loss` specifically. Optional. The default is na.
alert_trailing (series string) Text that will replace the '{{strategy.order.alert_message}}' placeholder when one is used in the "Message" field of the "Create Alert" dialog. Only replaces the text if the exit was triggered by crossing `trail_offset` specifically. Optional. The default is na.
disable_alert (series bool) If true when the function creates an order, the strategy alert will not fire upon the execution of that order. The parameter accepts a 'series bool' argument, allowing users to control which orders will trigger alerts when they fill. Optional. The default is false.
strategy.opentrades.commission

Returns the sum of entry and exit fees paid in the open trade.
strategy.opentrades.commission(trade_num) → series float
EXAMPLE

// Calculates the gross profit or loss for the current open position.
//@version=5
strategy("`strategy.opentrades.commission` Example", commission_type = strategy.commission.percent, commission_value = 0.1)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Calculate gross profit or loss for open positions only.
tradeOpenGrossPL() =>
    sumOpenGrossPL = 0.0
    for tradeNo = 0 to strategy.opentrades - 1
        sumOpenGrossPL += strategy.opentrades.profit(tradeNo) - strategy.opentrades.commission(tradeNo)
    result = sumOpenGrossPL
    
plot(tradeOpenGrossPL())
ARGUMENTS
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
SEE ALSO
strategy
strategy.opentrades.entry_bar_index

Returns the bar_index of the open trade's entry.
strategy.opentrades.entry_bar_index(trade_num) → series int
EXAMPLE

// Wait 10 bars and then close the position.
//@version=5
strategy("`strategy.opentrades.entry_bar_index` Example")

barsSinceLastEntry() =>
    strategy.opentrades > 0 ? bar_index - strategy.opentrades.entry_bar_index(strategy.opentrades - 1) : na

// Enter a long position if there are no open positions.
if strategy.opentrades == 0
    strategy.entry("Long",  strategy.long)

// Close the long position after 10 bars. 
if barsSinceLastEntry() >= 10
    strategy.close("Long")
ARGUMENTS
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
SEE ALSO
strategy.closedtrades.entry_bar_index
strategy.opentrades.entry_comment

Returns the comment message of the open trade's entry, or na if there is no entry with this `trade_num`.
strategy.opentrades.entry_comment(trade_num) → series string
EXAMPLE

//@version=5
strategy("`strategy.opentrades.entry_comment()` Example", overlay = true)

stopPrice = open * 1.01

longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))

if (longCondition)
    strategy.entry("Long", strategy.long, stop = stopPrice, comment = str.tostring(stopPrice, "#.####"))

var testTable = table.new(position.top_right, 1, 3, color.orange, border_width = 1)

if barstate.islastconfirmedhistory or barstate.isrealtime
    table.cell(testTable, 0, 0, 'Last entry stats')
    table.cell(testTable, 0, 1, "Order stop price value: " + strategy.opentrades.entry_comment(strategy.opentrades - 1))
    table.cell(testTable, 0, 2, "Actual Entry Price: " + str.tostring(strategy.opentrades.entry_price(strategy.opentrades - 1)))
ARGUMENTS
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
SEE ALSO
strategy
strategy.entry
strategy.opentrades
strategy.opentrades.entry_id

Returns the id of the open trade's entry.
strategy.opentrades.entry_id(trade_num) → series string
EXAMPLE

//@version=5
strategy("`strategy.opentrades.entry_id` Example", overlay = true)

// We enter a long position when 14 period sma crosses over 28 period sma.
// We enter a short position when 14 period sma crosses under 28 period sma.
longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))
shortCondition = ta.crossunder(ta.sma(close, 14), ta.sma(close, 28))

// Strategy calls to enter a long or short position when the corresponding condition is met.
if longCondition
    strategy.entry("Long entry at bar #" + str.tostring(bar_index), strategy.long)
if shortCondition
    strategy.entry("Short entry at bar #" + str.tostring(bar_index), strategy.short)

// Display ID of the latest open position.
if barstate.islastconfirmedhistory
    label.new(bar_index, high + (2 * ta.tr),  "Last opened position is \n " + strategy.opentrades.entry_id(strategy.opentrades - 1))
RETURNS
Returns the id of the open trade's entry.
ARGUMENTS
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
REMARKS
The function returns na if trade_num is not in the range: 0 to strategy.opentrades-1.
SEE ALSO
strategy.opentrades.entry_bar_index
strategy.opentrades.entry_price

Returns the price of the open trade's entry.
strategy.opentrades.entry_price(trade_num) → series float
EXAMPLE

//@version=5
strategy("strategy.opentrades.entry_price Example 1", overlay = true)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if ta.crossover(close, ta.sma(close, 14))
    strategy.entry("Long", strategy.long)

// Return the entry price for the latest closed trade.
currEntryPrice = strategy.opentrades.entry_price(strategy.opentrades - 1)
currExitPrice = currEntryPrice * 1.05

if high >= currExitPrice
    strategy.close("Long")

plot(currEntryPrice, "Long entry price", style = plot.style_linebr)
plot(currExitPrice, "Long exit price", color.green, style = plot.style_linebr)
EXAMPLE

// Calculates the average price for the open position.
//@version=5
strategy("strategy.opentrades.entry_price Example 2", pyramiding = 2)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Calculates the average price for the open position.
avgOpenPositionPrice() =>
    sumOpenPositionPrice = 0.0
    for tradeNo = 0 to strategy.opentrades - 1
        sumOpenPositionPrice += strategy.opentrades.entry_price(tradeNo) * strategy.opentrades.size(tradeNo) / strategy.position_size
    result = nz(sumOpenPositionPrice / strategy.opentrades)

plot(avgOpenPositionPrice())
ARGUMENTS
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
SEE ALSO
strategy.closedtrades.exit_price
strategy.opentrades.entry_time

Returns the UNIX time of the open trade's entry.
strategy.opentrades.entry_time(trade_num) → series int
EXAMPLE

//@version=5
strategy("strategy.opentrades.entry_time Example")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Calculates duration in milliseconds since the last position was opened.
timeSinceLastEntry()=>
    strategy.opentrades > 0 ? (time - strategy.opentrades.entry_time(strategy.opentrades - 1)) : na

plot(timeSinceLastEntry() / 1000 * 60 * 60 * 24, "Days since last entry")
ARGUMENTS
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
SEE ALSO
strategy.closedtrades.entry_time
strategy.opentrades.max_drawdown

Returns the maximum drawdown of the open trade, i.e., the maximum possible loss during the trade.
strategy.opentrades.max_drawdown(trade_num) → series float
EXAMPLE

//@version=5
strategy("strategy.opentrades.max_drawdown Example 1")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Plot the max drawdown of the latest open trade.
plot(strategy.opentrades.max_drawdown(strategy.opentrades - 1), "Max drawdown of the latest open trade")
EXAMPLE

// Calculates the max trade drawdown value for all open trades.
//@version=5
strategy("`strategy.opentrades.max_drawdown` Example 2", pyramiding = 100)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Get the biggest max trade drawdown value from all of the open trades.
maxTradeDrawDown() =>
    maxDrawdown = 0.0
    for tradeNo = 0 to strategy.opentrades - 1
        maxDrawdown := math.max(maxDrawdown, strategy.opentrades.max_drawdown(tradeNo))
    result = maxDrawdown

plot(maxTradeDrawDown(), "Biggest max drawdown")
ARGUMENTS
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
REMARKS
The function returns na if trade_num is not in the range: 0 to strategy.closedtrades - 1.
SEE ALSO
strategy.closedtrades.max_drawdown
strategy.opentrades.max_runup

Returns the maximum run up of the open trade, i.e., the maximum possible profit during the trade.
strategy.opentrades.max_runup(trade_num) → series float
EXAMPLE

//@version=5
strategy("strategy.opentrades.max_runup Example 1")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Plot the max runup of the latest open trade.
plot(strategy.opentrades.max_runup(strategy.opentrades - 1), "Max runup of the latest open trade")
EXAMPLE

// Calculates the max trade runup value for all open trades.
//@version=5
strategy("strategy.opentrades.max_runup Example 2", pyramiding = 100)

// Enter a long position every 30 bars.
if bar_index % 30 == 0
    strategy.entry("Long", strategy.long)

// Calculate biggest max trade runup value from all of the open trades.
maxOpenTradeRunUp() =>
    maxRunup = 0.0
    for tradeNo = 0 to strategy.opentrades - 1
        maxRunup := math.max(maxRunup, strategy.opentrades.max_runup(tradeNo))
    result = maxRunup

plot(maxOpenTradeRunUp(), "Biggest max runup of all open trades")
ARGUMENTS
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
SEE ALSO
strategy.closedtrades.max_runup
strategy.opentrades.profit

Returns the profit/loss of the open trade. Losses are expressed as negative values.
strategy.opentrades.profit(trade_num) → series float
EXAMPLE

// Returns the profit of the last open trade.
//@version=5
strategy("`strategy.opentrades.profit` Example 1", commission_type = strategy.commission.percent, commission_value = 0.1)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

plot(strategy.opentrades.profit(strategy.opentrades - 1), "Profit of the latest open trade")
EXAMPLE

// Calculates the profit for all open trades.
//@version=5
strategy("`strategy.opentrades.profit` Example 2", pyramiding = 5)

// Strategy calls to enter 5 long positions every 2 bars.
if bar_index % 2 == 0
    strategy.entry("Long", strategy.long, qty = 5)

// Calculate open profit or loss for the open positions.
tradeOpenPL() =>
    sumProfit = 0.0
    for tradeNo = 0 to strategy.opentrades - 1
        sumProfit += strategy.opentrades.profit(tradeNo)
    result = sumProfit
    
plot(tradeOpenPL(), "Profit of all open trades")
ARGUMENTS
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
SEE ALSO
strategy.closedtrades.profit
strategy.opentrades.size

Returns the direction and the number of contracts traded in the open trade. If the value is > 0, the market position was long. If the value is < 0, the market position was short.
strategy.opentrades.size(trade_num) → series float
EXAMPLE

//@version=5
strategy("`strategy.opentrades.size` Example 1")

// We calculate the max amt of shares we can buy.
amtShares = math.floor(strategy.equity / close)
// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long, qty = amtShares)
if bar_index % 20 == 0
    strategy.close("Long")

// Plot the number of contracts in the latest open trade.
plot(strategy.opentrades.size(strategy.opentrades - 1), "Amount of contracts in latest open trade")
EXAMPLE

// Calculates the average profit percentage for all open trades.
//@version=5
strategy("`strategy.opentrades.size` Example 2")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Calculate profit for all open trades.
profitPct = 0.0
for tradeNo = 0 to strategy.opentrades - 1
    entryP = strategy.opentrades.entry_price(tradeNo)
    exitP = close
    profitPct += (exitP - entryP) / entryP * strategy.opentrades.size(tradeNo) * 100
    
// Calculate average profit percent for all open trades.
avgProfitPct = nz(profitPct / strategy.opentrades)
plot(avgProfitPct)
ARGUMENTS
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
SEE ALSO
strategy.closedtrades.size
strategy.order

It is a command to place order. If an order with the same ID is already pending, it is possible to modify the order. If there is no order with the specified ID, a new order is placed. To deactivate order, the command strategy.cancel or strategy.cancel_all should be used. In comparison to the function strategy.entry, the function strategy.order is not affected by pyramiding. If both 'limit' and 'stop' parameters are 'NaN', the order type is market order.
strategy.order(id, direction, qty, limit, stop, oca_name, oca_type, comment, alert_message, disable_alert) → void
strategy.order(id, direction, qty, limit, stop, oca_name, oca_type, comment, alert_message) → void
EXAMPLE

//@version=5
strategy(title = "simple strategy order example")
if open > high[1]
    strategy.order("buy", strategy.long, 1) // buy by market if current open great then previous high
if open < low[1]
    strategy.order("sell", strategy.short, 1) // sell by market if current open less then previous low
ARGUMENTS
id (series string) A required parameter. The order identifier. It is possible to cancel or modify an order by referencing its identifier.
direction (series strategy_direction) A required parameter. Order direction: 'strategy.long' is for buy, 'strategy.short' is for sell.
qty (series int/float) An optional parameter. Number of contracts/shares/lots/units to trade. The default value is 'NaN'.
limit (series int/float) An optional parameter. Limit price of the order. If it is specified, the order type is either 'limit', or 'stop-limit'. 'NaN' should be specified for any other order type.
stop (series int/float) An optional parameter. Stop price of the order. If it is specified, the order type is either 'stop', or 'stop-limit'. 'NaN' should be specified for any other order type.
oca_name (series string) An optional parameter. Name of the OCA group the order belongs to. If the order should not belong to any particular OCA group, there should be an empty string.
oca_type (input string) An optional parameter. Type of the OCA group. The allowed values are: strategy.oca.none - the order should not belong to any particular OCA group; strategy.oca.cancel - the order should belong to an OCA group, where as soon as an order is filled, all other orders of the same group are cancelled; strategy.oca.reduce - the order should belong to an OCA group, where if X number of contracts of an order is filled, number of contracts for each other order of the same OCA group is decreased by X.
comment (series string) An optional parameter. Additional notes on the order.
alert_message (series string) An optional parameter which replaces the {{strategy.order.alert_message}} placeholder when it is used in the "Create Alert" dialog box's "Message" field.
disable_alert (series bool) If true when the function creates an order, the strategy alert will not fire upon the execution of that order. The parameter accepts a 'series bool' argument, allowing users to control which orders will trigger alerts when they fill. Optional. The default is false.
strategy.risk.allow_entry_in

This function can be used to specify in which market direction the strategy.entry function is allowed to open positions.
strategy.risk.allow_entry_in(value) → void
EXAMPLE

//@version=5
strategy("strategy.risk.allow_entry_in")

strategy.risk.allow_entry_in(strategy.direction.long)
if open > close
    strategy.entry("Long", strategy.long)
// Instead of opening a short position with 10 contracts, this command will close long entries.
if open < close
    strategy.entry("Short", strategy.short, qty = 10)
ARGUMENTS
value (simple string) The allowed direction. Possible values: strategy.direction.all, strategy.direction.long, strategy.direction.short
strategy.risk.max_cons_loss_days

The purpose of this rule is to cancel all pending orders, close all open positions and stop placing orders after a specified number of consecutive days with losses. The rule affects the whole strategy.
strategy.risk.max_cons_loss_days(count, alert_message) → void
EXAMPLE

//@version=5
strategy("risk.max_cons_loss_days Demo 1")
strategy.risk.max_cons_loss_days(3) // No orders will be placed after 3 days, if each day is with loss.
plot(strategy.position_size)
ARGUMENTS
count (simple int) A required parameter. The allowed number of consecutive days with losses.
alert_message (simple string) An optional parameter which replaces the {{strategy.order.alert_message}} placeholder when it is used in the "Create Alert" dialog box's "Message" field.
strategy.risk.max_drawdown

The purpose of this rule is to determine maximum drawdown. The rule affects the whole strategy. Once the maximum drawdown value is reached, all pending orders are cancelled, all open positions are closed and no new orders can be placed.
strategy.risk.max_drawdown(value, type, alert_message) → void
EXAMPLE

//@version=5
strategy("risk.max_drawdown Demo 1")
strategy.risk.max_drawdown(50, strategy.percent_of_equity) // set maximum drawdown to 50% of maximum equity
plot(strategy.position_size)
EXAMPLE

//@version=5
strategy("risk.max_drawdown Demo 2", currency = "EUR")
strategy.risk.max_drawdown(2000, strategy.cash)  // set maximum drawdown to 2000 EUR from maximum equity
plot(strategy.position_size)
ARGUMENTS
value (simple int/float) A required parameter. The maximum drawdown value. It is specified either in money (base currency), or in percentage of maximum equity. For % of equity the range of allowed values is from 0 to 100.
type (simple string) A required parameter. The type of the value. Please specify one of the following values: strategy.percent_of_equity or strategy.cash. Note: if equity drops down to zero or to a negative and the 'strategy.percent_of_equity' is specified, all pending orders are cancelled, all open positions are closed and no new orders can be placed for good.
alert_message (simple string) An optional parameter which replaces the {{strategy.order.alert_message}} placeholder when it is used in the "Create Alert" dialog box's "Message" field.
strategy.risk.max_intraday_filled_orders

The purpose of this rule is to determine maximum number of filled orders per 1 day (per 1 bar, if chart resolution is higher than 1 day). The rule affects the whole strategy. Once the maximum number of filled orders is reached, all pending orders are cancelled, all open positions are closed and no new orders can be placed till the end of the current trading session.
strategy.risk.max_intraday_filled_orders(count, alert_message) → void
EXAMPLE

//@version=5
strategy("risk.max_intraday_filled_orders Demo")
strategy.risk.max_intraday_filled_orders(10) // After 10 orders are filled, no more strategy orders will be placed (except for a market order to exit current open market position, if there is any).
if open > close
    strategy.entry("buy", strategy.long)
if open < close
    strategy.entry("sell", strategy.short)
ARGUMENTS
count (simple int) A required parameter. The maximum number of filled orders per 1 day.
alert_message (simple string) An optional parameter which replaces the {{strategy.order.alert_message}} placeholder when it is used in the "Create Alert" dialog box's "Message" field.
strategy.risk.max_intraday_loss

The maximum loss value allowed during a day. It is specified either in money (base currency), or in percentage of maximum intraday equity (0 -100).
strategy.risk.max_intraday_loss(value, type, alert_message) → void
EXAMPLE

// Sets the maximum intraday loss using the strategy's equity value.
//@version=5
strategy("strategy.risk.max_intraday_loss Example 1", overlay = false, default_qty_type = strategy.percent_of_equity, default_qty_value = 100)

// Input for maximum intraday loss %. 
lossPct = input.float(10)

// Set maximum intraday loss to our lossPct input
strategy.risk.max_intraday_loss(lossPct, strategy.percent_of_equity)

// Enter Short at bar_index zero.
if bar_index == 0
    strategy.entry("Short", strategy.short)

// Store equity value from the beginning of the day
eqFromDayStart = ta.valuewhen(ta.change(dayofweek) > 0, strategy.equity, 0)

// Calculate change of the current equity from the beginning of the current day.
eqChgPct = 100 * ((strategy.equity - eqFromDayStart) / strategy.equity)

// Plot it
plot(eqChgPct) 
hline(-lossPct)
EXAMPLE

// Sets the maximum intraday loss using the strategy's cash value.
//@version=5
strategy("strategy.risk.max_intraday_loss Example 2", overlay = false)

// Input for maximum intraday loss in absolute cash value of the symbol. 
absCashLoss = input.float(5)

// Set maximum intraday loss to `absCashLoss` in account currency.
strategy.risk.max_intraday_loss(absCashLoss, strategy.cash)

// Enter Short at bar_index zero.
if bar_index == 0
    strategy.entry("Short", strategy.short)

// Store the open price value from the beginning of the day.
beginPrice = ta.valuewhen(ta.change(dayofweek) > 0, open, 0)

// Calculate the absolute price change for the current period.
priceChg = (close - beginPrice)

hline(absCashLoss)
plot(priceChg)
ARGUMENTS
value (simple int/float) A required parameter. The maximum loss value. It is specified either in money (base currency), or in percentage of maximum intraday equity. For % of equity the range of allowed values is from 0 to 100.
type (simple string) A required parameter. The type of the value. Please specify one of the following values: strategy.percent_of_equity or strategy.cash. Note: if equity drops down to zero or to a negative and the strategy.percent_of_equity is specified, all pending orders are cancelled, all open positions are closed and no new orders can be placed for good.
alert_message (simple string) An optional parameter which replaces the {{strategy.order.alert_message}} placeholder when it is used in the "Create Alert" dialog box's "Message" field.
SEE ALSO
strategy
strategy.risk.max_position_size

The purpose of this rule is to determine maximum size of a market position. The rule affects the following function: strategy.entry. The 'entry' quantity can be reduced (if needed) to such number of contracts/shares/lots/units, so the total position size doesn't exceed the value specified in 'strategy.risk.max_position_size'. If minimum possible quantity still violates the rule, the order will not be placed.
strategy.risk.max_position_size(contracts) → void
EXAMPLE

//@version=5
strategy("risk.max_position_size Demo", default_qty_value = 100)
strategy.risk.max_position_size(10)
if open > close
    strategy.entry("buy", strategy.long)
plot(strategy.position_size)  // max plot value will be 10
ARGUMENTS
contracts (simple int/float) A required parameter. Maximum number of contracts/shares/lots/units in a position.
string

Casts na to string
string(x) → const string
string(x) → input string
string(x) → simple string
string(x) → series string
RETURNS
The value of the argument after casting to string.
SEE ALSO
float
int
bool
color
line
label
syminfo.prefix

Returns exchange prefix of the `symbol`, e.g. "NASDAQ".
syminfo.prefix(symbol) → simple string
syminfo.prefix(symbol) → series string
EXAMPLE

//@version=5
indicator("syminfo.prefix fun", overlay=true)
i_sym = input.symbol("NASDAQ:AAPL")
pref = syminfo.prefix(i_sym)
tick = syminfo.ticker(i_sym)
t = ticker.new(pref, tick, session.extended)
s = request.security(t, "1D", close)
plot(s)
RETURNS
Returns exchange prefix of the `symbol`, e.g. "NASDAQ".
ARGUMENTS
symbol (series string) Symbol. Note that the symbol should be passed with a prefix. For example: "NASDAQ:AAPL" instead of "AAPL".
REMARKS
The result of the function is used in the ticker.new/ticker.modify and request.security.
SEE ALSO
syminfo.tickerid
syminfo.ticker
syminfo.prefix
syminfo.ticker
ticker.new
syminfo.ticker

Returns `symbol` name without exchange prefix, e.g. "AAPL".
syminfo.ticker(symbol) → simple string
syminfo.ticker(symbol) → series string
EXAMPLE

//@version=5
indicator("syminfo.ticker fun", overlay=true) 
i_sym = input.symbol("NASDAQ:AAPL")
pref = syminfo.prefix(i_sym)
tick = syminfo.ticker(i_sym)
t = ticker.new(pref, tick, session.extended)
s = request.security(t, "1D", close)
plot(s)
RETURNS
Returns `symbol` name without exchange prefix, e.g. "AAPL".
ARGUMENTS
symbol (series string) Symbol. Note that the symbol should be passed with a prefix. For example: "NASDAQ:AAPL" instead of "AAPL".
REMARKS
The result of the function is used in the ticker.new/ticker.modify and request.security.
SEE ALSO
syminfo.tickerid
syminfo.ticker
syminfo.prefix
syminfo.prefix
ticker.new
ta.alma

Arnaud Legoux Moving Average. It uses Gaussian distribution as weights for moving average.
ta.alma(series, length, offset, sigma) → series float
ta.alma(series, length, offset, sigma, floor) → series float
EXAMPLE

//@version=5
indicator("ta.alma", overlay=true) 
plot(ta.alma(close, 9, 0.85, 6))

// same on pine, but much less efficient
pine_alma(series, windowsize, offset, sigma) =>
    m = offset * (windowsize - 1)
    //m = math.floor(offset * (windowsize - 1)) // Used as m when math.floor=true
    s = windowsize / sigma
    norm = 0.0
    sum = 0.0
    for i = 0 to windowsize - 1
        weight = math.exp(-1 * math.pow(i - m, 2) / (2 * math.pow(s, 2)))
        norm := norm + weight
        sum := sum + series[windowsize - i - 1] * weight
    sum / norm
plot(pine_alma(close, 9, 0.85, 6))
RETURNS
Arnaud Legoux Moving Average.
ARGUMENTS
series (series int/float) Series of values to process.
length (series int) Number of bars (length).
offset (simple int/float) Controls tradeoff between smoothness (closer to 1) and responsiveness (closer to 0).
sigma (simple int/float) Changes the smoothness of ALMA. The larger sigma the smoother ALMA.
floor (simple bool) An optional parameter. Specifies whether the offset calculation is floored before ALMA is calculated. Default value is false.
REMARKS
`na` values in the `source` series are included in calculations and will produce an `na` result.
SEE ALSO
ta.sma
ta.ema
ta.rma
ta.wma
ta.vwma
ta.swma
ta.atr

Function atr (average true range) returns the RMA of true range. True range is max(high - low, abs(high - close[1]), abs(low - close[1])).
ta.atr(length) → series float
EXAMPLE

//@version=5
indicator("ta.atr")
plot(ta.atr(14))

//the same on pine
pine_atr(length) =>
    trueRange = na(high[1])? high-low : math.max(math.max(high - low, math.abs(high - close[1])), math.abs(low - close[1]))
    //true range can be also calculated with ta.tr(true)
    ta.rma(trueRange, length)

plot(pine_atr(14))
RETURNS
Average true range.
ARGUMENTS
length (simple int) Length (number of bars back).
REMARKS
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
SEE ALSO
ta.tr
ta.rma
ta.barssince

Counts the number of bars since the last time the condition was true.
ta.barssince(condition) → series int
EXAMPLE

//@version=5
indicator("ta.barssince")
// get number of bars since last color.green bar
plot(ta.barssince(close >= open))
RETURNS
Number of bars since condition was true.
REMARKS
If the condition has never been met prior to the current bar, the function returns na.
Please note that using this variable/function can cause indicator repainting.
SEE ALSO
ta.lowestbars
ta.highestbars
ta.valuewhen
ta.highest
ta.lowest
ta.bb

Bollinger Bands. A Bollinger Band is a technical analysis tool defined by a set of lines plotted two standard deviations (positively and negatively) away from a simple moving average (SMA) of the security's price, but can be adjusted to user preferences.
ta.bb(series, length, mult) → [series float, series float, series float]
EXAMPLE

//@version=5
indicator("ta.bb")

[middle, upper, lower] = ta.bb(close, 5, 4)
plot(middle, color=color.yellow)
plot(upper, color=color.yellow)
plot(lower, color=color.yellow)

// the same on pine
f_bb(src, length, mult) =>
    float basis = ta.sma(src, length)
    float dev = mult * ta.stdev(src, length)
    [basis, basis + dev, basis - dev]

[pineMiddle, pineUpper, pineLower] = f_bb(close, 5, 4)

plot(pineMiddle)
plot(pineUpper)
plot(pineLower)
RETURNS
Bollinger Bands.
ARGUMENTS
series (series int/float) Series of values to process.
length (series int) Number of bars (length).
mult (simple int/float) Standard deviation factor.
REMARKS
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
SEE ALSO
ta.sma
ta.stdev
ta.kc
ta.bbw

Bollinger Bands Width. The Bollinger Band Width is the difference between the upper and the lower Bollinger Bands divided by the middle band.
ta.bbw(series, length, mult) → series float
EXAMPLE

//@version=5
indicator("ta.bbw")

plot(ta.bbw(close, 5, 4), color=color.yellow)

// the same on pine
f_bbw(src, length, mult) =>
    float basis = ta.sma(src, length)
    float dev = mult * ta.stdev(src, length)
    ((basis + dev) - (basis - dev)) / basis

plot(f_bbw(close, 5, 4))
RETURNS
Bollinger Bands Width.
ARGUMENTS
series (series int/float) Series of values to process.
length (series int) Number of bars (length).
mult (simple int/float) Standard deviation factor.
REMARKS
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
SEE ALSO
ta.bb
ta.sma
ta.stdev
ta.cci

The CCI (commodity channel index) is calculated as the difference between the typical price of a commodity and its simple moving average, divided by the mean absolute deviation of the typical price. The index is scaled by an inverse factor of 0.015 to provide more readable numbers.
ta.cci(source, length) → series float
RETURNS
Commodity channel index of source for length bars back.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
`na` values in the `source` series are ignored.
ta.change

Compares the current `source` value to its value `length` bars ago and returns the difference.
ta.change(source) → series float
ta.change(source) → series bool
ta.change(source) → series int
ta.change(source, length) → series float
ta.change(source, length) → series bool
ta.change(source, length) → series int
EXAMPLE

//@version=5
indicator('Day and Direction Change', overlay = true)
dailyBarTime = time('1D')
isNewDay = ta.change(dailyBarTime)
bgcolor(isNewDay ? color.new(color.green, 80) : na)

isGreenBar = close >= open
colorChange = ta.change(isGreenBar)
plotshape(colorChange, 'Direction Change')
RETURNS
The difference between the values when they are numerical. When a 'bool' source is used, returns `true` when the current source is different from the previous source.
ARGUMENTS
source (series int/float/bool) Source series.
length (series int) How far the past `source` value is offset from the current one, in bars. Optional. The default is 1.
REMARKS
`na` values in the `source` series are included in calculations and will produce an `na` result.
SEE ALSO
ta.mom
ta.cross
ta.cmo

Chande Momentum Oscillator. Calculates the difference between the sum of recent gains and the sum of recent losses and then divides the result by the sum of all price movement over the same period.
ta.cmo(series, length) → series float
EXAMPLE

//@version=5
indicator("ta.cmo")
plot(ta.cmo(close, 5), color=color.yellow)

// the same on pine
f_cmo(src, length) =>
    float mom = ta.change(src)
    float sm1 = math.sum((mom >= 0) ? mom : 0.0, length)
    float sm2 = math.sum((mom >= 0) ? 0.0 : -mom, length)
    100 * (sm1 - sm2) / (sm1 + sm2)

plot(f_cmo(close, 5))
RETURNS
Chande Momentum Oscillator.
ARGUMENTS
series (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
`na` values in the `source` series are ignored.
SEE ALSO
ta.rsi
ta.stoch
math.sum
ta.cog

The cog (center of gravity) is an indicator based on statistics and the Fibonacci golden ratio.
ta.cog(source, length) → series float
EXAMPLE

//@version=5
indicator("ta.cog", overlay=true) 
plot(ta.cog(close, 10))

// the same on pine
pine_cog(source, length) =>
    sum = math.sum(source, length)
    num = 0.0
    for i = 0 to length - 1
        price = source[i]
        num := num + price * (i + 1)
    -num / sum

plot(pine_cog(close, 10))
RETURNS
Center of Gravity.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
`na` values in the `source` series are ignored.
SEE ALSO
ta.stoch
ta.correlation

Correlation coefficient. Describes the degree to which two series tend to deviate from their ta.sma values.
ta.correlation(source1, source2, length) → series float
RETURNS
Correlation coefficient.
ARGUMENTS
source1 (series int/float) Source series.
source2 (series int/float) Target series.
length (series int) Length (number of bars back).
REMARKS
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
SEE ALSO
request.security
ta.cross

ta.cross(source1, source2) → series bool
RETURNS
true if two series have crossed each other, otherwise false.
ARGUMENTS
source1 (series int/float) First data series.
source2 (series int/float) Second data series.
SEE ALSO
ta.change
ta.crossover

The `source1`-series is defined as having crossed over `source2`-series if, on the current bar, the value of `source1` is greater than the value of `source2`, and on the previous bar, the value of `source1` was less than or equal to the value of `source2`.
ta.crossover(source1, source2) → series bool
RETURNS
true if `source1` crossed over `source2` otherwise false.
ARGUMENTS
source1 (series int/float) First data series.
source2 (series int/float) Second data series.
ta.crossunder

The `source1`-series is defined as having crossed under `source2`-series if, on the current bar, the value of `source1` is less than the value of `source2`, and on the previous bar, the value of `source1` was greater than or equal to the value of `source2`.
ta.crossunder(source1, source2) → series bool
RETURNS
true if `source1` crossed under `source2` otherwise false.
ARGUMENTS
source1 (series int/float) First data series.
source2 (series int/float) Second data series.
ta.cum

Cumulative (total) sum of `source`. In other words it's a sum of all elements of `source`.
ta.cum(source) → series float
RETURNS
Total sum series.
ARGUMENTS
source (series int/float) Source used for the calculation.
SEE ALSO
math.sum
ta.dev

Measure of difference between the series and it's ta.sma
ta.dev(source, length) → series float
EXAMPLE

//@version=5
indicator("ta.dev")
plot(ta.dev(close, 10))

// the same on pine
pine_dev(source, length) =>
    mean = ta.sma(source, length)
    sum = 0.0
    for i = 0 to length - 1
        val = source[i]
        sum := sum + math.abs(val - mean)
    dev = sum/length
plot(pine_dev(close, 10))
RETURNS
Deviation of `source` for `length` bars back.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
`na` values in the `source` series are ignored.
SEE ALSO
ta.variance
ta.stdev
ta.dmi

The dmi function returns the directional movement index.
ta.dmi(diLength, adxSmoothing) → [series float, series float, series float]
EXAMPLE

//@version=5
indicator(title="Directional Movement Index", shorttitle="DMI", format=format.price, precision=4)
len = input.int(17, minval=1, title="DI Length")
lensig = input.int(14, title="ADX Smoothing", minval=1, maxval=50)
[diplus, diminus, adx] = ta.dmi(len, lensig)
plot(adx, color=color.red, title="ADX")
plot(diplus, color=color.blue, title="+DI")
plot(diminus, color=color.orange, title="-DI")
RETURNS
Tuple of three DMI series: Positive Directional Movement (+DI), Negative Directional Movement (-DI) and Average Directional Movement Index (ADX).
ARGUMENTS
diLength (simple int) DI Period.
adxSmoothing (simple int) ADX Smoothing Period.
SEE ALSO
ta.rsi
ta.tsi
ta.mfi
ta.ema

The ema function returns the exponentially weighted moving average. In ema weighting factors decrease exponentially. It calculates by using a formula: EMA = alpha * source + (1 - alpha) * EMA[1], where alpha = 2 / (length + 1).
ta.ema(source, length) → series float
EXAMPLE

//@version=5
indicator("ta.ema")
plot(ta.ema(close, 15))

//the same on pine
pine_ema(src, length) =>
    alpha = 2 / (length + 1)
    sum = 0.0
    sum := na(sum[1]) ? src : alpha * src + (1 - alpha) * nz(sum[1])
plot(pine_ema(close,15))
RETURNS
Exponential moving average of `source` with alpha = 2 / (length + 1).
ARGUMENTS
source (series int/float) Series of values to process.
length (simple int) Number of bars (length).
REMARKS
Please note that using this variable/function can cause indicator repainting.
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
SEE ALSO
ta.sma
ta.rma
ta.wma
ta.vwma
ta.swma
ta.alma
ta.falling

Test if the `source` series is now falling for `length` bars long.
ta.falling(source, length) → series bool
RETURNS
true if current `source` value is less than any previous `source` value for `length` bars back, false otherwise.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
SEE ALSO
ta.rising
ta.highest

Highest value for a given number of bars back.
ta.highest(source, length) → series float
ta.highest(length) → series float
RETURNS
Highest value in the series.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
Two args version: `source` is a series and `length` is the number of bars back.
One arg version: `length` is the number of bars back. Algorithm uses high as a `source` series.
`na` values in the `source` series are ignored.
SEE ALSO
ta.lowest
ta.lowestbars
ta.highestbars
ta.valuewhen
ta.barssince
ta.highestbars

Highest value offset for a given number of bars back.
ta.highestbars(source, length) → series int
ta.highestbars(length) → series int
RETURNS
Offset to the highest bar.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
Two args version: `source` is a series and `length` is the number of bars back.
One arg version: `length` is the number of bars back. Algorithm uses high as a `source` series.
`na` values in the `source` series are ignored.
SEE ALSO
ta.lowest
ta.highest
ta.lowestbars
ta.barssince
ta.valuewhen
ta.hma

The hma function returns the Hull Moving Average.
ta.hma(source, length) → series float
EXAMPLE

//@version=5
indicator("Hull Moving Average")
src = input(defval=close, title="Source")
length = input(defval=9, title="Length")
hmaBuildIn = ta.hma(src, length)
plot(hmaBuildIn, title="Hull MA", color=#674EA7)
RETURNS
Hull moving average of 'source' for 'length' bars back.
ARGUMENTS
source (series int/float) Series of values to process.
length (simple int) Number of bars.
REMARKS
`na` values in the `source` series are ignored.
SEE ALSO
ta.ema
ta.rma
ta.wma
ta.vwma
ta.sma
ta.kc

Keltner Channels. Keltner channel is a technical analysis indicator showing a central moving average line plus channel lines at a distance above and below.
ta.kc(series, length, mult) → [series float, series float, series float]
ta.kc(series, length, mult, useTrueRange) → [series float, series float, series float]
EXAMPLE

//@version=5
indicator("ta.kc")

[middle, upper, lower] = ta.kc(close, 5, 4)
plot(middle, color=color.yellow)
plot(upper, color=color.yellow)
plot(lower, color=color.yellow)


// the same on pine
f_kc(src, length, mult, useTrueRange) =>
    float basis = ta.ema(src, length)
    float span = (useTrueRange) ? ta.tr : (high - low)
    float rangeEma = ta.ema(span, length)
    [basis, basis + rangeEma * mult, basis - rangeEma * mult]
    
[pineMiddle, pineUpper, pineLower] = f_kc(close, 5, 4, true)

plot(pineMiddle)
plot(pineUpper)
plot(pineLower)
RETURNS
Keltner Channels.
ARGUMENTS
series (series int/float) Series of values to process.
length (simple int) Number of bars (length).
mult (simple int/float) Standard deviation factor.
useTrueRange (simple bool) An optional parameter. Specifies if True Range is used; default is true. If the value is false, the range will be calculated with the expression (high - low).
REMARKS
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
SEE ALSO
ta.ema
ta.atr
ta.bb
ta.kcw

Keltner Channels Width. The Keltner Channels Width is the difference between the upper and the lower Keltner Channels divided by the middle channel.
ta.kcw(series, length, mult) → series float
ta.kcw(series, length, mult, useTrueRange) → series float
EXAMPLE

//@version=5
indicator("ta.kcw")

plot(ta.kcw(close, 5, 4), color=color.yellow)

// the same on pine
f_kcw(src, length, mult, useTrueRange) =>
    float basis = ta.ema(src, length)
    float span = (useTrueRange) ? ta.tr : (high - low)
    float rangeEma = ta.ema(span, length)
    
    ((basis + rangeEma * mult) - (basis - rangeEma * mult)) / basis

plot(f_kcw(close, 5, 4, true))
RETURNS
Keltner Channels Width.
ARGUMENTS
series (series int/float) Series of values to process.
length (simple int) Number of bars (length).
mult (simple int/float) Standard deviation factor.
useTrueRange (simple bool) An optional parameter. Specifies if True Range is used; default is true. If the value is false, the range will be calculated with the expression (high - low).
REMARKS
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
SEE ALSO
ta.kc
ta.ema
ta.atr
ta.bb
ta.linreg

Linear regression curve. A line that best fits the prices specified over a user-defined time period. It is calculated using the least squares method. The result of this function is calculated using the formula: linreg = intercept + slope * (length - 1 - offset), where intercept and slope are the values calculated with the least squares method on `source` series.
ta.linreg(source, length, offset) → series float
RETURNS
Linear regression curve.
ARGUMENTS
source (series int/float) Source series.
length (series int) Number of bars (length).
offset (simple int) Offset.
REMARKS
`na` values in the `source` series are included in calculations and will produce an `na` result.
ta.lowest

Lowest value for a given number of bars back.
ta.lowest(source, length) → series float
ta.lowest(length) → series float
RETURNS
Lowest value in the series.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
Two args version: `source` is a series and `length` is the number of bars back.
One arg version: `length` is the number of bars back. Algorithm uses low as a `source` series.
`na` values in the `source` series are ignored.
SEE ALSO
ta.highest
ta.lowestbars
ta.highestbars
ta.valuewhen
ta.barssince
ta.lowestbars

Lowest value offset for a given number of bars back.
ta.lowestbars(source, length) → series int
ta.lowestbars(length) → series int
RETURNS
Offset to the lowest bar.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars back.
REMARKS
Two args version: `source` is a series and `length` is the number of bars back.
One arg version: `length` is the number of bars back. Algorithm uses low as a `source` series.
`na` values in the `source` series are ignored.
SEE ALSO
ta.lowest
ta.highest
ta.highestbars
ta.barssince
ta.valuewhen
ta.macd

MACD (moving average convergence/divergence). It is supposed to reveal changes in the strength, direction, momentum, and duration of a trend in a stock's price.
ta.macd(source, fastlen, slowlen, siglen) → [series float, series float, series float]
EXAMPLE

//@version=5
indicator("MACD")
[macdLine, signalLine, histLine] = ta.macd(close, 12, 26, 9)
plot(macdLine, color=color.blue)
plot(signalLine, color=color.orange)
plot(histLine, color=color.red, style=plot.style_histogram)
If you need only one value, use placeholders '_' like this:
EXAMPLE

//@version=5
indicator("MACD")
[_, signalLine, _] = ta.macd(close, 12, 26, 9)
plot(signalLine, color=color.orange)
RETURNS
Tuple of three MACD series: MACD line, signal line and histogram line.
ARGUMENTS
source (series int/float) Series of values to process.
fastlen (simple int) Fast Length parameter.
slowlen (simple int) Slow Length parameter.
siglen (simple int) Signal Length parameter.
REMARKS
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
SEE ALSO
ta.sma
ta.ema
ta.max

Returns the all-time high value of `source` from the beginning of the chart up to the current bar.
ta.max(source) → series float
ARGUMENTS
source (series int/float) Source used for the calculation.
REMARKS
na occurrences of `source` are ignored.
ta.median

Returns the median of the series.
ta.median(source, length) → series float
ta.median(source, length) → series int
RETURNS
The median of the series.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
ta.mfi

Money Flow Index. The Money Flow Index (MFI) is a technical oscillator that uses price and volume for identifying overbought or oversold conditions in an asset.
ta.mfi(series, length) → series float
EXAMPLE

//@version=5
indicator("Money Flow Index")

plot(ta.mfi(hlc3, 14), color=color.yellow)

// the same on pine
pine_mfi(src, length) =>
    float upper = math.sum(volume * (ta.change(src) <= 0.0 ? 0.0 : src), length)
    float lower = math.sum(volume * (ta.change(src) >= 0.0 ? 0.0 : src), length)
    mfi = 100.0 - (100.0 / (1.0 + upper / lower))
    mfi

plot(pine_mfi(hlc3, 14))
RETURNS
Money Flow Index.
ARGUMENTS
series (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
SEE ALSO
ta.rsi
math.sum
ta.min

Returns the all-time low value of `source` from the beginning of the chart up to the current bar.
ta.min(source) → series float
ARGUMENTS
source (series int/float) Source used for the calculation.
REMARKS
na occurrences of `source` are ignored.
ta.mode

Returns the mode) of the series. If there are several values with the same frequency, it returns the smallest value.
ta.mode(source, length) → series float
ta.mode(source, length) → series int
RETURNS
The most frequently occurring value from the `source`. If none exists, returns the smallest value instead.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
ta.mom

Momentum of `source` price and `source` price `length` bars ago. This is simply a difference: source - source[length].
ta.mom(source, length) → series float
RETURNS
Momentum of `source` price and `source` price `length` bars ago.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Offset from the current bar to the previous bar.
REMARKS
`na` values in the `source` series are included in calculations and will produce an `na` result.
SEE ALSO
ta.change
ta.percentile_linear_interpolation

Calculates percentile using method of linear interpolation between the two nearest ranks.
ta.percentile_linear_interpolation(source, length, percentage) → series float
RETURNS
P-th percentile of `source` series for `length` bars back.
ARGUMENTS
source (series int/float) Series of values to process (source).
length (series int) Number of bars back (length).
percentage (simple int/float) Percentage, a number from range 0..100.
REMARKS
Note that a percentile calculated using this method will NOT always be a member of the input data set.
`na` values in the `source` series are included in calculations and will produce an `na` result.
SEE ALSO
ta.percentile_nearest_rank
ta.percentile_nearest_rank

Calculates percentile using method of Nearest Rank.
ta.percentile_nearest_rank(source, length, percentage) → series float
RETURNS
P-th percentile of `source` series for `length` bars back.
ARGUMENTS
source (series int/float) Series of values to process (source).
length (series int) Number of bars back (length).
percentage (simple int/float) Percentage, a number from range 0..100.
REMARKS
Using the Nearest Rank method on lengths less than 100 bars back can result in the same number being used for more than one percentile.
A percentile calculated using the Nearest Rank method will always be a member of the input data set.
The 100th percentile is defined to be the largest value in the input data set.
`na` values in the `source` series are ignored.
SEE ALSO
ta.percentile_linear_interpolation
ta.percentrank

Percent rank is the percents of how many previous values was less than or equal to the current value of given series.
ta.percentrank(source, length) → series float
RETURNS
Percent rank of `source` for `length` bars back.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
`na` values in the `source` series are included in calculations and will produce an `na` result.
ta.pivot_point_levels

Calculates the pivot point levels using the specified `type` and `anchor`.
ta.pivot_point_levels(type, anchor, developing) → float[]
EXAMPLE

//@version=5
indicator("Weekly Pivots", max_lines_count=500, overlay=true)
timeframe = "1W"
typeInput = input.string("Traditional", "Type", options=["Traditional", "Fibonacci", "Woodie", "Classic", "DM", "Camarilla"])
weekChange = timeframe.change(timeframe)
pivotPointsArray = ta.pivot_point_levels(typeInput, weekChange)
if weekChange
    for pivotLevel in pivotPointsArray
        line.new(time, pivotLevel, time + timeframe.in_seconds(timeframe) * 1000, pivotLevel, xloc=xloc.bar_time)
RETURNS
A float[] array with numerical values representing 11 pivot point levels: [P, R1, S1, R2, S2, R3, S3, R4, S4, R5, S5]. Levels absent from the specified `type` return na values (e.g., "DM" only calculates P, R1, and S1).
ARGUMENTS
type (series string) The type of pivot point levels. Possible values: "Traditional", "Fibonacci", "Woodie", "Classic", "DM", "Camarilla".
anchor (series bool) The condition that triggers the reset of the pivot point calculations. When true, calculations reset; when false, results calculated at the last reset persist.
developing (series bool) If false, the values are those calculated the last time the anchor condition was true. They remain constant until the anchor condition becomes true again. If true, the pivots are developing, i.e., they constantly recalculate on the data developing between the point of the last anchor (or bar zero if the anchor condition was never true) and the current bar. Optional. The default is false.
REMARKS
The `developing` parameter cannot be `true` when `type` is set to "Woodie", because the Woodie calculation for a period depends on that period's open, which means that the pivot value is either available or unavailable, but never developing. If used together, the indicator will return a runtime error.
ta.pivothigh

This function returns price of the pivot high point. It returns 'NaN', if there was no pivot high point.
ta.pivothigh(source, leftbars, rightbars) → series float
ta.pivothigh(leftbars, rightbars) → series float
EXAMPLE

//@version=5
indicator("PivotHigh", overlay=true)
leftBars = input(2)
rightBars=input(2)
ph = ta.pivothigh(leftBars, rightBars)
plot(ph, style=plot.style_cross, linewidth=3, color= color.red, offset=-rightBars)
RETURNS
Price of the point or 'NaN'.
ARGUMENTS
source (series int/float) An optional parameter. Data series to calculate the value. 'High' by default.
leftbars (series int/float) Left strength.
rightbars (series int/float) Right strength.
REMARKS
If parameters 'leftbars' or 'rightbars' are series you should use max_bars_back function for the 'source' variable.
ta.pivotlow

This function returns price of the pivot low point. It returns 'NaN', if there was no pivot low point.
ta.pivotlow(source, leftbars, rightbars) → series float
ta.pivotlow(leftbars, rightbars) → series float
EXAMPLE

//@version=5
indicator("PivotLow", overlay=true)
leftBars = input(2)
rightBars=input(2)
pl = ta.pivotlow(close, leftBars, rightBars)
plot(pl, style=plot.style_cross, linewidth=3, color= color.blue, offset=-rightBars)
RETURNS
Price of the point or 'NaN'.
ARGUMENTS
source (series int/float) An optional parameter. Data series to calculate the value. 'Low' by default.
leftbars (series int/float) Left strength.
rightbars (series int/float) Right strength.
REMARKS
If parameters 'leftbars' or 'rightbars' are series you should use max_bars_back function for the 'source' variable.
ta.range

Returns the difference between the min and max values in a series.
ta.range(source, length) → series float
ta.range(source, length) → series int
RETURNS
The difference between the min and max values in the series.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
ta.rising

Test if the `source` series is now rising for `length` bars long.
ta.rising(source, length) → series bool
RETURNS
true if current `source` is greater than any previous `source` for `length` bars back, false otherwise.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
`na` values in the `source` series are ignored.
SEE ALSO
ta.falling
ta.rma

Moving average used in RSI. It is the exponentially weighted moving average with alpha = 1 / length.
ta.rma(source, length) → series float
EXAMPLE

//@version=5
indicator("ta.rma")
plot(ta.rma(close, 15))

//the same on pine
pine_rma(src, length) =>
    alpha = 1/length
    sum = 0.0
    sum := na(sum[1]) ? ta.sma(src, length) : alpha * src + (1 - alpha) * nz(sum[1])
plot(pine_rma(close, 15))
RETURNS
Exponential moving average of `source` with alpha = 1 / `length`.
ARGUMENTS
source (series int/float) Series of values to process.
length (simple int) Number of bars (length).
REMARKS
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
SEE ALSO
ta.sma
ta.ema
ta.wma
ta.vwma
ta.swma
ta.alma
ta.rsi
ta.roc

Calculates the percentage of change (rate of change) between the current value of `source` and its value `length` bars ago.
It is calculated by the formula: 100 * change(src, length) / src[length].
ta.roc(source, length) → series float
RETURNS
The rate of change of `source` for `length` bars back.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
`na` values in the `source` series are included in calculations and will produce an `na` result.
ta.rsi

Relative strength index. It is calculated using the `ta.rma()` of upward and downward changes of `source` over the last `length` bars.
ta.rsi(source, length) → series float
EXAMPLE

//@version=5
indicator("ta.rsi")
plot(ta.rsi(close, 7))

// same on pine, but less efficient
pine_rsi(x, y) => 
    u = math.max(x - x[1], 0) // upward ta.change
    d = math.max(x[1] - x, 0) // downward ta.change
    rs = ta.rma(u, y) / ta.rma(d, y)
    res = 100 - 100 / (1 + rs)
    res

plot(pine_rsi(close, 7))
RETURNS
Relative strength index.
ARGUMENTS
source (series int/float) Series of values to process.
length (simple int) Number of bars (length).
REMARKS
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
SEE ALSO
ta.rma
ta.sar

Parabolic SAR (parabolic stop and reverse) is a method devised by J. Welles Wilder, Jr., to find potential reversals in the market price direction of traded goods.
ta.sar(start, inc, max) → series float
EXAMPLE

//@version=5
indicator("ta.sar")
plot(ta.sar(0.02, 0.02, 0.2), style=plot.style_cross, linewidth=3)

// The same on Pine Script™
pine_sar(start, inc, max) =>
    var float result = na
    var float maxMin = na
    var float acceleration = na
    var bool isBelow = na
    bool isFirstTrendBar = false
    
    if bar_index == 1
        if close > close[1]
            isBelow := true
            maxMin := high
            result := low[1]
        else
            isBelow := false
            maxMin := low
            result := high[1]
        isFirstTrendBar := true
        acceleration := start
    
    result := result + acceleration * (maxMin - result)
    
    if isBelow
        if result > low
            isFirstTrendBar := true
            isBelow := false
            result := math.max(high, maxMin)
            maxMin := low
            acceleration := start
    else
        if result < high
            isFirstTrendBar := true
            isBelow := true
            result := math.min(low, maxMin)
            maxMin := high
            acceleration := start
            
    if not isFirstTrendBar
        if isBelow
            if high > maxMin
                maxMin := high
                acceleration := math.min(acceleration + inc, max)
        else
            if low < maxMin
                maxMin := low
                acceleration := math.min(acceleration + inc, max)
    
    if isBelow
        result := math.min(result, low[1])
        if bar_index > 1
            result := math.min(result, low[2])
        
    else
        result := math.max(result, high[1])
        if bar_index > 1
            result := math.max(result, high[2])
    
    result
    
plot(pine_sar(0.02, 0.02, 0.2), style=plot.style_cross, linewidth=3)
RETURNS
Parabolic SAR.
ARGUMENTS
start (simple int/float) Start.
inc (simple int/float) Increment.
max (simple int/float) Maximum.
ta.sma

The sma function returns the moving average, that is the sum of last y values of x, divided by y.
ta.sma(source, length) → series float
EXAMPLE

//@version=5
indicator("ta.sma")
plot(ta.sma(close, 15))

// same on pine, but much less efficient
pine_sma(x, y) =>
    sum = 0.0
    for i = 0 to y - 1
        sum := sum + x[i] / y
    sum
plot(pine_sma(close, 15))
RETURNS
Simple moving average of `source` for `length` bars back.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
`na` values in the `source` series are ignored.
SEE ALSO
ta.ema
ta.rma
ta.wma
ta.vwma
ta.swma
ta.alma
ta.stdev

ta.stdev(source, length, biased) → series float
EXAMPLE

//@version=5
indicator("ta.stdev")
plot(ta.stdev(close, 5))

//the same on pine
isZero(val, eps) => math.abs(val) <= eps

SUM(fst, snd) =>
    EPS = 1e-10
    res = fst + snd
    if isZero(res, EPS)
        res := 0
    else
        if not isZero(res, 1e-4)
            res := res
        else
            15

pine_stdev(src, length) =>
    avg = ta.sma(src, length)
    sumOfSquareDeviations = 0.0
    for i = 0 to length - 1
        sum = SUM(src[i], -avg)
        sumOfSquareDeviations := sumOfSquareDeviations + sum * sum

    stdev = math.sqrt(sumOfSquareDeviations / length)
plot(pine_stdev(close, 5))
RETURNS
Standard deviation.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
biased (series bool) Determines which estimate should be used. Optional. The default is true.
REMARKS
If `biased` is true, function will calculate using a biased estimate of the entire population, if false - unbiased estimate of a sample.
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
SEE ALSO
ta.dev
ta.variance
ta.stoch

Stochastic. It is calculated by a formula: 100 * (close - lowest(low, length)) / (highest(high, length) - lowest(low, length)).
ta.stoch(source, high, low, length) → series float
RETURNS
Stochastic.
ARGUMENTS
source (series int/float) Source series.
high (series int/float) Series of high.
low (series int/float) Series of low.
length (series int) Length (number of bars back).
REMARKS
`na` values in the `source` series are ignored.
SEE ALSO
ta.cog
ta.supertrend

The Supertrend Indicator. The Supertrend is a trend following indicator.
ta.supertrend(factor, atrPeriod) → [series float, series float]
EXAMPLE

//@version=5
indicator("Pine Script™ Supertrend")

[supertrend, direction] = ta.supertrend(3, 10)
plot(direction < 0 ? supertrend : na, "Up direction", color = color.green, style=plot.style_linebr)
plot(direction > 0 ? supertrend : na, "Down direction", color = color.red, style=plot.style_linebr)

// The same on Pine Script™
pine_supertrend(factor, atrPeriod) =>
    src = hl2
    atr = ta.atr(atrPeriod)
    upperBand = src + factor * atr
    lowerBand = src - factor * atr
    prevLowerBand = nz(lowerBand[1])
    prevUpperBand = nz(upperBand[1])

    lowerBand := lowerBand > prevLowerBand or close[1] < prevLowerBand ? lowerBand : prevLowerBand
    upperBand := upperBand < prevUpperBand or close[1] > prevUpperBand ? upperBand : prevUpperBand
    int direction = na
    float superTrend = na
    prevSuperTrend = superTrend[1]
    if na(atr[1])
        direction := 1
    else if prevSuperTrend == prevUpperBand
        direction := close > upperBand ? -1 : 1
    else
        direction := close < lowerBand ? 1 : -1
    superTrend := direction == -1 ? lowerBand : upperBand
    [superTrend, direction]

[pineSupertrend, pineDirection] = pine_supertrend(3, 10)
plot(pineDirection < 0 ? pineSupertrend : na, "Up direction", color = color.green, style=plot.style_linebr)
plot(pineDirection > 0 ? pineSupertrend : na, "Down direction", color = color.red, style=plot.style_linebr)
RETURNS
Tuple of two supertrend series: supertrend line and direction of trend. Possible values are 1 (down direction) and -1 (up direction).
ARGUMENTS
factor (series int/float) The multiplier by which the ATR will get multiplied.
atrPeriod (simple int) Length of ATR.
SEE ALSO
ta.macd
ta.swma

Symmetrically weighted moving average with fixed length: 4. Weights: [1/6, 2/6, 2/6, 1/6].
ta.swma(source) → series float
EXAMPLE

//@version=5
indicator("ta.swma")
plot(ta.swma(close))

// same on pine, but less efficient
pine_swma(x) =>
    x[3] * 1 / 6 + x[2] * 2 / 6 + x[1] * 2 / 6 + x[0] * 1 / 6
plot(pine_swma(close))
RETURNS
Symmetrically weighted moving average.
ARGUMENTS
source (series int/float) Source series.
REMARKS
`na` values in the `source` series are included in calculations and will produce an `na` result.
SEE ALSO
ta.sma
ta.ema
ta.rma
ta.wma
ta.vwma
ta.alma
ta.tr

ta.tr(handle_na) → series float
RETURNS
True range. It is math.max(high - low, math.abs(high - close[1]), math.abs(low - close[1])).
ARGUMENTS
handle_na (simple bool) How NaN values are handled. if true, and previous day's close is NaN then tr would be calculated as current day high-low. Otherwise (if false) tr would return NaN in such cases. Also note, that ta.atr uses ta.tr(true).
REMARKS
ta.tr(false) is exactly the same as ta.tr.
SEE ALSO
ta.tr
ta.atr
ta.tsi

True strength index. It uses moving averages of the underlying momentum of a financial instrument.
ta.tsi(source, short_length, long_length) → series float
RETURNS
True strength index. A value in range [-1, 1].
ARGUMENTS
source (series int/float) Source series.
short_length (simple int) Short length.
long_length (simple int) Long length.
REMARKS
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
ta.valuewhen

Returns the value of the `source` series on the bar where the `condition` was true on the nth most recent occurrence.
ta.valuewhen(condition, source, occurrence) → series float
ta.valuewhen(condition, source, occurrence) → series int
ta.valuewhen(condition, source, occurrence) → series bool
ta.valuewhen(condition, source, occurrence) → series color
EXAMPLE

//@version=5
indicator("ta.valuewhen")
slow = ta.sma(close, 7)
fast = ta.sma(close, 14)
// Get value of `close` on second most recent cross
plot(ta.valuewhen(ta.cross(slow, fast), close, 1))
ARGUMENTS
condition (series bool) The condition to search for.
source (series int/float/bool/color) The value to be returned from the bar where the condition is met.
occurrence (simple int) The occurrence of the condition. The numbering starts from 0 and goes back in time, so '0' is the most recent occurrence of `condition`, '1' is the second most recent and so forth. Must be an integer >= 0.
REMARKS
This function requires execution on every bar. It is not recommended to use it inside a for or while loop structure, where its behavior can be unexpected. Please note that using this function can cause indicator repainting.
SEE ALSO
ta.lowestbars
ta.highestbars
ta.barssince
ta.highest
ta.lowest
ta.variance

Variance is the expectation of the squared deviation of a series from its mean (ta.sma), and it informally measures how far a set of numbers are spread out from their mean.
ta.variance(source, length, biased) → series float
RETURNS
Variance of `source` for `length` bars back.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
biased (series bool) Determines which estimate should be used. Optional. The default is true.
REMARKS
If `biased` is true, function will calculate using a biased estimate of the entire population, if false - unbiased estimate of a sample.
`na` values in the `source` series are ignored; the function calculates on the `length` quantity of non-`na` values.
SEE ALSO
ta.dev
ta.stdev
ta.vwap

Volume weighted average price.
ta.vwap(source) → series float
ta.vwap(source, anchor) → series float
ta.vwap(source, anchor, stdev_mult) → [series float, series float, series float]
EXAMPLE

//@version=5
indicator("Simple VWAP")
vwap = ta.vwap(open)
plot(vwap)
EXAMPLE

//@version=5
indicator("Advanced VWAP")
vwapAnchorInput = input.string("Daily", "Anchor", options = ["Daily", "Weekly", "Monthly"])
stdevMultiplierInput = input.float(1.0, "Standard Deviation Multiplier")
anchorTimeframe = switch vwapAnchorInput
    "Daily"   => "1D"
    "Weekly"  => "1W"
    "Monthly" => "1M"
anchor = timeframe.change(anchorTimeframe)
[vwap, upper, lower] = ta.vwap(open, anchor, stdevMultiplierInput)
plot(vwap)
plot(upper, color = color.green)
plot(lower, color = color.green)
RETURNS
A VWAP series, or a tuple [vwap, upper_band, lower_band] if `stdev_mult` is specified.
ARGUMENTS
source (series int/float) Source used for the VWAP calculation.
anchor (series bool) The condition that triggers the reset of VWAP calculations. When true, calculations reset; when false, calculations proceed using the values accumulated since the previous reset. Optional. The default is equivalent to passing timeframe.change with "1D" as its argument.
stdev_mult (series int/float) If specified, the function will calculate the standard deviation bands based on the main VWAP series and return a [vwap, upper_band, lower_band] tuple. The `upper_band`/`lower_band` values are calculated using the VWAP to which the standard deviation multiplied by this argument is added/subtracted. Optional. The default is na, in which case the function returns a single value, not a tuple.
REMARKS
Calculations only begin the first time the anchor condition becomes true. Until then, the function returns na.
SEE ALSO
ta.vwap
ta.vwma

The vwma function returns volume-weighted moving average of `source` for `length` bars back. It is the same as: sma(source * volume, length) / sma(volume, length).
ta.vwma(source, length) → series float
EXAMPLE

//@version=5
indicator("ta.vwma")
plot(ta.vwma(close, 15))

// same on pine, but less efficient
pine_vwma(x, y) =>
    ta.sma(x * volume, y) / ta.sma(volume, y)
plot(pine_vwma(close, 15))
RETURNS
Volume-weighted moving average of `source` for `length` bars back.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
`na` values in the `source` series are ignored.
SEE ALSO
ta.sma
ta.ema
ta.rma
ta.wma
ta.swma
ta.alma
ta.wma

The wma function returns weighted moving average of `source` for `length` bars back. In wma weighting factors decrease in arithmetical progression.
ta.wma(source, length) → series float
EXAMPLE

//@version=5
indicator("ta.wma")
plot(ta.wma(close, 15))

// same on pine, but much less efficient
pine_wma(x, y) =>
    norm = 0.0
    sum = 0.0
    for i = 0 to y - 1
        weight = (y - i) * y
        norm := norm + weight
        sum := sum + x[i] * weight
    sum / norm
plot(pine_wma(close, 15))
RETURNS
Weighted moving average of `source` for `length` bars back.
ARGUMENTS
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
REMARKS
`na` values in the `source` series are ignored.
SEE ALSO
ta.sma
ta.ema
ta.rma
ta.vwma
ta.swma
ta.alma
ta.wpr

Williams %R. The oscillator shows the current closing price in relation to the high and low of the past 'length' bars.
ta.wpr(length) → series float
EXAMPLE

//@version=5
indicator("Williams %R", shorttitle="%R", format=format.price, precision=2)
plot(ta.wpr(14), title="%R", color=color.new(#ff6d00, 0))
RETURNS
Williams %R.
ARGUMENTS
length (series int) Number of bars.
REMARKS
`na` values in the `source` series are ignored.
SEE ALSO
ta.mfi
ta.cmo
table

Casts na to table
table(x) → series table
RETURNS
The value of the argument after casting to table.
SEE ALSO
float
int
bool
color
string
line
label
table.cell

The function defines a cell in the table and sets its attributes.
table.cell(table_id, column, row, text, width, height, text_color, text_halign, text_valign, text_size, bgcolor, tooltip, text_font_family) → void
ARGUMENTS
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
text (series string) The text to be displayed inside the cell. Optional. The default is empty string.
text_font_family (series string) The font family of the text. Optional. The default value is font.family_default. Possible values: font.family_default, font.family_monospace.
width (series int/float) The width of the cell as a % of the indicator's visual space. Optional. By default, auto-adjusts the width based on the text inside the cell. Value 0 has the same effect.
height (series int/float) The height of the cell as a % of the indicator's visual space. Optional. By default, auto-adjusts the height based on the text inside of the cell. Value 0 has the same effect.
text_color (series color) The color of the text. Optional. The default is color.black.
text_halign (series string) The horizontal alignment of the cell's text. Optional. The default value is text.align_center. Possible values: text.align_left, text.align_center, text.align_right.
text_valign (series string) The vertical alignment of the cell's text. Optional. The default value is text.align_center. Possible values: text.align_top, text.align_center, text.align_bottom.
text_size (series string) The size of the text. An optional parameter, the default value is size.normal. Possible values: size.auto, size.tiny, size.small, size.normal, size.large, size.huge.
bgcolor (series color) The background color of the text. Optional. The default is no color.
tooltip (series string) The tooltip to be displayed inside the cell. Optional.
REMARKS
This function does not create the table itself, but defines the table’s cells. To use it, you first need to create a table object with table.new.
Each table.cell call overwrites all previously defined properties of a cell. If you call table.cell twice in a row, e.g., the first time with text='Test Text', and the second time with text_color=color.red but without a new text argument, the default value of the 'text' being an empty string, it will overwrite 'Test Text', and your cell will display an empty string. If you want, instead, to modify any of the cell's properties, use the table.cell_set_*() functions.
A single script can only display one table in each of the possible locations. If table.cell is used on several bars to change the same attribute of a cell (e.g. change the background color of the cell to red on the first bar, then to yellow on the second bar), only the last change will be reflected in the table, i.e., the cell’s background will be yellow. Avoid unnecessary setting of cell properties by enclosing function calls in an if barstate.islast block whenever possible, to restrict their execution to the last bar of the series.
SEE ALSO
table.cell_set_bgcolor
table.cell_set_height
table.cell_set_text
table.cell_set_text_color
table.cell_set_text_halign
table.cell_set_text_size
table.cell_set_text_valign
table.cell_set_width
table.cell_set_tooltip
table.cell_set_bgcolor

The function sets the background color of the cell.
table.cell_set_bgcolor(table_id, column, row, bgcolor) → void
ARGUMENTS
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
bgcolor (series color) The background color of the cell.
SEE ALSO
table.cell_set_height
table.cell_set_text
table.cell_set_text_color
table.cell_set_text_halign
table.cell_set_text_size
table.cell_set_text_valign
table.cell_set_width
table.cell_set_tooltip
table.cell_set_height

The function sets the height of cell.
table.cell_set_height(table_id, column, row, height) → void
ARGUMENTS
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
height (series int/float) The height of the cell as a % of the chart window. Passing 0 auto-adjusts the height based on the text inside of the cell.
SEE ALSO
table.cell_set_bgcolor
table.cell_set_text
table.cell_set_text_color
table.cell_set_text_halign
table.cell_set_text_size
table.cell_set_text_valign
table.cell_set_width
table.cell_set_tooltip
table.cell_set_text

The function sets the text in the specified cell.
table.cell_set_text(table_id, column, row, text) → void
EXAMPLE

//@version=5
indicator("TABLE example")
var tLog = table.new(position = position.top_left, rows = 1, columns = 2, bgcolor = color.yellow, border_width=1)
table.cell(tLog, row = 0, column = 0, text = "sometext", text_color = color.blue)
table.cell_set_text(tLog, row = 0, column = 0, text = "sometext")
ARGUMENTS
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
text (series string) The text to be displayed inside the cell.
SEE ALSO
table.cell_set_bgcolor
table.cell_set_height
table.cell_set_text_color
table.cell_set_text_halign
table.cell_set_text_size
table.cell_set_text_valign
table.cell_set_width
table.cell_set_tooltip
table.cell_set_text_color

The function sets the color of the text inside the cell.
table.cell_set_text_color(table_id, column, row, text_color) → void
ARGUMENTS
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
text_color (series color) The color of the text.
SEE ALSO
table.cell_set_bgcolor
table.cell_set_height
table.cell_set_text
table.cell_set_text_halign
table.cell_set_text_size
table.cell_set_text_valign
table.cell_set_width
table.cell_set_tooltip
table.cell_set_text_font_family

The function sets the font family of the text inside the cell.
table.cell_set_text_font_family(table_id, column, row, text_font_family) → void
EXAMPLE

//@version=5
indicator("Example of setting the table cell font")
var t = table.new(position.top_left, rows = 1, columns = 1)
table.cell(t, 0, 0, "monospace", text_color = color.blue)
table.cell_set_text_font_family(t, 0, 0, font.family_monospace)
ARGUMENTS
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
text_font_family (series string) The font family of the text. Possible values: font.family_default, font.family_monospace.
SEE ALSO
table.new
font.family_default
font.family_monospace
table.cell_set_text_halign

The function sets the horizontal alignment of the cell's text.
table.cell_set_text_halign(table_id, column, row, text_halign) → void
ARGUMENTS
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
text_halign (series string) The horizontal alignment of a cell's text. Possible values: text.align_left, text.align_center, text.align_right.
SEE ALSO
table.cell_set_bgcolor
table.cell_set_height
table.cell_set_text
table.cell_set_text_color
table.cell_set_text_size
table.cell_set_text_valign
table.cell_set_width
table.cell_set_tooltip
table.cell_set_text_size

The function sets the size of the cell's text.
table.cell_set_text_size(table_id, column, row, text_size) → void
ARGUMENTS
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
text_size (series string) The size of the text. Possible values: size.auto, size.tiny, size.small, size.normal, size.large, size.huge.
SEE ALSO
table.cell_set_bgcolor
table.cell_set_height
table.cell_set_text
table.cell_set_text_color
table.cell_set_text_halign
table.cell_set_text_valign
table.cell_set_width
table.cell_set_tooltip
table.cell_set_text_valign

The function sets the vertical alignment of a cell's text.
table.cell_set_text_valign(table_id, column, row, text_valign) → void
ARGUMENTS
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
text_valign (series string) The vertical alignment of the cell's text. Possible values: text.align_top, text.align_center, text.align_bottom.
SEE ALSO
table.cell_set_bgcolor
table.cell_set_height
table.cell_set_text
table.cell_set_text_color
table.cell_set_text_halign
table.cell_set_text_size
table.cell_set_width
table.cell_set_tooltip
table.cell_set_tooltip

The function sets the tooltip in the specified cell.
table.cell_set_tooltip(table_id, column, row, tooltip) → void
EXAMPLE

//@version=5
indicator("TABLE example")
var tLog = table.new(position = position.top_left, rows = 1, columns = 2, bgcolor = color.yellow, border_width=1)
table.cell(tLog, row = 0, column = 0, text = "sometext", text_color = color.blue)
table.cell_set_tooltip(tLog, row = 0, column = 0, tooltip = "sometext")
ARGUMENTS
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
tooltip (series string) The tooltip to be displayed inside the cell.
SEE ALSO
table.cell_set_bgcolor
table.cell_set_height
table.cell_set_text_color
table.cell_set_text_halign
table.cell_set_text_size
table.cell_set_text_valign
table.cell_set_width
table.cell_set_text
table.cell_set_width

The function sets the width of the cell.
table.cell_set_width(table_id, column, row, width) → void
ARGUMENTS
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
width (series int/float) The width of the cell as a % of the chart window. Passing 0 auto-adjusts the width based on the text inside of the cell.
SEE ALSO
table.cell_set_bgcolor
table.cell_set_height
table.cell_set_text
table.cell_set_text_color
table.cell_set_text_halign
table.cell_set_text_size
table.cell_set_text_valign
table.cell_set_tooltip
table.clear

The function removes a cell or a sequence of cells from the table. The cells are removed in a rectangle shape where the start_column and start_row specify the top-left corner, and end_column and end_row specify the bottom-right corner.
table.clear(table_id, start_column, start_row, end_column, end_row) → void
EXAMPLE

//@version=5
indicator("A donut", overlay=true)
if barstate.islast
    colNum = 8, rowNum = 8
    padding = "◯"
    donutTable = table.new(position.middle_right, colNum, rowNum)
    for c = 0 to colNum - 1
        for r = 0 to rowNum - 1
            table.cell(donutTable, c, r, text=padding, bgcolor=#face6e, text_color=color.new(color.black, 100))
    table.clear(donutTable, 2, 2, 5, 5)
ARGUMENTS
table_id (series table) A table object.
start_column (series int) The index of the column of the first cell to delete. Numbering starts at 0.
start_row (series int) The index of the row of the first cell to delete. Numbering starts at 0.
end_column (series int) The index of the column of the last cell to delete. Optional. The default is the argument used for start_column. Numbering starts at 0.
end_row (series int) The index of the row of the last cell to delete. Optional. The default is the argument used for start_row. Numbering starts at 0.
SEE ALSO
table.delete
table.new
table.delete

The function deletes a table.
table.delete(table_id) → void
EXAMPLE

//@version=5
indicator("table.delete example")
var testTable = table.new(position = position.top_right, columns = 2, rows = 1, bgcolor = color.yellow, border_width = 1)
if barstate.islast
    table.cell(table_id = testTable, column = 0, row = 0, text = "Open is " + str.tostring(open))
    table.cell(table_id = testTable, column = 1, row = 0, text = "Close is " + str.tostring(close), bgcolor=color.teal)
if barstate.isrealtime
    table.delete(testTable)
ARGUMENTS
table_id (series table) A table object.
SEE ALSO
table.new
table.clear
table.merge_cells

The function merges a sequence of cells in the table into one cell. The cells are merged in a rectangle shape where the start_column and start_row specify the top-left corner, and end_column and end_row specify the bottom-right corner.
table.merge_cells(table_id, start_column, start_row, end_column, end_row) → void
EXAMPLE

//@version=5
indicator("table.merge_cells example")
SMA50  = ta.sma(close, 50)
SMA100 = ta.sma(close, 100)
SMA200 = ta.sma(close, 200)
if barstate.islast
    maTable = table.new(position.bottom_right, 3, 3, bgcolor = color.gray, border_width = 1, border_color = color.black)
    // Header
    table.cell(maTable, 0, 0, text = "SMA Table")
    table.merge_cells(maTable, 0, 0, 2, 0)
    // Cell Titles
    table.cell(maTable, 0, 1, text = "SMA 50")
    table.cell(maTable, 1, 1, text = "SMA 100")
    table.cell(maTable, 2, 1, text = "SMA 200")
    // Values
    table.cell(maTable, 0, 2, bgcolor = color.white, text = str.tostring(SMA50))
    table.cell(maTable, 1, 2, bgcolor = color.white, text = str.tostring(SMA100))
    table.cell(maTable, 2, 2, bgcolor = color.white, text = str.tostring(SMA200))
ARGUMENTS
table_id (series table) A table object.
start_column (series int) The index of the column of the first cell to merge. Numbering starts at 0.
start_row (series int) The index of the row of the first cell to merge. Numbering starts at 0.
end_column (series int) The index of the column of the last cell to merge. Numbering starts at 0.
end_row (series int) The index of the row of the last cell to merge. Numbering starts at 0.
REMARKS
This function will merge cells, even if their properties are not yet defined with table.cell.
The resulting merged cell inherits all of its values from the cell located at `start_column`:`start_row`, except width and height. The width and height of the resulting merged cell are based on the width/height of other cells in the neighboring columns/rows and cannot be set manually.
To modify the merged cell with any of the `table.cell_set_*` functions, target the cell at the `start_column`:`start_row` coordinates.
An attempt to merge a cell that has already been merged will result in an error.
SEE ALSO
table.delete
table.new
table.new

The function creates a new table.
table.new(position, columns, rows, bgcolor, frame_color, frame_width, border_color, border_width) → series table
EXAMPLE

//@version=5
indicator("table.new example")
var testTable = table.new(position = position.top_right, columns = 2, rows = 1, bgcolor = color.yellow, border_width = 1)
if barstate.islast
    table.cell(table_id = testTable, column = 0, row = 0, text = "Open is " + str.tostring(open))
    table.cell(table_id = testTable, column = 1, row = 0, text = "Close is " + str.tostring(close), bgcolor=color.teal)
RETURNS
The ID of a table object that can be passed to other table.*() functions.
ARGUMENTS
position (series string) Position of the table. Possible values are: position.top_left, position.top_center, position.top_right, position.middle_left, position.middle_center, position.middle_right, position.bottom_left, position.bottom_center, position.bottom_right.
columns (series int) The number of columns in the table.
rows (series int) The number of rows in the table.
bgcolor (series color) The background color of the table. Optional. The default is no color.
frame_color (series color) The color of the outer frame of the table. Optional. The default is no color.
frame_width (series int) The width of the outer frame of the table. Optional. The default is 0.
border_color (series color) The color of the borders of the cells (excluding the outer frame). Optional. The default is no color.
border_width (series int) The width of the borders of the cells (excluding the outer frame). Optional. The default is 0.
REMARKS
This function creates the table object itself, but the table will not be displayed until its cells are populated. To define a cell and change its contents or attributes, use table.cell and other table.cell_*() functions.
One table.new call can only display one table (the last one drawn), but the function itself will be recalculated on each bar it is used on. For performance reasons, it is wise to use table.new in conjunction with either the var keyword (so the table object is only created on the first bar) or in an if barstate.islast block (so the table object is only created on the last bar).
SEE ALSO
table.cell
table.clear
table.delete
table.set_bgcolor
table.set_border_color
table.set_border_width
table.set_frame_color
table.set_frame_width
table.set_position
table.set_bgcolor

The function sets the background color of a table.
table.set_bgcolor(table_id, bgcolor) → void
ARGUMENTS
table_id (series table) A table object.
bgcolor (series color) The background color of the table. Optional. The default is no color.
SEE ALSO
table.clear
table.delete
table.new
table.set_border_color
table.set_border_width
table.set_frame_color
table.set_frame_width
table.set_position
table.set_border_color

The function sets the color of the borders (excluding the outer frame) of the table's cells.
table.set_border_color(table_id, border_color) → void
ARGUMENTS
table_id (series table) A table object.
border_color (series color) The color of the borders. Optional. The default is no color.
SEE ALSO
table.clear
table.delete
table.new
table.set_frame_color
table.set_border_width
table.set_bgcolor
table.set_frame_width
table.set_position
table.set_border_width

The function sets the width of the borders (excluding the outer frame) of the table's cells.
table.set_border_width(table_id, border_width) → void
ARGUMENTS
table_id (series table) A table object.
border_width (series int) The width of the borders. Optional. The default is 0.
SEE ALSO
table.clear
table.delete
table.new
table.set_frame_color
table.set_frame_width
table.set_bgcolor
table.set_border_color
table.set_position
table.set_frame_color

The function sets the color of the outer frame of a table.
table.set_frame_color(table_id, frame_color) → void
ARGUMENTS
table_id (series table) A table object.
frame_color (series color) The color of the frame of the table. Optional. The default is no color.
SEE ALSO
table.clear
table.delete
table.new
table.set_border_color
table.set_border_width
table.set_bgcolor
table.set_frame_width
table.set_position
table.set_frame_width

The function set the width of the outer frame of a table.
table.set_frame_width(table_id, frame_width) → void
ARGUMENTS
table_id (series table) A table object.
frame_width (series int) The width of the outer frame of the table. Optional. The default is 0.
SEE ALSO
table.clear
table.delete
table.new
table.set_frame_color
table.set_border_width
table.set_bgcolor
table.set_border_color
table.set_position
table.set_position

The function sets the position of a table.
table.set_position(table_id, position) → void
ARGUMENTS
table_id (series table) A table object.
position (series string) Position of the table. Possible values are: position.top_left, position.top_center, position.top_right, position.middle_left, position.middle_center, position.middle_right, position.bottom_left, position.bottom_center, position.bottom_right.
SEE ALSO
table.clear
table.delete
table.new
table.set_bgcolor
table.set_border_color
table.set_border_width
table.set_frame_color
table.set_frame_width
ticker.heikinashi

Creates a ticker identifier for requesting Heikin Ashi bar values.
ticker.heikinashi(symbol) → simple string
EXAMPLE

//@version=5
indicator("ticker.heikinashi", overlay=true) 
heikinashi_close = request.security(ticker.heikinashi(syminfo.tickerid), timeframe.period, close)

heikinashi_aapl_60_close = request.security(ticker.heikinashi("AAPL"), "60", close)
plot(heikinashi_close)
plot(heikinashi_aapl_60_close)
RETURNS
String value of ticker id, that can be supplied to request.security function.
ARGUMENTS
symbol (simple string) Symbol ticker identifier.
SEE ALSO
syminfo.tickerid
syminfo.ticker
request.security
ticker.renko
ticker.linebreak
ticker.kagi
ticker.pointfigure
ticker.kagi

Creates a ticker identifier for requesting Kagi values.
ticker.kagi(symbol, reversal) → simple string
EXAMPLE

//@version=5
indicator("ticker.kagi", overlay=true) 
kagi_tickerid = ticker.kagi(syminfo.tickerid, 3)
kagi_close = request.security(kagi_tickerid, timeframe.period, close)
plot(kagi_close)
RETURNS
String value of ticker id, that can be supplied to request.security function.
ARGUMENTS
symbol (simple string) Symbol ticker identifier.
reversal (simple int/float) Reversal amount (absolute price value).
SEE ALSO
syminfo.tickerid
syminfo.ticker
request.security
ticker.heikinashi
ticker.renko
ticker.linebreak
ticker.pointfigure
ticker.linebreak

Creates a ticker identifier for requesting Line Break values.
ticker.linebreak(symbol, number_of_lines) → simple string
EXAMPLE

//@version=5
indicator("ticker.linebreak", overlay=true) 
linebreak_tickerid = ticker.linebreak(syminfo.tickerid, 3)
linebreak_close = request.security(linebreak_tickerid, timeframe.period, close)
plot(linebreak_close)
RETURNS
String value of ticker id, that can be supplied to request.security function.
ARGUMENTS
symbol (simple string) Symbol ticker identifier.
number_of_lines (simple int) Number of line.
SEE ALSO
syminfo.tickerid
syminfo.ticker
request.security
ticker.heikinashi
ticker.renko
ticker.kagi
ticker.pointfigure
ticker.modify

Creates a ticker identifier for requesting additional data for the script.
ticker.modify(tickerid, session, adjustment) → simple string
EXAMPLE

//@version=5
indicator("ticker_modify", overlay=true)
t1 = ticker.new(syminfo.prefix, syminfo.ticker, session.regular, adjustment.splits)
c1 = request.security(t1, "D", close)
t2 = ticker.modify(t1, session.extended)
c2 = request.security(t2, "2D", close)
plot(c1)
plot(c2)
RETURNS
String value of ticker id, that can be supplied to request.security function.
ARGUMENTS
tickerid (simple string) Symbol name with exchange prefix, e.g. 'BATS:MSFT', 'NASDAQ:MSFT' or tickerid with session and adjustment from the ticker.new function.
session (simple string) Session type. Optional argument. Possible values: session.regular, session.extended. Session type of the current chart is syminfo.session. If session is not given, then syminfo.session value is used.
adjustment (simple string) Adjustment type. Optional argument. Possible values: adjustment.none, adjustment.splits, adjustment.dividends. If adjustment is not given, then default adjustment value is used (can be different depending on particular instrument).
SEE ALSO
syminfo.tickerid
syminfo.ticker
syminfo.session
session.extended
session.regular
ticker.heikinashi
adjustment.none
adjustment.splits
adjustment.dividends
ticker.new

Creates a ticker identifier for requesting additional data for the script.
ticker.new(prefix, ticker, session, adjustment) → simple string
EXAMPLE

//@version=5
indicator("ticker.new", overlay=true) 
t = ticker.new(syminfo.prefix, syminfo.ticker, session.regular, adjustment.splits)
t2 = ticker.heikinashi(t)
c = request.security(t2, timeframe.period, low, barmerge.gaps_on)
plot(c, style=plot.style_linebr)
RETURNS
String value of ticker id, that can be supplied to request.security function.
ARGUMENTS
prefix (simple string) Exchange prefix. For example: 'BATS', 'NYSE', 'NASDAQ'. Exchange prefix of main series is syminfo.prefix.
ticker (simple string) Ticker name. For example 'AAPL', 'MSFT', 'EURUSD'. Ticker name of the main series is syminfo.ticker.
session (simple string) Session type. Optional argument. Possible values: session.regular, session.extended. Session type of the current chart is syminfo.session. If session is not given, then syminfo.session value is used.
adjustment (simple string) Adjustment type. Optional argument. Possible values: adjustment.none, adjustment.splits, adjustment.dividends. If adjustment is not given, then default adjustment value is used (can be different depending on particular instrument).
REMARKS
You may use return value of ticker.new function as input argument for ticker.heikinashi, ticker.renko, ticker.linebreak, ticker.kagi, ticker.pointfigure functions.
SEE ALSO
syminfo.tickerid
syminfo.ticker
syminfo.session
session.extended
session.regular
ticker.heikinashi
adjustment.none
adjustment.splits
adjustment.dividends
ticker.pointfigure

Creates a ticker identifier for requesting Point & Figure values.
ticker.pointfigure(symbol, source, style, param, reversal) → simple string
EXAMPLE

//@version=5
indicator("ticker.pointfigure", overlay=true) 
pnf_tickerid = ticker.pointfigure(syminfo.tickerid, "hl", "Traditional", 1, 3)
pnf_close = request.security(pnf_tickerid, timeframe.period, close)
plot(pnf_close)
RETURNS
String value of ticker id, that can be supplied to request.security function.
ARGUMENTS
symbol (simple string) Symbol ticker identifier.
source (simple string) The source for calculating Point & Figure. Possible values are: 'hl', 'close'.
style (simple string) Box Size Assignment Method: 'ATR', 'Traditional'.
param (simple int/float) ATR Length if `style` is equal to 'ATR', or Box Size if `style` is equal to 'Traditional'.
reversal (simple int) Reversal amount.
SEE ALSO
syminfo.tickerid
syminfo.ticker
request.security
ticker.heikinashi
ticker.renko
ticker.linebreak
ticker.kagi
ticker.renko

Creates a ticker identifier for requesting Renko values.
ticker.renko(symbol, style, param, request_wicks, source) → simple string
EXAMPLE

//@version=5
indicator("ticker.renko", overlay=true) 
renko_tickerid = ticker.renko(syminfo.tickerid, "ATR", 10)
renko_close = request.security(renko_tickerid, timeframe.period, close)
plot(renko_close)
EXAMPLE

//@version=5
indicator("Renko candles", overlay=false)
renko_tickerid = ticker.renko(syminfo.tickerid, "ATR", 10)
[renko_open, renko_high, renko_low, renko_close] = request.security(renko_tickerid, timeframe.period, [open, high, low, close])
plotcandle(renko_open, renko_high, renko_low, renko_close, color = renko_close > renko_open ? color.green : color.red)
RETURNS
String value of ticker id, that can be supplied to request.security function.
ARGUMENTS
symbol (simple string) Symbol ticker identifier.
style (simple string) Box Size Assignment Method: 'ATR', 'Traditional'.
param (simple int/float) ATR Length if `style` is equal to 'ATR', or Box Size if `style` is equal to 'Traditional'.
request_wicks (simple bool) Specifies if wick values are returned for Renko bricks. When " + addInternalLineNotTr("op", "true", "true") + ", " + addInternalLineNotTr("var", "high", "high") + " and " + addInternalLineNotTr("var", "low", "low") + " values requested from a symbol using the ticker formed by this function will include wick values when they are present. When " + addInternalLineNotTr("op", "false", "false") + ", " + addInternalLineNotTr("var", "high", "high") + " and " + addInternalLineNotTr("var", "low", "low") + " will always be equal to either " + addInternalLineNotTr("var", "open", "open") + " or " + addInternalLineNotTr("var", "close", "close") + ". Optional. The default is " + addInternalLineNotTr("op", "false", "false") + ". A detailed explanation of how Renko wicks are calculated can be found in our Help Center.
source (simple string) The source used to calculate bricks. Optional. Possible values: "Close", "OHLC". The default is "Close".
SEE ALSO
syminfo.tickerid
syminfo.ticker
request.security
ticker.heikinashi
ticker.linebreak
ticker.kagi
ticker.pointfigure
ticker.standard

Creates a ticker to request data from a standard chart that is unaffected by modifiers like extended session, dividend adjustment, currency conversion, and the calculations of non-standard chart types: Heikin Ashi, Renko, etc. Among other things, this makes it possible to retrieve standard chart values when the script is running on a non-standard chart.
ticker.standard(symbol) → simple string
EXAMPLE

//@version=5
indicator("ticker.standard", overlay = true)
// This script should be run on a non-standard chart such as HA, Renko...

// Requests data from the chart type the script is running on.
chartTypeValue = request.security(syminfo.tickerid, "1D", close)

// Request data from the standard chart type, regardless of the chart type the script is running on.
standardChartValue = request.security(ticker.standard(syminfo.tickerid), "1D", close)

// This will not use a standard ticker ID because the `symbol` argument contains only the ticker — not the prefix (exchange).
standardChartValue2 = request.security(ticker.standard(syminfo.ticker), "1D", close)

plot(chartTypeValue)
plot(standardChartValue, color = color.green)
RETURNS
A string representing the ticker of a standard chart in the "prefix:ticker" format. If the `symbol` argument does not contain the prefix and ticker information, the function returns the supplied argument as is.
ARGUMENTS
symbol (simple string) A ticker ID to be converted into its standard form. Optional. The default is syminfo.tickerid.
SEE ALSO
request.security
time

The time function returns the UNIX time of the current bar for the specified timeframe and session or NaN if the time point is out of session.
time(timeframe, session, timezone) → series int
time(timeframe, session) → series int
time(timeframe) → series int
EXAMPLE

//@version=5
indicator("Time", overlay=true)
// Try this on chart AAPL,1
timeinrange(res, sess) => not na(time(res, sess, "America/New_York")) ? 1 : 0
plot(timeinrange("1", "1300-1400"), color=color.red)

// This plots 1.0 at every start of 10 minute bar on a 1 minute chart:
newbar(res) => ta.change(time(res)) == 0 ? 0 : 1
plot(newbar("10"))
While setting up a session you can specify not just the hours and minutes but also the days of the week that will be included in that session.
If the days aren't specified, the session is considered to have been set from Sunday (1) to Saturday (7), i.e. "1100-2000" is the same as "1100-1200:1234567".
You can change that by specifying the days. For example, on a symbol that is traded seven days a week with the 24-hour trading session the following script will not color Saturdays and Sundays:
EXAMPLE

//@version=5
indicator("Time", overlay=true)
t1 = time(timeframe.period, "0000-0000:23456")
bgcolor(t1 ? color.new(color.blue, 90) : na)
One `session` argument can include several different sessions, separated by commas. For example, the following script will highlight the bars from 10:00 to 11:00 and from 14:00 to 15:00 (workdays only):
EXAMPLE

//@version=5
indicator("Time", overlay=true)
t1 = time(timeframe.period, "1000-1100,1400-1500:23456")
bgcolor(t1 ? color.new(color.blue, 90) : na)
RETURNS
UNIX time.
ARGUMENTS
timeframe (series string) Timeframe. An empty string is interpreted as the current timeframe of the chart.
session (series string) Session specification. Optional argument, session of the symbol is used by default. An empty string is interpreted as the session of the symbol.
timezone (series string) Timezone of the `session` argument. Can only be used when a `session` is specified. Optional. The default is syminfo.timezone. Can be specified in GMT notation (e.g. "GMT-5") or as an IANA time zone database name (e.g. "America/New_York").
REMARKS
UNIX time is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
SEE ALSO
time
time_close

The time_close function returns the UNIX time of the close of the current bar for the specified resolution and session or NaN if the time point is out of session.
time_close(timeframe, session, timezone) → series int
time_close(timeframe, session) → series int
time_close(timeframe) → series int
EXAMPLE

//@version=5
indicator("Time", overlay=true)
t1 = time_close(timeframe.period, "1200-1300", "America/New_York")
bgcolor(t1 ? color.new(color.blue, 90) : na)
RETURNS
UNIX time.
ARGUMENTS
timeframe (series string) Resolution. An empty string is interpreted as the current resolution of the chart.
session (series string) Session specification. Optional argument, session of the symbol is used by default. An empty string is interpreted as the session of the symbol.
timezone (series string) Timezone of the `session` argument. Can only be used when a `session` is specified. Optional. The default is syminfo.timezone. Can be specified in GMT notation (e.g. "GMT-5") or as an IANA time zone database name (e.g. "America/New_York").
REMARKS
UNIX time is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
SEE ALSO
time_close
timeframe.change

Detects changes in the specified `timeframe`.
timeframe.change(timeframe) → series bool
EXAMPLE

//@version=5
// Run this script on an intraday chart.
indicator("New day started", overlay = true)
// Highlights the first bar of the new day.
isNewDay = timeframe.change("1D")
bgcolor(isNewDay ? color.new(color.green, 80) : na)
RETURNS
Returns true on the first bar of a new `timeframe`, false otherwise.
ARGUMENTS
timeframe (series string) String formatted according to the User manual's timeframe string specifications.
timeframe.in_seconds

Converts the timeframe passed to the `timeframe` argument into seconds.
timeframe.in_seconds(timeframe) → simple int
timeframe.in_seconds(timeframe) → series int
EXAMPLE

//@version=5
indicator("timeframe_in_seconds")

// Get chart timeframe:
i_tf = input.timeframe("1D")

// Convert timeframe to the int value (number of seconds in 1 Day):
tf = timeframe.in_seconds(i_tf)

plot(tf)
RETURNS
An int representation of the number of seconds in one bar of a `timeframe`.
ARGUMENTS
timeframe (series string) Timeframe. Optional. The default is timeframe.period.
REMARKS
For the `timeframe` >= '1M' function calculates number of seconds based on 30.4167 (365/12) days in month.
SEE ALSO
input.timeframe
timeframe.period
timestamp

Function timestamp returns UNIX time of specified date and time.
timestamp(dateString) → const int
timestamp(year, month, day, hour, minute, second) → simple int
timestamp(timezone, year, month, day, hour, minute, second) → simple int
timestamp(year, month, day, hour, minute, second) → series int
timestamp(timezone, year, month, day, hour, minute, second) → series int
EXAMPLE

//@version=5
indicator("timestamp")
plot(timestamp(2016, 01, 19, 09, 30), linewidth=3, color=color.green)
plot(timestamp(syminfo.timezone, 2016, 01, 19, 09, 30), color=color.blue)
plot(timestamp(2016, 01, 19, 09, 30), color=color.yellow)
plot(timestamp("GMT+6", 2016, 01, 19, 09, 30))
plot(timestamp(2019, 06, 19, 09, 30, 15), color=color.lime)
plot(timestamp("GMT+3", 2019, 06, 19, 09, 30, 15), color=color.fuchsia)
plot(timestamp("Feb 01 2020 22:10:05"))
plot(timestamp("2011-10-10T14:48:00"))
plot(timestamp("04 Dec 1995 00:12:00 GMT+5"))
RETURNS
UNIX time.
ARGUMENTS
timezone (series string) Allows adjusting the returned value to a time zone specified in either UTC/GMT notation (e.g., "UTC-5", "GMT+0530") or as an IANA time zone database name (e.g., "America/New_York"). Optional. The default is syminfo.timezone.
year (series int) Year.
month (series int) Month.
day (series int) Day.
hour (series int) (Optional argument) Hour. Default is 0.
minute (series int) (Optional argument) Minute. Default is 0.
second (series int) (Optional argument) Second. Default is 0.
dateString (const string) A string containing the date and, optionally, the time and time zone. Its format must comply with either the IETF RFC 2822 or ISO 8601 standards ("DD MMM YYYY hh:mm:ss ±hhmm" or "YYYY-MM-DDThh:mm:ss±hh:mm", so "20 Feb 2020" or "2020-02-20"). If no time is supplied, "00:00" is used. If no time zone is supplied, GMT+0 will be used. Note that this diverges from the usual behavior of the function where it returns time in the exchange's timezone.
REMARKS
UNIX time is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
SEE ALSO
time
weekofyear

weekofyear(time) → series int
weekofyear(time, timezone) → series int
RETURNS
Week of year (in exchange timezone) for provided UNIX time.
ARGUMENTS
time (series int) UNIX time in milliseconds.
timezone (series string) Allows adjusting the returned value to a time zone specified in either UTC/GMT notation (e.g., "UTC-5", "GMT+0530") or as an IANA time zone database name (e.g., "America/New_York"). Optional. The default is syminfo.timezone.
REMARKS
UNIX time is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
Note that this function returns the week based on the time of the bar's open. For overnight sessions (e.g. EURUSD, where Monday session starts on Sunday, 17:00) this value can be lower by 1 than the week of the trading day.
SEE ALSO
weekofyear
time
year
month
dayofmonth
dayofweek
hour
minute
second
year

year(time) → series int
year(time, timezone) → series int
RETURNS
Year (in exchange timezone) for provided UNIX time.
ARGUMENTS
time (series int) UNIX time in milliseconds.
timezone (series string) Allows adjusting the returned value to a time zone specified in either UTC/GMT notation (e.g., "UTC-5", "GMT+0530") or as an IANA time zone database name (e.g., "America/New_York"). Optional. The default is syminfo.timezone.
REMARKS
UNIX time is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
Note that this function returns the year based on the time of the bar's open. For overnight sessions (e.g. EURUSD, where Monday session starts on Sunday, 17:00 UTC-4) this value can be lower by 1 than the year of the trading day.
SEE ALSO
year
time
month
dayofmonth
dayofweek
hour
minute
second