Built-In Variables
adjustment.dividends

Constant for dividends adjustment type (dividends adjustment is applied).
TYPE
const string
SEE ALSO
adjustment.none
adjustment.splits
ticker.new
adjustment.none

Constant for none adjustment type (no adjustment is applied).
TYPE
const string
SEE ALSO
adjustment.splits
adjustment.dividends
ticker.new
adjustment.splits

Constant for splits adjustment type (splits adjustment is applied).
TYPE
const string
SEE ALSO
adjustment.none
adjustment.dividends
ticker.new
alert.freq_all

A named constant for use with the `freq` parameter of the alert() function.
All function calls trigger the alert.
TYPE
const string
SEE ALSO
alert
alert.freq_once_per_bar

A named constant for use with the `freq` parameter of the alert() function.
The first function call during the bar triggers the alert.
TYPE
const string
SEE ALSO
alert
alert.freq_once_per_bar_close

A named constant for use with the `freq` parameter of the alert() function.
The function call triggers the alert only when it occurs during the last script iteration of the real-time bar, when it closes.
TYPE
const string
SEE ALSO
alert
bar_index

Current bar index. Numbering is zero-based, index of the first bar is 0.
TYPE
series int
EXAMPLE

//@version=5
indicator("bar_index")
plot(bar_index)
plot(bar_index > 5000 ? close : 0)
REMARKS
Note that bar_index has replaced n variable in version 4.
Note that bar indexing starts from 0 on the first historical bar.
Please note that using this variable/function can cause indicator repainting.
SEE ALSO
last_bar_index
barstate.isfirst
barstate.islast
barstate.isrealtime
barmerge.gaps_off

Merge strategy for requested data. Data is merged continuously without gaps, all the gaps are filled with the previous nearest existing value.
TYPE
const barmerge_gaps
SEE ALSO
request.security
barmerge.gaps_on
barmerge.gaps_on

Merge strategy for requested data. Data is merged with possible gaps (na values).
TYPE
const barmerge_gaps
SEE ALSO
request.security
barmerge.gaps_off
barmerge.lookahead_off

Merge strategy for the requested data position. Requested barset is merged with current barset in the order of sorting bars by their close time. This merge strategy disables effect of getting data from "future" on calculation on history.
TYPE
const barmerge_lookahead
SEE ALSO
request.security
barmerge.lookahead_on
barmerge.lookahead_on

Merge strategy for the requested data position. Requested barset is merged with current barset in the order of sorting bars by their opening time. This merge strategy can lead to undesirable effect of getting data from "future" on calculation on history. This is unacceptable in backtesting strategies, but can be useful in indicators.
TYPE
const barmerge_lookahead
SEE ALSO
request.security
barmerge.lookahead_off
barstate.isconfirmed

Returns true if the script is calculating the last (closing) update of the current bar. The next script calculation will be on the new bar data.
TYPE
series bool
REMARKS
PineScript code that uses this variable could calculate differently on history and real-time data.
It is NOT recommended to use barstate.isconfirmed in request.security expression. Its value requested from request.security is unpredictable.
Please note that using this variable/function can cause indicator repainting.
SEE ALSO
barstate.isfirst
barstate.islast
barstate.ishistory
barstate.isrealtime
barstate.isnew
barstate.islastconfirmedhistory
barstate.isfirst

Returns true if current bar is first bar in barset, false otherwise.
TYPE
series bool
REMARKS
PineScript code that uses this variable could calculate differently on history and real-time data.
Please note that using this variable/function can cause indicator repainting.
SEE ALSO
barstate.islast
barstate.ishistory
barstate.isrealtime
barstate.isnew
barstate.isconfirmed
barstate.islastconfirmedhistory
barstate.ishistory

Returns true if current bar is a historical bar, false otherwise.
TYPE
series bool
REMARKS
PineScript code that uses this variable could calculate differently on history and real-time data.
Please note that using this variable/function can cause indicator repainting.
SEE ALSO
barstate.isfirst
barstate.islast
barstate.isrealtime
barstate.isnew
barstate.isconfirmed
barstate.islastconfirmedhistory
barstate.islast

Returns true if current bar is the last bar in barset, false otherwise. This condition is true for all real-time bars in barset.
TYPE
series bool
REMARKS
PineScript code that uses this variable could calculate differently on history and real-time data.
Please note that using this variable/function can cause indicator repainting.
SEE ALSO
barstate.isfirst
barstate.ishistory
barstate.isrealtime
barstate.isnew
barstate.isconfirmed
barstate.islastconfirmedhistory
barstate.islastconfirmedhistory

Returns true if script is executing on the dataset's last bar when market is closed, or script is executing on the bar immediately preceding the real-time bar, if market is open. Returns false otherwise.
TYPE
series bool
REMARKS
PineScript code that uses this variable could calculate differently on history and real-time data.
Please note that using this variable/function can cause indicator repainting.
SEE ALSO
barstate.isfirst
barstate.islast
barstate.ishistory
barstate.isrealtime
barstate.isnew
barstate.isnew

Returns true if script is currently calculating on new bar, false otherwise. This variable is true when calculating on historical bars or on first update of a newly generated real-time bar.
TYPE
series bool
REMARKS
PineScript code that uses this variable could calculate differently on history and real-time data.
Please note that using this variable/function can cause indicator repainting.
SEE ALSO
barstate.isfirst
barstate.islast
barstate.ishistory
barstate.isrealtime
barstate.isconfirmed
barstate.islastconfirmedhistory
barstate.isrealtime

Returns true if current bar is a real-time bar, false otherwise.
TYPE
series bool
REMARKS
PineScript code that uses this variable could calculate differently on history and real-time data.
Please note that using this variable/function can cause indicator repainting.
SEE ALSO
barstate.isfirst
barstate.islast
barstate.ishistory
barstate.isnew
barstate.isconfirmed
barstate.islastconfirmedhistory
box.all

Returns an array filled with all the current boxes drawn by the script.
TYPE
box[]
EXAMPLE

//@version=5
indicator("box.all")
//delete all boxes
box.new(time, open, time + 60 * 60 * 24, close, xloc=xloc.bar_time, border_style=line.style_dashed)
a_allBoxes = box.all
if array.size(a_allBoxes) > 0
    for i = 0 to array.size(a_allBoxes) - 1
        box.delete(array.get(a_allBoxes, i))
REMARKS
The array is read-only. Index zero of the array is the ID of the oldest object on the chart.
SEE ALSO
box.new
line.all
label.all
table.all
chart.bg_color

Returns the color of the chart's background from the "Chart settings/Appearance/Background" field. When a gradient is selected, the middle point of the gradient is returned.
TYPE
input color
SEE ALSO
chart.fg_color
chart.fg_color

Returns a color providing optimal contrast with chart.bg_color.
TYPE
input color
SEE ALSO
chart.bg_color
chart.is_heikinashi

TYPE
simple bool
RETURNS
Returns true if the chart type is Heikin Ashi, false otherwise.
SEE ALSO
chart.is_renko
chart.is_linebreak
chart.is_kagi
chart.is_pnf
chart.is_range
chart.is_kagi

TYPE
simple bool
RETURNS
Returns true if the chart type is Kagi, false otherwise.
SEE ALSO
chart.is_renko
chart.is_linebreak
chart.is_heikinashi
chart.is_pnf
chart.is_range
chart.is_linebreak

TYPE
simple bool
RETURNS
Returns true if the chart type is Line break, false otherwise.
SEE ALSO
chart.is_renko
chart.is_heikinashi
chart.is_kagi
chart.is_pnf
chart.is_range
chart.is_pnf

TYPE
simple bool
RETURNS
Returns true if the chart type is Point & figure, false otherwise.
SEE ALSO
chart.is_renko
chart.is_linebreak
chart.is_kagi
chart.is_heikinashi
chart.is_range
chart.is_range

TYPE
simple bool
RETURNS
Returns true if the chart type is Range, false otherwise.
SEE ALSO
chart.is_renko
chart.is_linebreak
chart.is_kagi
chart.is_pnf
chart.is_heikinashi
chart.is_renko

TYPE
simple bool
RETURNS
Returns true if the chart type is Renko, false otherwise.
SEE ALSO
chart.is_heikinashi
chart.is_linebreak
chart.is_kagi
chart.is_pnf
chart.is_range
chart.is_standard

TYPE
simple bool
RETURNS
Returns true if the chart type is bars, candles, hollow candles, line, area or baseline, false otherwise.
SEE ALSO
chart.is_renko
chart.is_linebreak
chart.is_kagi
chart.is_pnf
chart.is_range
chart.is_heikinashi
chart.left_visible_bar_time

