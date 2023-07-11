// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © theEccentricTrader
//@version=5


indicator('Swing Counter [theEccentricTrader]', overlay = true, max_lines_count = 500)


//////////// sample period filter ////////////


showSamplePeriod = input(defval = true, title = 'Show Sample Period', group = 'Sample Period')

start = input.time(timestamp('2 Jan 1800 00:00'), title = 'Start Date', group = 'Sample Period')
end = input.time(timestamp('1 Jan 3000 00:00'), title = 'End Date', group = 'Sample Period')
samplePeriodFilter = time >= start and time <= end

var barOneDate = time

f_timeToString(_t) =>
    str.tostring(dayofmonth(_t), '00') + '.' + str.tostring(month(_t), '00') + '.' + str.tostring(year(_t), '0000')

startDateText = barOneDate > start ? f_timeToString(barOneDate) : f_timeToString(start)
endDateText = time >= end ? '-' + f_timeToString(end) : '-' + f_timeToString(time)


//////////// swing counters //////////// 


shPrice = close[1] >= open[1] and close < open and high >= high[1] and barstate.isconfirmed ? high : 
 close[1] >= open[1] and close < open and high <= high[1] and barstate.isconfirmed ? high[1] : na
shBarIndex = close[1] >= open[1] and close < open and high >= high[1] and barstate.isconfirmed ? bar_index : 
 close[1] >= open[1] and close < open and high <= high[1] and barstate.isconfirmed ? bar_index - 1 : na
shPriceOne = ta.valuewhen(shPrice, shPrice, 1)
shBarIndexOne = ta.valuewhen(shBarIndex, shBarIndex, 1)

slPrice = close[1] < open[1] and close >= open and low <= low[1] and barstate.isconfirmed ? low : 
 close[1] < open[1] and close >= open and low >= low[1] and barstate.isconfirmed ? low[1] : na
slBarIndex = close[1] < open[1] and close >= open and low <= low[1] and barstate.isconfirmed ? bar_index : 
 close[1] < open[1] and close >= open and low >= low[1] and barstate.isconfirmed ? bar_index - 1 : na
slPriceOne = ta.valuewhen(slPrice, slPrice, 1)
slBarIndexOne = ta.valuewhen(slBarIndex, slBarIndex, 1)

returnLineUptrend = ta.valuewhen(shPrice, shPrice, 0) > ta.valuewhen(shPrice, shPrice, 1)
uptrend = ta.valuewhen(slPrice, slPrice, 0) > ta.valuewhen(slPrice, slPrice, 1)
downtrend = ta.valuewhen(shPrice, shPrice, 0) < ta.valuewhen(shPrice, shPrice, 1)
returnLineDowntrend = ta.valuewhen(slPrice, slPrice, 0) < ta.valuewhen(slPrice, slPrice, 1)
doubleTop = ta.valuewhen(shPrice, shPrice, 0) == ta.valuewhen(shPrice, shPrice, 1)
doubleBottom = ta.valuewhen(slPrice, slPrice, 0) == ta.valuewhen(slPrice, slPrice, 1)

var shCounter = 0
var slCounter = 0

var returnLineUptrendCounter = 0
var uptrendCounter = 0
var downtrendCounter = 0
var returnLineDowntrendCounter = 0
var doubleTopCounter = 0
var doubleBottomCounter = 0

if shPrice and samplePeriodFilter
    shCounter += 1
if slPrice and samplePeriodFilter
    slCounter += 1

if shPrice and returnLineUptrend and samplePeriodFilter
    returnLineUptrendCounter += 1
if slPrice and uptrend and samplePeriodFilter
    uptrendCounter += 1 
if shPrice and downtrend and samplePeriodFilter
    downtrendCounter += 1
if slPrice and returnLineDowntrend and samplePeriodFilter
    returnLineDowntrendCounter += 1
if shPrice and doubleTop and samplePeriodFilter
    doubleTopCounter += 1
if slPrice and doubleBottom and samplePeriodFilter
    doubleBottomCounter += 1
    

//////////// table ////////////


swingTablePositionInput = input.string(title = 'Position', defval = 'Top Right', options = ['Top Right', 'Top Center', 'Top Left', 'Bottom Right', 'Bottom Center', 'Bottom Left', 
 'Middle Right', 'Middle Center', 'Middle Left'], group = 'Table Positioning')
swingTablePosition = swingTablePositionInput == 'Top Right' ? position.top_right : swingTablePositionInput == 'Top Center' ? position.top_center : 
 swingTablePositionInput == 'Top Left' ? position.top_left : swingTablePositionInput == 'Bottom Right' ? position.bottom_right : swingTablePositionInput == 'Bottom Center' ? position.bottom_center : 
 swingTablePositionInput == 'Bottom Left' ? position.bottom_left : swingTablePositionInput == 'Middle Right' ? position.middle_right : swingTablePositionInput == 'Middle Center' ? position.middle_center : 
 swingTablePositionInput == 'Middle Left' ? position.middle_left : na

textSizeInput = input.string(title = 'Text Size', defval='Normal', options = ['Tiny', 'Small', 'Normal', 'Large'], group = 'Table Text Sizing')
textSize = textSizeInput == 'Tiny' ? size.tiny : textSizeInput == 'Small' ? size.small : textSizeInput == 'Normal' ? size.normal : textSizeInput == 'Large' ? size.large : na

var swingTable = table.new(swingTablePosition, 100, 100, border_width = 1)