The time of the leftmost bar currently visible on the chart.
TYPE
input int
REMARKS
Scripts using this variable will automatically re-execute when its value updates to reflect changes in the chart, which can be caused by users scrolling the chart, or new real-time bars.
SEE ALSO
chart.right_visible_bar_time
chart.right_visible_bar_time

The time of the rightmost bar currently visible on the chart.
TYPE
input int
REMARKS
Scripts using this variable will automatically re-execute when its value updates to reflect changes in the chart, which can be caused by users scrolling the chart, or new real-time bars.
SEE ALSO
chart.left_visible_bar_time
close

Close price of the current bar when it has closed, or last traded price of a yet incomplete, realtime bar.
TYPE
series float
REMARKS
Previous values may be accessed with square brackets operator [], e.g. close[1], close[2].
SEE ALSO
open
high
low
volume
time
hl2
hlc3
hlcc4
ohlc4
color.aqua

Is a named constant for #00BCD4 color.
TYPE
const color
SEE ALSO
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.orange
color.black

Is a named constant for #363A45 color.
TYPE
const color
SEE ALSO
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.blue

Is a named constant for #2962ff color.
TYPE
const color
SEE ALSO
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.teal
color.aqua
color.orange
color.fuchsia

Is a named constant for #E040FB color.
TYPE
const color
SEE ALSO
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.gray

Is a named constant for #787B86 color.
TYPE
const color
SEE ALSO
color.black
color.silver
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.green

Is a named constant for #4CAF50 color.
TYPE
const color
SEE ALSO
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.lime

Is a named constant for #00E676 color.
TYPE
const color
SEE ALSO
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.maroon

Is a named constant for #880E4F color.
TYPE
const color
SEE ALSO
color.black
color.silver
color.gray
color.white
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.navy

Is a named constant for #311B92 color.
TYPE
const color
SEE ALSO
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.blue
color.teal
color.aqua
color.orange
color.olive

Is a named constant for #808000 color.
TYPE
const color
SEE ALSO
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.orange

Is a named constant for #FF9800 color.
TYPE
const color
SEE ALSO
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.purple

Is a named constant for #9C27B0 color.
TYPE
const color
SEE ALSO
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.red

Is a named constant for #FF5252 color.
TYPE
const color
SEE ALSO
color.black
color.silver
color.gray
color.white
color.maroon
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.silver

Is a named constant for #B2B5BE color.
TYPE
const color
SEE ALSO
color.black
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.teal

Is a named constant for #00897B color.
TYPE
const color
SEE ALSO
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.aqua
color.orange
color.white

Is a named constant for #FFFFFF color.
TYPE
const color
SEE ALSO
color.black
color.silver
color.gray
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.yellow

Is a named constant for #FFEB3B color.
TYPE
const color
SEE ALSO
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.navy
color.blue
color.teal
color.aqua
color.orange
currency.AUD

Australian dollar.
TYPE
const string
SEE ALSO
strategy
currency.BTC

Bitcoin.
TYPE
const string
SEE ALSO
strategy
currency.CAD

Canadian dollar.
TYPE
const string
SEE ALSO
strategy
currency.CHF

Swiss franc.
TYPE
const string
SEE ALSO
strategy
currency.ETH

Ethereum.
TYPE
const string
SEE ALSO
strategy
currency.EUR

Euro.
TYPE
const string
SEE ALSO
strategy
currency.GBP

Pound sterling.
TYPE
const string
SEE ALSO
strategy
currency.HKD

Hong Kong dollar.
TYPE
const string
SEE ALSO
strategy
currency.INR

Indian rupee.
TYPE
const string
SEE ALSO
strategy
currency.JPY

Japanese yen.
TYPE
const string
SEE ALSO
strategy
currency.KRW

South Korean won.
TYPE
const string
SEE ALSO
strategy
currency.MYR

Malaysian ringgit.
TYPE
const string
SEE ALSO
strategy
currency.NOK

Norwegian krone.
TYPE
const string
SEE ALSO
strategy
currency.NONE

Unspecified currency.
TYPE
const string
SEE ALSO
strategy
currency.NZD

New Zealand dollar.
TYPE
const string
SEE ALSO
strategy
currency.RUB

Russian ruble.
TYPE
const string
SEE ALSO
strategy
currency.SEK

Swedish krona.
TYPE
const string
SEE ALSO
strategy
currency.SGD

Singapore dollar.
TYPE
const string
SEE ALSO
strategy
currency.TRY

Turkish lira.
TYPE
const string
SEE ALSO
strategy
currency.USD

United States dollar.
TYPE
const string
SEE ALSO
strategy
currency.USDT

Tether.
TYPE
const string
SEE ALSO
strategy
currency.ZAR

South African rand.
TYPE
const string
SEE ALSO
strategy
dayofmonth

Date of current bar time in exchange timezone.
TYPE
series int
REMARKS
Note that this variable returns the day based on the time of the bar's open. For overnight sessions (e.g. EURUSD, where Monday session starts on Sunday, 17:00) this value can be lower by 1 than the day of the trading day.
SEE ALSO
dayofmonth
time
year
month
weekofyear
dayofweek
hour
minute
second
dayofweek

Day of week for current bar time in exchange timezone.
TYPE
series int
REMARKS
Note that this variable returns the day based on the time of the bar's open. For overnight sessions (e.g. EURUSD, where Monday session starts on Sunday, 17:00) this value can be lower by 1 than the day of the trading day.
You can use dayofweek.sunday, dayofweek.monday, dayofweek.tuesday, dayofweek.wednesday, dayofweek.thursday, dayofweek.friday and dayofweek.saturday variables for comparisons.
SEE ALSO
dayofweek
time
year
month
weekofyear
dayofmonth
hour
minute
second
dayofweek.friday

Is a named constant for return value of dayofweek function and value of dayofweek variable.
TYPE
const int
SEE ALSO
dayofweek.sunday
dayofweek.monday
dayofweek.tuesday
dayofweek.wednesday
dayofweek.thursday
dayofweek.saturday
dayofweek.monday

Is a named constant for return value of dayofweek function and value of dayofweek variable.
TYPE
const int
SEE ALSO
dayofweek.sunday
dayofweek.tuesday
dayofweek.wednesday
dayofweek.thursday
dayofweek.friday
dayofweek.saturday
dayofweek.saturday

Is a named constant for return value of dayofweek function and value of dayofweek variable.
TYPE
const int
SEE ALSO
dayofweek.sunday
dayofweek.monday
dayofweek.tuesday
dayofweek.wednesday
dayofweek.thursday
dayofweek.friday
dayofweek.sunday

Is a named constant for return value of dayofweek function and value of dayofweek variable.
TYPE
const int
SEE ALSO
dayofweek.monday
dayofweek.tuesday
dayofweek.wednesday
dayofweek.thursday
dayofweek.friday
dayofweek.saturday
dayofweek.thursday

Is a named constant for return value of dayofweek function and value of dayofweek variable.
TYPE
const int
SEE ALSO
dayofweek.sunday
dayofweek.monday
dayofweek.tuesday
dayofweek.wednesday
dayofweek.friday
dayofweek.saturday
dayofweek.tuesday

Is a named constant for return value of dayofweek function and value of dayofweek variable.
TYPE
const int
SEE ALSO
dayofweek.sunday
dayofweek.monday
dayofweek.wednesday
dayofweek.thursday
dayofweek.friday
dayofweek.saturday
dayofweek.wednesday

Is a named constant for return value of dayofweek function and value of dayofweek variable.
TYPE
const int
SEE ALSO
dayofweek.sunday
dayofweek.monday
dayofweek.tuesday
dayofweek.thursday
dayofweek.friday
dayofweek.saturday
display.all

A named constant for use with the `display` parameter of `plot*()` and `input*()` functions. Displays plotted or input values in all possible locations.
TYPE
const plot_simple_display
SEE ALSO
plot
plotshape
plotchar
plotarrow
plotbar
plotcandle
display.data_window

A named constant for use with the `display` parameter of `plot*()` and `input*()` functions. Displays plotted or input values in the Data Window, a menu accessible from the chart's right sidebar.
TYPE
const plot_display
SEE ALSO
plot
plotshape
plotchar
plotarrow
plotbar
plotcandle
display.none

A named constant for use with the `display` parameter of `plot*()` and `input*()` functions. `plot*()` functions using this will not display their plotted values anywhere. However, alert template messages and fill functions can still use the values, and they will appear in exported chart data. `input*()` functions using this constant will only display their values within the script's settings.
TYPE
const plot_simple_display
SEE ALSO
plot
plotshape
plotchar
plotarrow
plotbar
plotcandle
display.pane

A named constant for use with the `display` parameter of `plot*()` functions. Displays plotted values in the chart pane used by the script.
TYPE
const plot_display
SEE ALSO
plot
plotshape
plotchar
plotarrow
plotbar
plotcandle
display.price_scale

A named constant for use with the `display` parameter of `plot*()` functions. Displays the plot’s label and value on the price scale if the chart's settings allow it.
TYPE
const plot_display
SEE ALSO
plot
plotshape
plotchar
plotarrow
plotbar
plotcandle
display.status_line

A named constant for use with the `display` parameter of `plot*()` and `input*()` functions. Displays plotted or input values in the status line next to the script's name on the chart if the chart's settings allow it.
TYPE
const plot_display
SEE ALSO
plot
plotshape
plotchar
plotarrow
plotbar
plotcandle
dividends.gross

A named constant for the request.dividends function. Is used to request the dividends return on a stock before deductions.
TYPE
const string
SEE ALSO
request.dividends
dividends.net

A named constant for the request.dividends function. Is used to request the dividends return on a stock after deductions.
TYPE
const string
SEE ALSO
request.dividends
earnings.actual

A named constant for the request.earnings function. Is used to request the earnings value as it was reported.
TYPE
const string
SEE ALSO
request.earnings
earnings.estimate

A named constant for the request.earnings function. Is used to request the estimated earnings value.
TYPE
const string
SEE ALSO
request.earnings
earnings.standardized

A named constant for the request.earnings function. Is used to request the standardized earnings value.
TYPE
const string
SEE ALSO
request.earnings
extend.both

A named constant for line.new and line.set_extend functions.
TYPE
const string
SEE ALSO
line.new
line.set_extend
extend.none
extend.left
extend.right
extend.left

A named constant for line.new and line.set_extend functions.
TYPE
const string
SEE ALSO
line.new
line.set_extend
extend.none
extend.right
extend.both
extend.none

A named constant for line.new and line.set_extend functions.
TYPE
const string
SEE ALSO
line.new
line.set_extend
extend.left
extend.right
extend.both
extend.right

A named constant for line.new and line.set_extend functions.
TYPE
const string
SEE ALSO
line.new
line.set_extend
extend.none
extend.left
extend.both
font.family_default

Default text font for box.new, box.set_text_font_family, label.new, label.set_text_font_family, table.cell and table.cell_set_text_font_family functions.
TYPE
const string
SEE ALSO
box.new
box.set_text_font_family
label.new
label.set_text_font_family
table.cell
table.cell_set_text_font_family
font.family_monospace
font.family_monospace

Monospace text font for box.new, box.set_text_font_family, label.new, label.set_text_font_family, table.cell and table.cell_set_text_font_family functions.
TYPE
const string
SEE ALSO
box.new
box.set_text_font_family
label.new
label.set_text_font_family
table.cell
table.cell_set_text_font_family
font.family_default
format.inherit

Is a named constant for selecting the formatting of the script output values from the parent series in the indicator function.
TYPE
const string
SEE ALSO
indicator
format.price
format.volume
format.percent
format.mintick

Is a named constant to use with the str.tostring function. Passing a number to str.tostring with this argument rounds the number to the nearest value that can be divided by syminfo.mintick, without the remainder, with ties rounding up, and returns the string version of said value with trailing zeroes.
TYPE
const string
SEE ALSO
indicator
format.inherit
format.price
format.volume
format.percent

Is a named constant for selecting the formatting of the script output values as a percentage in the indicator function. It adds a percent sign after values.
TYPE
const string
REMARKS
The default precision is 2, regardless of the precision of the chart itself. This can be changed with the 'precision' argument of the indicator function.
SEE ALSO
indicator
format.inherit
format.price
format.volume
format.price

Is a named constant for selecting the formatting of the script output values as prices in the indicator function.
TYPE
const string
REMARKS
If format is format.price, default precision value is set. You can use the precision argument of indicator function to change the precision value.
SEE ALSO
indicator
format.inherit
format.volume
format.percent
format.volume

Is a named constant for selecting the formatting of the script output values as volume in the indicator function, e.g. '5183' will be formatted as '5.183K'.
TYPE
const string
SEE ALSO
indicator
format.inherit
format.price
format.percent
high

Current high price.
TYPE
series float
REMARKS
Previous values may be accessed with square brackets operator [], e.g. high[1], high[2].
SEE ALSO
open
low
close
volume
time
hl2
hlc3
hlcc4
ohlc4
hl2

Is a shortcut for (high + low)/2
TYPE
series float
SEE ALSO
open
high
low
close
volume
time
hlc3
hlcc4
ohlc4
hlc3

Is a shortcut for (high + low + close)/3
TYPE
series float
SEE ALSO
open
high
low
close
volume
time
hl2
hlcc4
ohlc4
hlcc4

Is a shortcut for (high + low + close + close)/4
TYPE
series float
SEE ALSO
open
high
low
close
volume
time
hl2
hlc3
ohlc4
hline.style_dashed

Is a named constant for dashed linestyle of hline function.
TYPE
const hline_style
SEE ALSO
hline.style_solid
hline.style_dotted
hline.style_dotted

Is a named constant for dotted linestyle of hline function.
TYPE
const hline_style
SEE ALSO
hline.style_solid
hline.style_dashed
hline.style_solid

Is a named constant for solid linestyle of hline function.
TYPE
const hline_style
SEE ALSO
hline.style_dotted
hline.style_dashed
hour

Current bar hour in exchange timezone.
TYPE
series int
SEE ALSO
hour
time
year
month
weekofyear
dayofmonth
dayofweek
minute
second
label.all

Returns an array filled with all the current labels drawn by the script.
TYPE
label[]
EXAMPLE

//@version=5
indicator("label.all")
//delete all labels
label.new(bar_index, close)
a_allLabels = label.all
if array.size(a_allLabels) > 0
    for i = 0 to array.size(a_allLabels) - 1
        label.delete(array.get(a_allLabels, i))
REMARKS
The array is read-only. Index zero of the array is the ID of the oldest object on the chart.
SEE ALSO
label.new
line.all
box.all
table.all
label.style_arrowdown

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_arrowup

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_circle

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_cross

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_diamond

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_flag

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_label_center

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_square
label.style_diamond
label.style_label_down

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_label_left

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_label_lower_left

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_label_lower_right

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_label_right

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_label_up

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_label_upper_left

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_label_upper_right

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_center
label.style_square
label.style_diamond
label.style_none

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_center
label.style_square
label.style_diamond
label.style_square

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_diamond
label.style_text_outline

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_triangledown

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_triangleup

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_xcross
label.style_cross
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_xcross

Label style for label.new and label.set_style functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
label.set_textalign
label.style_none
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_center
label.style_square
label.style_diamond
last_bar_index

Bar index of the last chart bar. Bar indices begin at zero on the first bar.
TYPE
series int
EXAMPLE

//@version=5
strategy("Mark Last X Bars For Backtesting", overlay = true, calc_on_every_tick = true)
lastBarsFilterInput = input.int(100, "Bars Count:")
// Here, we store the 'last_bar_index' value that is known from the beginning of the script's calculation.
// The 'last_bar_index' will change when new real-time bars appear, so we declare 'lastbar' with the 'var' keyword.
var lastbar = last_bar_index
// Check if the current bar_index is 'lastBarsFilterInput' removed from the last bar on the chart, or the chart is traded in real-time.
allowedToTrade = (lastbar - bar_index <= lastBarsFilterInput) or barstate.isrealtime
bgcolor(allowedToTrade ? color.new(color.green, 80) : na)
RETURNS
Last historical bar index for closed markets, or the real-time bar index for open markets.
REMARKS
Please note that using this variable can cause indicator repainting.
SEE ALSO
bar_index
last_bar_time
barstate.ishistory
barstate.isrealtime
last_bar_time

Time in UNIX format of the last chart bar. It is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
TYPE
series int
REMARKS
Please note that using this variable/function can cause indicator repainting.
Note that this variable returns the timestamp based on the time of the bar's open.
SEE ALSO
time
timenow
timestamp
last_bar_index
line.all

Returns an array filled with all the current lines drawn by the script.
TYPE
line[]
EXAMPLE

//@version=5
indicator("line.all")
//delete all lines
line.new(bar_index - 10, close, bar_index, close)
a_allLines = line.all
if array.size(a_allLines) > 0
    for i = 0 to array.size(a_allLines) - 1
        line.delete(array.get(a_allLines, i))
REMARKS
The array is read-only. Index zero of the array is the ID of the oldest object on the chart.
SEE ALSO
line.new
label.all
box.all
table.all
line.style_arrow_both

Line style for line.new and line.set_style functions. Solid line with arrows on both points.
TYPE
const string
SEE ALSO
line.new
line.set_style
line.style_solid
line.style_dotted
line.style_dashed
line.style_arrow_left
line.style_arrow_right
line.style_arrow_left