table.cell(swingTable, 0, 0, text = 'Peaks', bgcolor = color.blue, text_color = color.white, text_size = textSize)
table.cell(swingTable, 0, 1, text = 'Troughs', bgcolor = color.blue, text_color = color.white, text_size = textSize)
table.cell(swingTable, 0, 2, text = 'Higher Peaks', bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(swingTable, 0, 3, text = 'Higher Troughs', bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(swingTable, 0, 4, text = 'Lower Peaks', bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(swingTable, 0, 5, text = 'Lower Troughs', bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(swingTable, 0, 6, text = 'Double-Top Peaks', bgcolor = color.blue, text_color = color.white, text_size = textSize)
table.cell(swingTable, 0, 7, text = 'Double-Bottom Troughs', bgcolor = color.blue, text_color = color.white, text_size = textSize)

table.cell(swingTable, 1, 0, text = str.tostring(shCounter), bgcolor = color.blue, text_color = color.white, text_size = textSize)
table.cell(swingTable, 1, 1, text = str.tostring(slCounter), bgcolor = color.blue, text_color = color.white, text_size = textSize)
table.cell(swingTable, 1, 2, text = str.tostring(returnLineUptrendCounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(swingTable, 1, 3, text = str.tostring(uptrendCounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(swingTable, 1, 4, text = str.tostring(downtrendCounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(swingTable, 1, 5, text = str.tostring(returnLineDowntrendCounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(swingTable, 1, 6, text = str.tostring(doubleTopCounter), bgcolor = color.blue, text_color = color.white, text_size = textSize)
table.cell(swingTable, 1, 7, text = str.tostring(doubleBottomCounter), bgcolor = color.blue, text_color = color.white, text_size = textSize)

table.cell(swingTable, 2, 1, text = '%', bgcolor = color.blue, text_color = color.white, text_size = textSize)
table.cell(swingTable, 2, 2, text = str.tostring(math.round(returnLineUptrendCounter / shCounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(swingTable, 2, 3, text = str.tostring(math.round(uptrendCounter / slCounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(swingTable, 2, 4, text = str.tostring(math.round(downtrendCounter / shCounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(swingTable, 2, 5, text = str.tostring(math.round(returnLineDowntrendCounter / slCounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(swingTable, 2, 6, text = str.tostring(math.round(doubleTopCounter / shCounter * 100, 2)), bgcolor = color.blue, text_color = color.white, text_size = textSize)
table.cell(swingTable, 2, 7, text = str.tostring(math.round(doubleBottomCounter / slCounter * 100, 2)), bgcolor = color.blue, text_color = color.white, text_size = textSize)

if showSamplePeriod 
    table.cell(swingTable, 0, 8, text = startDateText + endDateText, bgcolor = color.black, text_color = color.white, text_size = textSize)


//////////// plots ////////////


showPlots = input(defval = true, title = "Show Plots", group = 'Plots')

plotshape(shPrice and returnLineUptrend and samplePeriodFilter and showPlots and high >= high[1] and barstate.isconfirmed, 
 style = shape.triangleup, color = color.green, text = "HP", textcolor = color.green)
plotshape(slPrice and uptrend and samplePeriodFilter and showPlots and low <= low[1] and barstate.isconfirmed, 
 style = shape.triangleup, color = color.green, text = "HT", textcolor = color.green, location = location.belowbar)
plotshape(shPrice and downtrend and samplePeriodFilter and showPlots and high >= high[1] and barstate.isconfirmed, 
 style = shape.triangledown, color = color.red, text = "LP", textcolor = color.red)
plotshape(slPrice and returnLineDowntrend and samplePeriodFilter and showPlots and low <= low[1] and barstate.isconfirmed, 
 style = shape.triangledown, color = color.red, text = "LT", textcolor = color.red, location = location.belowbar)
plotshape(shPrice and doubleTop and samplePeriodFilter and showPlots, 
 style = shape.diamond, color = color.blue, text = "DP", textcolor = color.blue)
plotshape(slPrice and doubleBottom and samplePeriodFilter and showPlots, 
 style = shape.diamond, color = color.blue, text = "DT", textcolor = color.blue, location = location.belowbar)

plotshape(shPrice and returnLineUptrend and samplePeriodFilter and showPlots and high < high[1] and barstate.isconfirmed, 
 style = shape.triangleup, color = color.green, text = "HP", textcolor = color.green, offset = -1)
plotshape(slPrice and uptrend and samplePeriodFilter and showPlots and low > low[1] and barstate.isconfirmed, 
 style = shape.triangleup, color = color.green, text = "HT", textcolor = color.green, location = location.belowbar, offset = -1)
plotshape(shPrice and downtrend and samplePeriodFilter and showPlots and high < high[1] and barstate.isconfirmed, 
 style = shape.triangledown, color = color.red, text = "LP", textcolor = color.red, offset = -1)
plotshape(slPrice and returnLineDowntrend and samplePeriodFilter and showPlots and low > low[1] and barstate.isconfirmed, 
 style = shape.triangledown, color = color.red, text = "LT", textcolor = color.red, location = location.belowbar, offset = -1)


//////////// lines ////////////


showLines = input(defval = true, title = "Show Lines", group = 'Lines')

if shPrice and showLines
    line.new(shBarIndexOne, shPriceOne, shBarIndex, shPrice, color = shPrice > shPriceOne ? color.green : color.red)
if slPrice and showLines
    line.new(slBarIndexOne, slPriceOne, slBarIndex, slPrice, color = slPrice > slPriceOne ? color.green : color.red)
    