Line style for line.new and line.set_style functions. Solid line with arrow on the first point.
TYPE
const string
SEE ALSO
line.new
line.set_style
line.style_solid
line.style_dotted
line.style_dashed
line.style_arrow_right
line.style_arrow_both
line.style_arrow_right

Line style for line.new and line.set_style functions. Solid line with arrow on the second point.
TYPE
const string
SEE ALSO
line.new
line.set_style
line.style_solid
line.style_dotted
line.style_dashed
line.style_arrow_left
line.style_arrow_both
line.style_dashed

Line style for line.new and line.set_style functions.
TYPE
const string
SEE ALSO
line.new
line.set_style
line.style_solid
line.style_dotted
line.style_arrow_left
line.style_arrow_right
line.style_arrow_both
line.style_dotted

Line style for line.new and line.set_style functions.
TYPE
const string
SEE ALSO
line.new
line.set_style
line.style_solid
line.style_dashed
line.style_arrow_left
line.style_arrow_right
line.style_arrow_both
line.style_solid

Line style for line.new and line.set_style functions.
TYPE
const string
SEE ALSO
line.new
line.set_style
line.style_dotted
line.style_dashed
line.style_arrow_left
line.style_arrow_right
line.style_arrow_both
linefill.all

Returns an array filled with all the current linefill objects drawn by the script.
TYPE
linefill[]
REMARKS
The array is read-only. Index zero of the array is the ID of the oldest object on the chart.
location.abovebar

Location value for plotshape, plotchar functions. Shape is plotted above main series bars.
TYPE
const string
SEE ALSO
plotshape
plotchar
location.belowbar
location.top
location.bottom
location.absolute
location.absolute

Location value for plotshape, plotchar functions. Shape is plotted on chart using indicator value as a price coordinate.
TYPE
const string
SEE ALSO
plotshape
plotchar
location.abovebar
location.belowbar
location.top
location.bottom
location.belowbar

Location value for plotshape, plotchar functions. Shape is plotted below main series bars.
TYPE
const string
SEE ALSO
plotshape
plotchar
location.abovebar
location.top
location.bottom
location.absolute
location.bottom

Location value for plotshape, plotchar functions. Shape is plotted near the bottom chart border.
TYPE
const string
SEE ALSO
plotshape
plotchar
location.abovebar
location.belowbar
location.top
location.absolute
location.top

Location value for plotshape, plotchar functions. Shape is plotted near the top chart border.
TYPE
const string
SEE ALSO
plotshape
plotchar
location.abovebar
location.belowbar
location.bottom
location.absolute
low

Current low price.
TYPE
series float
REMARKS
Previous values may be accessed with square brackets operator [], e.g. low[1], low[2].
SEE ALSO
open
high
close
volume
time
hl2
hlc3
hlcc4
ohlc4
math.e

Is a named constant for Euler's number). It is equal to 2.7182818284590452.
TYPE
const float
SEE ALSO
math.phi
math.pi
math.rphi
math.phi

Is a named constant for the golden ratio. It is equal to 1.6180339887498948.
TYPE
const float
SEE ALSO
math.e
math.pi
math.rphi
math.pi

Is a named constant for Archimedes' constant. It is equal to 3.1415926535897932.
TYPE
const float
SEE ALSO
math.e
math.phi
math.rphi
math.rphi

Is a named constant for the golden ratio conjugate. It is equal to 0.6180339887498948.
TYPE
const float
SEE ALSO
math.e
math.pi
math.phi
minute

Current bar minute in exchange timezone.
TYPE
series int
SEE ALSO
minute
time
year
month
weekofyear
dayofmonth
dayofweek
hour
second
month

Current bar month in exchange timezone.
TYPE
series int
REMARKS
Note that this variable returns the month based on the time of the bar's open. For overnight sessions (e.g. EURUSD, where Monday session starts on Sunday, 17:00) this value can be lower by 1 than the month of the trading day.
SEE ALSO
month
time
year
weekofyear
dayofmonth
dayofweek
hour
minute
second
na

A keyword signifying "not available", indicating that a variable has no assigned value.
TYPE
simple na
EXAMPLE

//@version=5
indicator("na")
// CORRECT
// Plot no value when on bars zero to nine. Plot `close` on other bars.
plot(bar_index < 10 ? na : close)
// CORRECT ALTERNATIVE
// Initialize `a` to `na`. Reassign `close` to `a` on bars 10 and later.
float a = na
if bar_index >= 10
    a := close
plot(a)

// INCORRECT
// Trying to test the preceding bar's `close` for `na`.
// Will not work correctly on bar zero, when `close[1]` is `na`.
plot(close[1] == na ? close : close[1])
// CORRECT
// Use the `na()` function to test for `na`.
plot(na(close[1]) ? close : close[1])
// CORRECT ALTERNATIVE
// `nz()` tests `close[1]` for `na`. It returns `close[1]` if it is not `na`, and `close` if it is.
plot(nz(close[1], close))
REMARKS
Do not use this variable with comparison operators to test values for `na`, as it might lead to unexpected behavior. Instead, use the na function. Note that `na` can be used to initialize variables when the initialization statement also specifies the variable's type.
SEE ALSO
na
ohlc4

Is a shortcut for (open + high + low + close)/4
TYPE
series float
SEE ALSO
open
high
low
close
volume
time
hl2
hlc3
hlcc4
open

Current open price.
TYPE
series float
REMARKS
Previous values may be accessed with square brackets operator [], e.g. open[1], open[2].
SEE ALSO
high
low
close
volume
time
hl2
hlc3
hlcc4
ohlc4
order.ascending

Determines the sort order of the array from the smallest to the largest value.
TYPE
const sort_order
SEE ALSO
array.new_float
array.sort
order.descending

Determines the sort order of the array from the largest to the smallest value.
TYPE
const sort_order
SEE ALSO
array.new_float
array.sort
plot.style_area

A named constant for the 'Area' style, to be used as an argument for the `style` parameter in the plot function.
TYPE
const plot_style
SEE ALSO
plot
plot.style_steplinebr
plot.style_line
plot.style_linebr
plot.style_stepline
plot.style_stepline_diamond
plot.style_histogram
plot.style_areabr
plot.style_cross
plot.style_columns
plot.style_circles
plot.style_areabr

A named constant for the 'Area With Breaks' style, to be used as an argument for the `style` parameter in the plot function. Similar to plot.style_area, except the gaps in the data are not filled.
TYPE
const plot_style
SEE ALSO
plot
plot.style_steplinebr
plot.style_line
plot.style_linebr
plot.style_stepline
plot.style_stepline_diamond
plot.style_histogram
plot.style_cross
plot.style_area
plot.style_columns
plot.style_circles
plot.style_circles

A named constant for the 'Circles' style, to be used as an argument for the `style` parameter in the plot function.
TYPE
const plot_style
SEE ALSO
plot
plot.style_steplinebr
plot.style_line
plot.style_linebr
plot.style_stepline
plot.style_stepline_diamond
plot.style_histogram
plot.style_cross
plot.style_area
plot.style_areabr
plot.style_columns
plot.style_columns

A named constant for the 'Columns' style, to be used as an argument for the `style` parameter in the plot function.
TYPE
const plot_style
SEE ALSO
plot
plot.style_steplinebr
plot.style_line
plot.style_linebr
plot.style_stepline
plot.style_stepline_diamond
plot.style_histogram
plot.style_cross
plot.style_area
plot.style_areabr
plot.style_circles
plot.style_cross

A named constant for the 'Cross' style, to be used as an argument for the `style` parameter in the plot function.
TYPE
const plot_style
SEE ALSO
plot
plot.style_steplinebr
plot.style_line
plot.style_linebr
plot.style_stepline
plot.style_stepline_diamond
plot.style_histogram
plot.style_area
plot.style_areabr
plot.style_columns
plot.style_circles
plot.style_histogram

A named constant for the 'Histogram' style, to be used as an argument for the `style` parameter in the plot function.
TYPE
const plot_style
SEE ALSO
plot
plot.style_steplinebr
plot.style_line
plot.style_linebr
plot.style_stepline
plot.style_stepline_diamond
plot.style_cross
plot.style_area
plot.style_areabr
plot.style_columns
plot.style_circles
plot.style_line

A named constant for the 'Line' style, to be used as an argument for the `style` parameter in the plot function.
TYPE
const plot_style
SEE ALSO
plot
plot.style_steplinebr
plot.style_linebr
plot.style_stepline
plot.style_stepline_diamond
plot.style_histogram
plot.style_cross
plot.style_area
plot.style_areabr
plot.style_columns
plot.style_circles
plot.style_linebr

A named constant for the 'Line With Breaks' style, to be used as an argument for the `style` parameter in the plot function. Similar to plot.style_line, except the gaps in the data are not filled.
TYPE
const plot_style
SEE ALSO
plot
plot.style_steplinebr
plot.style_line
plot.style_stepline
plot.style_stepline_diamond
plot.style_histogram
plot.style_cross
plot.style_area
plot.style_areabr
plot.style_columns
plot.style_circles
plot.style_stepline

A named constant for the 'Step Line' style, to be used as an argument for the `style` parameter in the plot function.
TYPE
const plot_style
SEE ALSO
plot
plot.style_line
plot.style_steplinebr
plot.style_stepline_diamond
plot.style_linebr
plot.style_histogram
plot.style_cross
plot.style_area
plot.style_areabr
plot.style_columns
plot.style_circles
plot.style_stepline_diamond

A named constant for the 'Step Line With Diamonds' style, to be used as an argument for the `style` parameter in the plot function. Similar to plot.style_stepline, except the data changes are also marked with the Diamond shapes.
TYPE
const plot_style
SEE ALSO
plot
plot.style_steplinebr
plot.style_line
plot.style_linebr
plot.style_histogram
plot.style_cross
plot.style_area
plot.style_areabr
plot.style_columns
plot.style_circles
plot.style_steplinebr

A named constant for the 'Step line with Breaks' style, to be used as an argument for the `style` parameter in the plot function.
TYPE
const plot_style
SEE ALSO
plot
plot.style_circles
plot.style_line
plot.style_linebr
plot.style_stepline
plot.style_stepline_diamond
plot.style_histogram
plot.style_cross
plot.style_area
plot.style_areabr
plot.style_columns
position.bottom_center

Table position is used in table.new, table.cell functions.
Binds the table to the bottom edge in the center.
TYPE
const string
SEE ALSO
table.new
table.cell
table.set_position
position.top_left
position.top_center
position.top_right
position.middle_left
position.middle_center
position.middle_right
position.bottom_left
position.bottom_left

Table position is used in table.new, table.cell functions.
Binds the table to the bottom left of the screen.
TYPE
const string
SEE ALSO
table.new
table.cell
table.set_position
position.top_left
position.top_center
position.top_right
position.middle_left
position.middle_center
position.middle_right
position.bottom_center
position.bottom_right

Table position is used in table.new, table.cell functions.
Binds the table to the bottom right of the screen.
TYPE
const string
SEE ALSO
table.new
table.cell
table.set_position
position.top_left
position.top_center
position.top_right
position.middle_left
position.middle_center
position.middle_right
position.bottom_left
position.bottom_center
position.middle_center

Table position is used in table.new, table.cell functions.
Binds the table to the center of the screen.
TYPE
const string
SEE ALSO
table.new
table.cell
table.set_position
position.top_left
position.top_center
position.top_right
position.middle_left
position.middle_right
position.bottom_left
position.bottom_center
position.middle_left

Table position is used in table.new, table.cell functions.
Binds the table to the left side of the screen.
TYPE
const string
SEE ALSO
table.new
table.cell
table.set_position
position.top_left
position.top_center
position.top_right
position.middle_center
position.middle_right
position.bottom_left
position.bottom_center
position.middle_right

Table position is used in table.new, table.cell functions.
Binds the table to the right side of the screen.
TYPE
const string
SEE ALSO
table.new
table.cell
table.set_position
position.top_left
position.top_center
position.top_right
position.middle_left
position.middle_center
position.bottom_left
position.bottom_center
position.top_center

Table position is used in table.new, table.cell functions.
Binds the table to the top edge in the center.
TYPE
const string
SEE ALSO
table.new
table.cell
table.set_position
position.top_left
position.top_right
position.middle_left
position.middle_center
position.middle_right
position.bottom_left
position.bottom_center
position.top_left

Table position is used in table.new, table.cell functions.
Binds the table to the upper-left edge.
TYPE
const string
SEE ALSO
table.new
table.cell
table.set_position
position.top_center
position.top_right
position.middle_left
position.middle_center
position.middle_right
position.bottom_left
position.bottom_center
position.top_right

Table position is used in table.new, table.cell functions.
Binds the table to the upper-right edge.
TYPE
const string
SEE ALSO
table.new
table.cell
table.set_position
position.top_left
position.top_center
position.middle_left
position.middle_center
position.middle_right
position.bottom_left
position.bottom_center
scale.left

Scale value for indicator function. Indicator is added to the left price scale.
TYPE
const scale_type
SEE ALSO
indicator
scale.none

Scale value for indicator function. Indicator is added in 'No Scale' mode. Can be used only with 'overlay=true'.
TYPE
const scale_type
SEE ALSO
indicator
scale.right

Scale value for indicator function. Indicator is added to the right price scale.
TYPE
const scale_type
SEE ALSO
indicator
second

Current bar second in exchange timezone.
TYPE
series int
SEE ALSO
second
time
year
month
weekofyear
dayofmonth
dayofweek
hour
minute
session.extended

Constant for extended session type (with extended hours data).
TYPE
const string
SEE ALSO
session.regular
syminfo.session
session.isfirstbar

Returns true if the current bar is the first bar of the day's session, `false` otherwise. If extended session information is used, only returns true on the first bar of the pre-market bars.
TYPE
series bool
session.isfirstbar_regular

Returns true on the first regular session bar of the day, `false` otherwise. The result is the same whether extended session information is used or not.
TYPE
series bool
session.islastbar

Returns true if the current bar is the last bar of the day's session, `false` otherwise. If extended session information is used, only returns true on the last bar of the post-market bars.
TYPE
series bool
REMARKS
This variable is not guaranteed to return true once in every session because the last bar of the session might not exist if no trades occur during what should be the session's last bar.
This variable is not guaranteed to work as expected on non-standard chart types, e.g., Renko.
session.islastbar_regular

Returns true on the last regular session bar of the day, `false` otherwise. The result is the same whether extended session information is used or not.
TYPE
series bool
REMARKS
This variable is not guaranteed to return true once in every session because the last bar of the session might not exist if no trades occur during what should be the session's last bar.
This variable is not guaranteed to work as expected on non-standard chart types, e.g., Renko.
session.ismarket

Returns true if the current bar is a part of the regular trading hours (i.e. market hours), false otherwise
TYPE
series bool
SEE ALSO
session.ispremarket
session.ispostmarket
session.ispostmarket

Returns true if the current bar is a part of the post-market, false otherwise. On non-intraday charts always returns false.
TYPE
series bool
SEE ALSO
session.ismarket
session.ispremarket
session.ispremarket

Returns true if the current bar is a part of the pre-market, false otherwise. On non-intraday charts always returns false.
TYPE
series bool
SEE ALSO
session.ismarket
session.ispostmarket
session.regular

Constant for regular session type (no extended hours data).
TYPE
const string
SEE ALSO
session.extended
syminfo.session
shape.arrowdown

Shape style for plotshape function.
TYPE
const string
SEE ALSO
plotshape
shape.arrowup

Shape style for plotshape function.
TYPE
const string
SEE ALSO
plotshape
shape.circle

Shape style for plotshape function.
TYPE
const string
SEE ALSO
plotshape
shape.cross

Shape style for plotshape function.
TYPE
const string
SEE ALSO
plotshape
shape.diamond

Shape style for plotshape function.
TYPE
const string
SEE ALSO
plotshape
shape.flag

Shape style for plotshape function.
TYPE
const string
SEE ALSO
plotshape
shape.labeldown

Shape style for plotshape function.
TYPE
const string
SEE ALSO
plotshape
shape.labelup

Shape style for plotshape function.
TYPE
const string
SEE ALSO
plotshape
shape.square

Shape style for plotshape function.
TYPE
const string
SEE ALSO
plotshape
shape.triangledown

Shape style for plotshape function.
TYPE
const string
SEE ALSO
plotshape
shape.triangleup

Shape style for plotshape function.
TYPE
const string
SEE ALSO
plotshape
shape.xcross

Shape style for plotshape function.
TYPE
const string
SEE ALSO
plotshape
size.auto

Size value for plotshape, plotchar functions. The size of the shape automatically adapts to the size of the bars.
TYPE
const string
SEE ALSO
plotshape
plotchar
label.set_size
size.tiny
size.small
size.normal
size.large
size.huge
size.huge

Size value for plotshape, plotchar functions. The size of the shape constantly huge.
TYPE
const string
SEE ALSO
plotshape
plotchar
label.set_size
size.auto
size.tiny
size.small
size.normal
size.large
size.large

Size value for plotshape, plotchar functions. The size of the shape constantly large.
TYPE
const string
SEE ALSO
plotshape
plotchar
label.set_size
size.auto
size.tiny
size.small
size.normal
size.huge
size.normal

Size value for plotshape, plotchar functions. The size of the shape constantly normal.
TYPE
const string
SEE ALSO
plotshape
plotchar
label.set_size
size.auto
size.tiny
size.small
size.large
size.huge
size.small

Size value for plotshape, plotchar functions. The size of the shape constantly small.
TYPE
const string
SEE ALSO
plotshape
plotchar
label.set_size
size.auto
size.tiny
size.normal
size.large
size.huge
size.tiny

Size value for plotshape, plotchar functions. The size of the shape constantly tiny.
TYPE
const string
SEE ALSO
plotshape
plotchar
label.set_size
size.auto
size.small
size.normal
size.large
size.huge
splits.denominator

A named constant for the request.splits function. Is used to request the denominator (the number below the line in a fraction) of a splits.
TYPE
const string
SEE ALSO
request.splits
splits.numerator

A named constant for the request.splits function. Is used to request the numerator (the number above the line in a fraction) of a splits.
TYPE
const string
SEE ALSO
request.splits
strategy.account_currency

Returns the currency used to calculate results, which can be set in the strategy's properties.
TYPE
simple string
SEE ALSO
strategy
strategy.convert_to_account
strategy.convert_to_symbol
strategy.cash

This is one of the arguments that can be supplied to the `default_qty_type` parameter in the strategy declaration statement. It is only relevant when no value is used for the ‘qty’ parameter in strategy.entry or strategy.order function calls. It specifies that an amount of cash in the `strategy.account_currency` will be used to enter trades.
TYPE
const string
EXAMPLE

//@version=5
strategy("strategy.cash", overlay = true, default_qty_value = 50, default_qty_type = strategy.cash, initial_capital = 1000000)

if bar_index == 0
    // As ‘qty’ is not defined, the previously defined values for the `default_qty_type` and `default_qty_value` parameters are used to enter trades, namely 50 units of cash in the currency of `strategy.account_currency`.
    // `qty` is calculated as (default_qty_value)/(close price). If current price is $5, then qty = 50/5 = 10.
    strategy.entry("EN", strategy.long)
if bar_index == 2
    strategy.close("EN")
SEE ALSO
strategy
strategy.closedtrades

Number of trades, which were closed for the whole trading interval.
TYPE
series int
SEE ALSO
strategy.position_size
strategy.opentrades
strategy.wintrades
strategy.losstrades
strategy.eventrades
strategy.commission.cash_per_contract

Commission type for an order. Money displayed in the account currency per contract.
TYPE
const string
SEE ALSO
strategy
strategy.commission.cash_per_order

Commission type for an order. Money displayed in the account currency per order.
TYPE
const string
SEE ALSO
strategy
strategy.commission.percent

Commission type for an order. A percentage of the cash volume of order.
TYPE
const string
SEE ALSO
strategy
strategy.direction.all

It allows strategy to open both long and short positions.
TYPE
const string
SEE ALSO
strategy.risk.allow_entry_in
strategy.direction.long

It allows strategy to open only long positions.
TYPE
const string
SEE ALSO
strategy.risk.allow_entry_in
strategy.direction.short

It allows strategy to open only short positions.
TYPE
const string
SEE ALSO
strategy.risk.allow_entry_in
strategy.equity

Current equity (strategy.initial_capital + strategy.netprofit + strategy.openprofit).
TYPE
series float
SEE ALSO
strategy.netprofit
strategy.openprofit
strategy.position_size
strategy.eventrades

Number of breakeven trades for the whole trading interval.
TYPE
series int
SEE ALSO
strategy.position_size
strategy.opentrades
strategy.closedtrades
strategy.wintrades
strategy.losstrades
strategy.fixed

This is one of the arguments that can be supplied to the `default_qty_type` parameter in the strategy declaration statement. It is only relevant when no value is used for the ‘qty’ parameter in strategy.entry or strategy.order function calls. It specifies that a number of contracts/shares/lots will be used to enter trades.
TYPE
const string
EXAMPLE

//@version=5
strategy("strategy.fixed", overlay = true, default_qty_value = 50, default_qty_type = strategy.fixed, initial_capital = 1000000)

if bar_index == 0
    // As ‘qty’ is not defined, the previously defined values for the `default_qty_type` and `default_qty_value` parameters are used to enter trades, namely 50 contracts.
    // qty = 50
    strategy.entry("EN", strategy.long)
if bar_index == 2
    strategy.close("EN")
SEE ALSO
strategy
strategy.grossloss

Total currency value of all completed losing trades.
TYPE
series float
SEE ALSO
strategy.netprofit
strategy.grossprofit
strategy.grossprofit

Total currency value of all completed winning trades.
TYPE
series float
SEE ALSO
strategy.netprofit
strategy.grossloss
strategy.initial_capital

The amount of initial capital set in the strategy properties.
TYPE
series float
SEE ALSO
strategy
strategy.long

Long position entry.
TYPE
const strategy_direction
SEE ALSO
strategy.entry
strategy.exit
strategy.order
strategy.losstrades

Number of unprofitable trades for the whole trading interval.
TYPE
series int
SEE ALSO
strategy.position_size
strategy.opentrades
strategy.closedtrades
strategy.wintrades
strategy.eventrades
strategy.max_contracts_held_all

Maximum number of contracts/shares/lots/units in one trade for the whole trading interval.
TYPE
series float
SEE ALSO
strategy.position_size
strategy.max_contracts_held_long
strategy.max_contracts_held_short
strategy.max_contracts_held_long

Maximum number of contracts/shares/lots/units in one long trade for the whole trading interval.
TYPE
series float
SEE ALSO
strategy.position_size
strategy.max_contracts_held_all
strategy.max_contracts_held_short
strategy.max_contracts_held_short

Maximum number of contracts/shares/lots/units in one short trade for the whole trading interval.
TYPE
series float
SEE ALSO
strategy.position_size
strategy.max_contracts_held_all
strategy.max_contracts_held_long
strategy.max_drawdown

Maximum equity drawdown value for the whole trading interval.
TYPE
series float
SEE ALSO
strategy.netprofit
strategy.equity
strategy.max_runup
strategy.max_runup

Maximum equity run-up value for the whole trading interval.
TYPE
series float
SEE ALSO
strategy.netprofit
strategy.equity
strategy.max_drawdown
strategy.netprofit

Total currency value of all completed trades.
TYPE
series float
SEE ALSO
strategy.openprofit
strategy.position_size
strategy.grossprofit
strategy.grossloss
strategy.oca.cancel

OCA type value for strategy's functions. The parameter determines that an order should belong to an OCO group, where as soon as an order is filled, all other orders of the same group are cancelled. Note: if more than 1 guaranteed-to-be-executed orders of the same OCA group are placed at once, all those orders are filled.
TYPE
const string
SEE ALSO
strategy.entry
strategy.exit
strategy.order
strategy.oca.none

OCA type value for strategy's functions. The parameter determines that an order should not belong to any particular OCO group.
TYPE
const string
SEE ALSO
strategy.entry
strategy.exit
strategy.order
strategy.oca.reduce

OCA type value for strategy's functions. The parameter determines that an order should belong to an OCO group, where if X number of contracts of an order is filled, number of contracts for each other order of the same OCO group is decreased by X. Note: if more than 1 guaranteed-to-be-executed orders of the same OCA group are placed at once, all those orders are filled.
TYPE
const string
SEE ALSO
strategy.entry
strategy.exit
strategy.order
strategy.openprofit

Current unrealized profit or loss for all open positions.
TYPE
series float
SEE ALSO
strategy.netprofit
strategy.position_size
strategy.opentrades

Number of market position entries, which were not closed and remain opened. If there is no open market position, 0 is returned.
TYPE
series int
SEE ALSO
strategy.position_size
strategy.percent_of_equity

This is one of the arguments that can be supplied to the `default_qty_type` parameter in the strategy declaration statement. It is only relevant when no value is used for the ‘qty’ parameter in strategy.entry or strategy.order function calls. It specifies that a percentage (0-100) of equity will be used to enter trades.
TYPE
const string
EXAMPLE

//@version=5
strategy("strategy.percent_of_equity", overlay = false, default_qty_value = 100, default_qty_type = strategy.percent_of_equity, initial_capital = 1000000)

// As ‘qty’ is not defined, the previously defined values for the `default_qty_type` and `default_qty_value` parameters are used to enter trades, namely 100% of available equity.
if bar_index == 0
    strategy.entry("EN", strategy.long)
if bar_index == 2
    strategy.close("EN")
plot(strategy.equity)

 // The ‘qty’ parameter is set to 10. Entering position with fixed size of 10 contracts and entry market price = (10 * close).
if bar_index == 4
    strategy.entry("EN", strategy.long, qty = 10)
if bar_index == 6
    strategy.close("EN")
SEE ALSO
strategy
strategy.position_avg_price

Average entry price of current market position. If the market position is flat, 'NaN' is returned.
TYPE
series float
SEE ALSO
strategy.position_size
strategy.position_entry_name

Name of the order that initially opened current market position.
TYPE
series string
SEE ALSO
strategy.position_size
strategy.position_size

Direction and size of the current market position. If the value is > 0, the market position is long. If the value is < 0, the market position is short. The absolute value is the number of contracts/shares/lots/units in trade (position size).
TYPE
series float
SEE ALSO
strategy.position_avg_price
strategy.short

Short position entry.
TYPE
const strategy_direction
SEE ALSO
strategy.entry
strategy.exit
strategy.order
strategy.wintrades

Number of profitable trades for the whole trading interval.
TYPE
series int
SEE ALSO
strategy.position_size
strategy.opentrades
strategy.closedtrades
strategy.losstrades
strategy.eventrades
syminfo.basecurrency

Base currency for the symbol. For the symbol 'BTCUSD' returns 'BTC'.
TYPE
simple string
SEE ALSO
syminfo.currency
syminfo.ticker
syminfo.country

Returns the two-letter code of the country where the symbol is traded, in the ISO 3166-1 alpha-2 format, or na if the exchange is not directly tied to a specific country. For example, on "NASDAQ:AAPL" it will return "US", on "LSE:AAPL" it will return "GB", and on "BITSTAMP:BTCUSD it will return na.
TYPE
simple string
syminfo.currency

Currency for the current symbol. Returns currency code: 'USD', 'EUR', etc.
TYPE
simple string
SEE ALSO
syminfo.basecurrency
syminfo.ticker
currency.USD
currency.EUR
syminfo.description

Description for the current symbol.
TYPE
simple string
SEE ALSO
syminfo.ticker
syminfo.prefix
syminfo.industry

Returns the industry of the symbol, or na if the symbol has no industry. Example: "Internet Software/Services", "Packaged software", "Integrated Oil", "Motor Vehicles", etc. These are the same values one can see in the chart's "Symbol info" window.
TYPE
simple string
REMARKS
A sector is a broad section of the economy. An industry is a narrower classification. NASDAQ:CAT (Caterpillar, Inc.) for example, belongs to the "Producer Manufacturing" sector and the "Trucks/Construction/Farm Machinery" industry.
syminfo.mintick

Min tick value for the current symbol.
TYPE
simple float
SEE ALSO
syminfo.pointvalue
syminfo.pointvalue

Point value for the current symbol.
TYPE
simple float
SEE ALSO
syminfo.mintick
syminfo.prefix

Prefix of current symbol name (i.e. for 'CME_EOD:TICKER' prefix is 'CME_EOD').
TYPE
simple string
EXAMPLE

//@version=5
indicator("syminfo.prefix")

// If current chart symbol is 'BATS:MSFT' then syminfo.prefix is 'BATS'.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, text=syminfo.prefix)
SEE ALSO
syminfo.ticker
syminfo.tickerid
syminfo.root

Root for derivatives like futures contract. For other symbols returns the same value as syminfo.ticker.
TYPE
simple string
EXAMPLE

//@version=5
indicator("syminfo.root")

// If the current chart symbol is continuous futures ('ES1!'), it would display 'ES'.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, syminfo.root)
SEE ALSO
syminfo.ticker
syminfo.tickerid
syminfo.sector

Returns the sector of the symbol, or na if the symbol has no sector. Example: "Electronic Technology", "Technology services", "Energy Minerals", "Consumer Durables", etc. These are the same values one can see in the chart's "Symbol info" window.
TYPE
simple string
REMARKS
A sector is a broad section of the economy. An industry is a narrower classification. NASDAQ:CAT (Caterpillar, Inc.) for example, belongs to the "Producer Manufacturing" sector and the "Trucks/Construction/Farm Machinery" industry.
syminfo.session

Session type of the chart main series. Possible values are session.regular, session.extended.
TYPE
simple string
SEE ALSO
session.regular
session.extended
syminfo.ticker

Symbol name without exchange prefix, e.g. 'MSFT'.
TYPE
simple string
SEE ALSO
syminfo.tickerid
timeframe.period
timeframe.multiplier
syminfo.root
syminfo.tickerid

Returns the full form of the ticker ID representing a symbol, for use as an argument in functions with a `ticker` or `symbol` parameter. It always includes the prefix (exchange) and ticker separated by a colon ("NASDAQ:AAPL"), but it can also include other symbol data such as dividend adjustment, chart type, currency conversion, etc.
TYPE
simple string
REMARKS
Because the value of this variable does not always use a simple "prefix:ticker" format, it is a poor candidate for use in boolean comparisons or string manipulation functions. In those contexts, run the variable's result through ticker.standard to purify it. This will remove any extraneous information and return a ticker ID consistently formatted using the "prefix:ticker" structure.
SEE ALSO
ticker.new
syminfo.ticker
timeframe.period
timeframe.multiplier
syminfo.root
syminfo.timezone

Timezone of the exchange of the chart main series. Possible values see in timestamp.
TYPE
simple string
SEE ALSO
timestamp
syminfo.type

Type of the current symbol. Possible values are stock, futures, index, forex, crypto, fund, dr.
TYPE
simple string
SEE ALSO
syminfo.ticker
syminfo.volumetype

Volume type of the current symbol. Possible values are: "base" for base currency, "quote" for quote currency, "tick" for the number of transactions, and "n/a" when there is no volume or its type is not specified.
TYPE
simple string
REMARKS
Only some data feed suppliers provide information qualifying volume. As a result, the variable will return a value on some symbols only, mostly in the crypto sector.
SEE ALSO
syminfo.type
ta.accdist

Accumulation/distribution index.
TYPE
series float
ta.iii

Intraday Intensity Index.
TYPE
series float
EXAMPLE

//@version=5
indicator("Intraday Intensity Index")
plot(ta.iii, color=color.yellow)

// the same on pine
f_iii() =>
    (2 * close - high - low) / ((high - low) * volume)

plot(f_iii())
ta.nvi

Negative Volume Index.
TYPE
series float
EXAMPLE

//@version=5
indicator("Negative Volume Index")

plot(ta.nvi, color=color.yellow)

// the same on pine
f_nvi() =>
    float ta_nvi = 1.0
    float prevNvi = (nz(ta_nvi[1], 0.0) == 0.0)  ? 1.0: ta_nvi[1]
    if nz(close, 0.0) == 0.0 or nz(close[1], 0.0) == 0.0
        ta_nvi := prevNvi
    else
        ta_nvi := (volume < nz(volume[1], 0.0)) ? prevNvi + ((close - close[1]) / close[1]) * prevNvi : prevNvi
    result = ta_nvi

plot(f_nvi())
ta.obv

On Balance Volume.
TYPE
series float
EXAMPLE

//@version=5
indicator("On Balance Volume")
plot(ta.obv, color=color.yellow)

// the same on pine
f_obv() =>
    ta.cum(math.sign(ta.change(close)) * volume)

plot(f_obv())
ta.pvi

Positive Volume Index.
TYPE
series float
EXAMPLE

//@version=5
indicator("Positive Volume Index")

plot(ta.pvi, color=color.yellow)

// the same on pine
f_pvi() =>
    float ta_pvi = 1.0
    float prevPvi = (nz(ta_pvi[1], 0.0) == 0.0)  ? 1.0: ta_pvi[1]
    if nz(close, 0.0) == 0.0 or nz(close[1], 0.0) == 0.0
        ta_pvi := prevPvi
    else
        ta_pvi := (volume > nz(volume[1], 0.0)) ? prevPvi + ((close - close[1]) / close[1]) * prevPvi : prevPvi
    result = ta_pvi

plot(f_pvi())
ta.pvt

Price-Volume Trend.
TYPE
series float
EXAMPLE

//@version=5
indicator("Price-Volume Trend")
plot(ta.pvt, color=color.yellow)

// the same on pine
f_pvt() =>
    ta.cum((ta.change(close) / close[1]) * volume)

plot(f_pvt())
ta.tr

True range. Same as tr(false). It is max(high - low, abs(high - close[1]), abs(low - close[1]))
TYPE
series float
SEE ALSO
ta.tr
ta.atr
ta.vwap

Volume Weighted Average Price. It uses hlc3 as its source series.
TYPE
series float
SEE ALSO
ta.vwap
ta.wad

Williams Accumulation/Distribution.
TYPE
series float
EXAMPLE

//@version=5
indicator("Williams Accumulation/Distribution")
plot(ta.wad, color=color.yellow)

// the same on pine
f_wad() =>
    trueHigh = math.max(high, close[1])
    trueLow = math.min(low, close[1])
    mom = ta.change(close)
    gain = (mom > 0) ? close - trueLow : (mom < 0) ? close - trueHigh : 0
    ta.cum(gain)

plot(f_wad())
ta.wvad

Williams Variable Accumulation/Distribution.
TYPE
series float
EXAMPLE

//@version=5
indicator("Williams Variable Accumulation/Distribution")
plot(ta.wvad, color=color.yellow)

// the same on pine
f_wvad() =>
    (close - open) / (high - low) * volume

plot(f_wvad())
table.all

Returns an array filled with all the current tables drawn by the script.
TYPE
table[]
EXAMPLE

//@version=5
indicator("table.all")
//delete all tables
table.new(position = position.top_right, columns = 2, rows = 1, bgcolor = color.yellow, border_width = 1)
a_allTables = table.all
if array.size(a_allTables) > 0
    for i = 0 to array.size(a_allTables) - 1
        table.delete(array.get(a_allTables, i))
REMARKS
The array is read-only. Index zero of the array is the ID of the oldest object on the chart.
SEE ALSO
table.new
line.all
label.all
box.all
text.align_bottom

Vertical text alignment for box.new, box.set_text_valign, table.cell and table.cell_set_text_valign functions.
TYPE
const string
SEE ALSO
table.cell
table.cell_set_text_valign
text.align_center
text.align_left
text.align_right
text.align_center

Text alignment for box.new, box.set_text_halign, box.set_text_valign, label.new and label.set_textalign functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
text.align_left
text.align_right
text.align_left

Horizontal text alignment for box.new, box.set_text_halign, label.new and label.set_textalign functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
text.align_center
text.align_right
text.align_right

Horizontal text alignment for box.new, box.set_text_halign, label.new and label.set_textalign functions.
TYPE
const string
SEE ALSO
label.new
label.set_style
text.align_center
text.align_left
text.align_top

Vertical text alignment for box.new, box.set_text_valign, table.cell and table.cell_set_text_valign functions.
TYPE
const string
SEE ALSO
table.cell
table.cell_set_text_valign
text.align_center
text.align_left
text.align_right
text.wrap_auto

Automatic wrapping mode for box.new and box.set_text_wrap functions.
TYPE
const string
SEE ALSO
box.new
text.wrap_none

Disabled wrapping mode for box.new and box.set_text_wrap functions.
TYPE
const string
SEE ALSO
box.new
time

Current bar time in UNIX format. It is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
TYPE
series int
REMARKS
Note that this variable returns the timestamp based on the time of the bar's open. Because of that, for overnight sessions (e.g. EURUSD, where Monday session starts on Sunday, 17:00) this variable can return time before the specified date of the trading day. For example, on EURUSD, `dayofmonth(time)` can be lower by 1 than the date of the trading day, because the bar for the current day actually opens one day prior.
SEE ALSO
time
time_close
timenow
year
month
weekofyear
dayofmonth
dayofweek
hour
minute
second
time_close

Current bar close time in UNIX format. It is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970. On price-based charts this variable value is na.
TYPE
series int
SEE ALSO
time
timenow
year
month
weekofyear
dayofmonth
dayofweek
hour
minute
second
time_tradingday

The beginning time of the trading day the current bar belongs to, in UNIX format (the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970).
TYPE
series int
REMARKS
The variable is useful for overnight sessions, where the current day's session can start on the previous calendar day (e.g., on EURUSD the Monday session will start on Sunday, 17:00). Unlike `time`, which would return the timestamp for Sunday at 17:00 for the Monday daily bar, `time_tradingday` will return the timestamp for Monday, 00:00.
When used on timeframes higher than 1D, `time_tradingday` returns the trading day of the last day inside the bar (e.g. on 1W, it will return the last trading day of the week).
SEE ALSO
time
time_close
timeframe.isdaily

Returns true if current resolution is a daily resolution, false otherwise.
TYPE
simple bool
SEE ALSO
timeframe.isdwm
timeframe.isintraday
timeframe.isminutes
timeframe.isseconds
timeframe.isweekly
timeframe.ismonthly
timeframe.isdwm

Returns true if current resolution is a daily or weekly or monthly resolution, false otherwise.
TYPE
simple bool
SEE ALSO
timeframe.isintraday
timeframe.isminutes
timeframe.isseconds
timeframe.isdaily
timeframe.isweekly
timeframe.ismonthly
timeframe.isintraday

Returns true if current resolution is an intraday (minutes or seconds) resolution, false otherwise.
TYPE
simple bool
SEE ALSO
timeframe.isminutes
timeframe.isseconds
timeframe.isdwm
timeframe.isdaily
timeframe.isweekly
timeframe.ismonthly
timeframe.isminutes

Returns true if current resolution is a minutes resolution, false otherwise.
TYPE
simple bool
SEE ALSO
timeframe.isdwm
timeframe.isintraday
timeframe.isseconds
timeframe.isdaily
timeframe.isweekly
timeframe.ismonthly
timeframe.ismonthly

Returns true if current resolution is a monthly resolution, false otherwise.
TYPE
simple bool
SEE ALSO
timeframe.isdwm
timeframe.isintraday
timeframe.isminutes
timeframe.isseconds
timeframe.isdaily
timeframe.isweekly
timeframe.isseconds

Returns true if current resolution is a seconds resolution, false otherwise.
TYPE
simple bool
SEE ALSO
timeframe.isdwm
timeframe.isintraday
timeframe.isminutes
timeframe.isdaily
timeframe.isweekly
timeframe.ismonthly
timeframe.isweekly

Returns true if current resolution is a weekly resolution, false otherwise.
TYPE
simple bool
SEE ALSO
timeframe.isdwm
timeframe.isintraday
timeframe.isminutes
timeframe.isseconds
timeframe.isdaily
timeframe.ismonthly
timeframe.multiplier

Multiplier of resolution, e.g. '60' - 60, 'D' - 1, '5D' - 5, '12M' - 12.
TYPE
simple int
SEE ALSO
syminfo.ticker
syminfo.tickerid
timeframe.period
timeframe.period

A string representation of the chart's timeframe. The returned string's format is "[<quantity>][<units>]", where <quantity> and <units> are in some cases absent. <quantity> is the number of units, but it is absent if that number is 1. <unit> is "S" for seconds, "D" for days, "W" for weeks, "M" for months, but it is absent for minutes. No <unit> exists for hours.
The variable will return: "10S" for 10 seconds, "60" for 60 minutes, "D" for one day, "2W" for two weeks, "3M" for one quarter.
Can be used as an argument with any function containing a `timeframe` parameter.
TYPE
simple string
SEE ALSO
syminfo.ticker
syminfo.tickerid
timeframe.multiplier
timenow

Current time in UNIX format. It is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
TYPE
series int
REMARKS
Please note that using this variable/function can cause indicator repainting.
SEE ALSO
timestamp
time
time_close
year
month
weekofyear
dayofmonth
dayofweek
hour
minute
second
volume

Current bar volume.
TYPE
series float
REMARKS
Previous values may be accessed with square brackets operator [], e.g. volume[1], volume[2].
SEE ALSO
open
high
low
close
time
hl2
hlc3
hlcc4
ohlc4
weekofyear

Week number of current bar time in exchange timezone.
TYPE
series int
REMARKS
Note that this variable returns the week based on the time of the bar's open. For overnight sessions (e.g. EURUSD, where Monday session starts on Sunday, 17:00) this value can be lower by 1 than the week of the trading day.
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
xloc.bar_index

A named constant that specifies the algorithm of interpretation of x-value in functions line.new and label.new. If xloc = xloc.bar_index, value of x is a bar index.
TYPE
const string
SEE ALSO
line.new
label.new
line.set_xloc
label.set_xloc
xloc.bar_time
xloc.bar_time

A named constant that specifies the algorithm of interpretation of x-value in functions line.new and label.new. If xloc = xloc.bar_time, value of x is a bar UNIX time.
TYPE
const string
SEE ALSO
line.new
label.new
line.set_xloc
label.set_xloc
xloc.bar_index
year

Current bar year in exchange timezone.
TYPE
series int
REMARKS
Note that this variable returns the year based on the time of the bar's open. For overnight sessions (e.g. EURUSD, where Monday session starts on Sunday, 17:00) this value can be lower by 1 than the year of the trading day.
SEE ALSO
year
time
month
weekofyear
dayofmonth
dayofweek
hour
minute
second
yloc.abovebar

A named constant that specifies the algorithm of interpretation of y-value in function label.new.
TYPE
const string
SEE ALSO
label.new
label.set_yloc
yloc.price
yloc.belowbar
yloc.belowbar

A named constant that specifies the algorithm of interpretation of y-value in function label.new.
TYPE
const string
SEE ALSO
label.new
label.set_yloc
yloc.price
yloc.abovebar
yloc.price

A named constant that specifies the algorithm of interpretation of y-value in function label.new.
TYPE
const string
SEE ALSO
label.new
label.set_yloc
yloc.abovebar
yloc.belowbar