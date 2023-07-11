// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © theEccentricTrader
//@version=5


indicator('Double Candle Trend Counter [theEccentricTrader]', overlay = true)


//////////// sample period filter ////////////


showSamplePeriod = input(defval = true, title = 'Show Sample Period', group = 'Sample Period')

start = input.time(timestamp('1 Jan 1800 00:00'), title = 'Start Date', group = 'Sample Period')
end = input.time(timestamp('1 Jan 3000 00:00'), title = 'End Date', group = 'Sample Period')
samplePeriodFilter = time >= start and time <= end

var barOneDate = time

f_timeToString(_t) =>
    str.tostring(dayofmonth(_t), '00') + '.' + str.tostring(month(_t), '00') + '.' + str.tostring(year(_t), '0000')

startDateText = barOneDate > start ? f_timeToString(barOneDate) : f_timeToString(start)
endDateText = time >= end ? '-' + f_timeToString(end) : '-' + f_timeToString(time)


//////////// upper and lower candle trend counters //////////// 


oneDUT = high > high[1] and low > low[1] and (high[1] <= high[2] or low[1] <= low[2]) and barstate.isconfirmed and samplePeriodFilter
twoDUT = oneDUT[1] and high > high[1] and oneDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
threeDUT = twoDUT[1] and high > high[1] and twoDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
fourDUT = threeDUT[1] and high > high[1] and threeDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
fiveDUT = fourDUT[1] and high > high[1] and fourDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
sixDUT = fiveDUT[1] and high > high[1] and fiveDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
sevenDUT = sixDUT[1] and high > high[1] and sixDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
eightDUT = sevenDUT[1] and high > high[1] and sevenDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
nineDUT = eightDUT[1] and high > high[1] and eightDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
tenDUT = nineDUT[1] and high > high[1] and nineDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
elevenDUT = tenDUT[1] and high > high[1] and tenDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twelveDUT = elevenDUT[1] and high > high[1] and elevenDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
thirteenDUT = twelveDUT[1] and high > high[1] and twelveDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
fourteenDUT = thirteenDUT[1] and high > high[1] and thirteenDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
fifteenDUT = fourteenDUT[1] and high > high[1] and fourteenDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
sixteenDUT = fifteenDUT[1] and high > high[1] and fifteenDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
seventeenDUT = sixteenDUT[1] and high > high[1] and sixteenDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
eighteenDUT = seventeenDUT[1] and high > high[1] and seventeenDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
nineteenDUT = eighteenDUT[1] and high > high[1] and eighteenDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentyDUT = nineteenDUT[1] and high > high[1] and nineteenDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentyoneDUT = twentyDUT[1] and high > high[1] and twentyDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentytwoDUT = twentyoneDUT[1] and high > high[1] and twentyoneDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentythreeDUT = twentytwoDUT[1] and high > high[1] and twentytwoDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentyfourDUT = twentythreeDUT[1] and high > high[1] and twentythreeDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentyfiveDUT = twentyfourDUT[1] and high > high[1] and twentyfourDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentysixDUT = twentyfiveDUT[1] and high > high[1] and twentyfiveDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentysevenDUT = twentysixDUT[1] and high > high[1] and twentysixDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentyeightDUT = twentysevenDUT[1] and high > high[1] and twentysevenDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentynineDUT = twentyeightDUT[1] and high > high[1] and twentyeightDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
thirtyDUT = twentynineDUT[1] and high > high[1] and twentynineDUT[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter

oneDDT = high < high[1] and low < low[1] and (high[1] >= high[2] or low[1] >= low[2]) and barstate.isconfirmed and samplePeriodFilter
twoDDT = oneDDT[1] and high < high[1] and oneDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
threeDDT = twoDDT[1] and high < high[1] and twoDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
fourDDT = threeDDT[1] and high < high[1] and threeDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
fiveDDT = fourDDT[1] and high < high[1] and fourDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
sixDDT = fiveDDT[1] and high < high[1] and fiveDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
sevenDDT = sixDDT[1] and high < high[1] and sixDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
eightDDT = sevenDDT[1] and high < high[1] and sevenDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
nineDDT = eightDDT[1] and high < high[1] and eightDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
tenDDT = nineDDT[1] and high < high[1] and nineDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
elevenDDT = tenDDT[1] and high < high[1] and tenDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twelveDDT = elevenDDT[1] and high < high[1] and elevenDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
thirteenDDT = twelveDDT[1] and high < high[1] and twelveDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
fourteenDDT = thirteenDDT[1] and high < high[1] and thirteenDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
fifteenDDT = fourteenDDT[1] and high < high[1] and fourteenDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
sixteenDDT = fifteenDDT[1] and high < high[1] and fifteenDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
seventeenDDT = sixteenDDT[1] and high < high[1] and sixteenDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
eighteenDDT = seventeenDDT[1] and high < high[1] and seventeenDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
nineteenDDT = eighteenDDT[1] and high < high[1] and eighteenDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentyDDT = nineteenDDT[1] and high < high[1] and nineteenDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentyoneDDT = twentyDDT[1] and high < high[1] and twentyDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentytwoDDT = twentyoneDDT[1] and high < high[1] and twentyoneDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentythreeDDT = twentytwoDDT[1] and high < high[1] and twentytwoDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentyfourDDT = twentythreeDDT[1] and high < high[1] and twentythreeDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentyfiveDDT = twentyfourDDT[1] and high < high[1] and twentyfourDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentysixDDT = twentyfiveDDT[1] and high < high[1] and twentyfiveDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentysevenDDT = twentysixDDT[1] and high < high[1] and twentysixDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentyeightDDT = twentysevenDDT[1] and high < high[1] and twentysevenDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentynineDDT = twentyeightDDT[1] and high < high[1] and twentyeightDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
thirtyDDT = twentynineDDT[1] and high < high[1] and twentynineDDT[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter

var oneDUTcounter = 0
var twoDUTcounter = 0
var threeDUTcounter = 0
var fourDUTcounter = 0
var fiveDUTcounter = 0
var sixDUTcounter = 0
var sevenDUTcounter = 0
var eightDUTcounter = 0
var nineDUTcounter = 0
var tenDUTcounter = 0
var elevenDUTcounter = 0
var twelveDUTcounter = 0
var thirteenDUTcounter = 0
var fourteenDUTcounter = 0
var fifteenDUTcounter = 0
var sixteenDUTcounter = 0
var seventeenDUTcounter = 0
var eighteenDUTcounter = 0
var nineteenDUTcounter = 0
var twentyDUTcounter = 0
var twentyoneDUTcounter = 0
var twentytwoDUTcounter = 0
var twentythreeDUTcounter = 0
var twentyfourDUTcounter = 0
var twentyfiveDUTcounter = 0
var twentysixDUTcounter = 0
var twentysevenDUTcounter = 0
var twentyeightDUTcounter = 0
var twentynineDUTcounter = 0
var thirtyDUTcounter = 0

var oneDDTcounter = 0
var twoDDTcounter = 0
var threeDDTcounter = 0
var fourDDTcounter = 0
var fiveDDTcounter = 0
var sixDDTcounter = 0
var sevenDDTcounter = 0
var eightDDTcounter = 0
var nineDDTcounter = 0
var tenDDTcounter = 0
var elevenDDTcounter = 0
var twelveDDTcounter = 0
var thirteenDDTcounter = 0
var fourteenDDTcounter = 0
var fifteenDDTcounter = 0
var sixteenDDTcounter = 0
var seventeenDDTcounter = 0
var eighteenDDTcounter = 0
var nineteenDDTcounter = 0
var twentyDDTcounter = 0
var twentyoneDDTcounter = 0
var twentytwoDDTcounter = 0
var twentythreeDDTcounter = 0
var twentyfourDDTcounter = 0
var twentyfiveDDTcounter = 0
var twentysixDDTcounter = 0
var twentysevenDDTcounter = 0
var twentyeightDDTcounter = 0
var twentynineDDTcounter = 0
var thirtyDDTcounter = 0

if oneDUT
    oneDUTcounter += 1
if twoDUT
    twoDUTcounter += 1
if threeDUT
    threeDUTcounter += 1
if fourDUT
    fourDUTcounter += 1
if fiveDUT
    fiveDUTcounter += 1
if sixDUT
    sixDUTcounter += 1
if sevenDUT
    sevenDUTcounter += 1
if eightDUT
    eightDUTcounter += 1
if nineDUT
    nineDUTcounter += 1
if tenDUT
    tenDUTcounter += 1
if elevenDUT
    elevenDUTcounter += 1
if twelveDUT
    twelveDUTcounter += 1
if thirteenDUT
    thirteenDUTcounter += 1
if fourteenDUT
    fourteenDUTcounter += 1
if fifteenDUT
    fifteenDUTcounter += 1
if sixteenDUT
    sixteenDUTcounter += 1
if seventeenDUT
    seventeenDUTcounter += 1
if eighteenDUT
    eighteenDUTcounter += 1
if nineteenDUT
    nineteenDUTcounter += 1
if twentyDUT
    twentyDUTcounter += 1
if twentyoneDUT
    twentyoneDUTcounter += 1
if twentytwoDUT
    twentytwoDUTcounter += 1
if twentythreeDUT
    twentythreeDUTcounter += 1
if twentyfourDUT
    twentyfourDUTcounter += 1
if twentyfiveDUT
    twentyfiveDUTcounter += 1
if twentysixDUT
    twentysixDUTcounter += 1
if twentysevenDUT
    twentysevenDUTcounter += 1
if twentyeightDUT
    twentyeightDUTcounter += 1
if twentynineDUT
    twentynineDUTcounter += 1
if thirtyDUT
    thirtyDUTcounter += 1

if oneDDT
    oneDDTcounter += 1
if twoDDT
    twoDDTcounter += 1
if threeDDT
    threeDDTcounter += 1
if fourDDT
    fourDDTcounter += 1
if fiveDDT
    fiveDDTcounter += 1
if sixDDT
    sixDDTcounter += 1
if sevenDDT
    sevenDDTcounter += 1
if eightDDT
    eightDDTcounter += 1
if nineDDT
    nineDDTcounter += 1
if tenDDT
    tenDDTcounter += 1
if elevenDDT
    elevenDDTcounter += 1
if twelveDDT
    twelveDDTcounter += 1
if thirteenDDT
    thirteenDDTcounter += 1
if fourteenDDT
    fourteenDDTcounter += 1
if fifteenDDT
    fifteenDDTcounter += 1
if sixteenDDT
    sixteenDDTcounter += 1
if seventeenDDT
    seventeenDDTcounter += 1
if eighteenDDT
    eighteenDDTcounter += 1
if nineteenDDT
    nineteenDDTcounter += 1
if twentyDDT
    twentyDDTcounter += 1
if twentyoneDDT
    twentyoneDDTcounter += 1
if twentytwoDDT
    twentytwoDDTcounter += 1
if twentythreeDDT
    twentythreeDDTcounter += 1
if twentyfourDDT
    twentyfourDDTcounter += 1
if twentyfiveDDT
    twentyfiveDDTcounter += 1
if twentysixDDT
    twentysixDDTcounter += 1
if twentysevenDDT
    twentysevenDDTcounter += 1
if twentyeightDDT
    twentyeightDDTcounter += 1
if twentynineDDT
    twentynineDDTcounter += 1
if thirtyDDT
    thirtyDDTcounter += 1


////////////  table //////////// 


doubleCandleTrendTablePositionInput = input.string(title = 'Position', defval = 'Top Right', options=['Top Right', 'Top Center'], group = 'Table Positioning')
doubleCandleTrendTablePosition = doubleCandleTrendTablePositionInput == 'Top Right' ? position.top_right : doubleCandleTrendTablePositionInput == 'Top Center' ? position.top_center : 
 doubleCandleTrendTablePositionInput == 'Top Left' ? position.top_left : doubleCandleTrendTablePositionInput == 'Bottom Right' ? position.bottom_right : 
 doubleCandleTrendTablePositionInput == 'Bottom Center' ? position.bottom_center : doubleCandleTrendTablePositionInput == 'Bottom Left' ? position.bottom_left : 
 doubleCandleTrendTablePositionInput == 'Middle Right' ? position.middle_right : doubleCandleTrendTablePositionInput == 'Middle Center' ? position.middle_center : 
 doubleCandleTrendTablePositionInput == 'Middle Left' ? position.middle_left : na

textSizeInput = input.string(title = 'Text Size', defval = 'Normal', options = ['Tiny', 'Small', 'Normal', 'Large'], group = 'Table Text Sizing')
textSize = textSizeInput == 'Tiny' ? size.tiny : textSizeInput == 'Small' ? size.small : textSizeInput == 'Normal' ? size.normal : textSizeInput == 'Large' ? size.large : na

var doubleCandleTrendTable = table.new(doubleCandleTrendTablePosition, 100, 100, border_width=1)

table.cell(doubleCandleTrendTable, 1, 0, text = 'Double Uptrend Candle Trends', bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(doubleCandleTrendTable, 2, 0, text = '% Total', bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(doubleCandleTrendTable, 3, 0, text = '% Last', bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(doubleCandleTrendTable, 0, 1, text = '1-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
table.cell(doubleCandleTrendTable, 1, 1, text = str.tostring(oneDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(doubleCandleTrendTable, 2, 1, text = '-', bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(doubleCandleTrendTable, 3, 1, text = '-', bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(doubleCandleTrendTable, 4, 0, text = 'Double Downtrend Candle Trends', bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(doubleCandleTrendTable, 5, 0, text = '% Total', bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(doubleCandleTrendTable, 6, 0, text = '% Last', bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(doubleCandleTrendTable, 4, 1, text = str.tostring(oneDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(doubleCandleTrendTable, 5, 1, text = '-', bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(doubleCandleTrendTable, 6, 1, text = '-', bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twoDUTcounter >= 1 or twoDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 2, text = '2-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 2, text = str.tostring(twoDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 2, text = str.tostring(math.round(twoDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 2, text = '-', bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 2, text = str.tostring(twoDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 2, text = str.tostring(math.round(twoDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 2, text = '-', bgcolor = color.red, text_color = color.white, text_size = textSize)

if (threeDUTcounter >= 1 or threeDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 3, text = '3-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 3, text = str.tostring(threeDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 3, text = str.tostring(math.round(threeDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 3, text = str.tostring(math.round(threeDUTcounter / twoDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 3, text = str.tostring(threeDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 3, text = str.tostring(math.round(threeDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 3, text = str.tostring(math.round(threeDDTcounter / twoDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (fourDUTcounter >= 1 or fourDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 4, text = '4-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 4, text = str.tostring(fourDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 4, text = str.tostring(math.round(fourDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 4, text = str.tostring(math.round(fourDUTcounter / threeDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 4, text = str.tostring(fourDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 4, text = str.tostring(math.round(fourDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 4, text = str.tostring(math.round(fourDDTcounter / threeDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (fiveDUTcounter >= 1 or fiveDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 5, text = '5-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 5, text = str.tostring(fiveDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 5, text = str.tostring(math.round(fiveDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 5, text = str.tostring(math.round(fiveDUTcounter / fourDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 5, text = str.tostring(fiveDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 5, text = str.tostring(math.round(fiveDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 5, text = str.tostring(math.round(fiveDDTcounter / fourDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (sixDUTcounter >= 1 or sixDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 6, text = '6-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 6, text = str.tostring(sixDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 6, text = str.tostring(math.round(sixDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 6, text = str.tostring(math.round(sixDUTcounter / fiveDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 6, text = str.tostring(sixDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 6, text = str.tostring(math.round(sixDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 6, text = str.tostring(math.round(sixDDTcounter / fiveDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (sevenDUTcounter >= 1 or sevenDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 7, text = '7-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 7, text = str.tostring(sevenDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 7, text = str.tostring(math.round(sevenDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 7, text = str.tostring(math.round(sevenDUTcounter / sixDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 7, text = str.tostring(sevenDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 7, text = str.tostring(math.round(sevenDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 7, text = str.tostring(math.round(sevenDDTcounter / sixDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (eightDUTcounter >= 1 or eightDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 8, text = '8-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 8, text = str.tostring(eightDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 8, text = str.tostring(math.round(eightDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 8, text = str.tostring(math.round(eightDUTcounter / sevenDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 8, text = str.tostring(eightDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 8, text = str.tostring(math.round(eightDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 8, text = str.tostring(math.round(eightDDTcounter / sevenDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (nineDUTcounter >= 1 or nineDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 9, text = '9-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 9, text = str.tostring(nineDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 9, text = str.tostring(math.round(nineDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 9, text = str.tostring(math.round(nineDUTcounter / eightDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 9, text = str.tostring(nineDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 9, text = str.tostring(math.round(nineDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 9, text = str.tostring(math.round(nineDDTcounter / eightDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (tenDUTcounter >= 1 or tenDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 10, text = '10-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 10, text = str.tostring(tenDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 10, text = str.tostring(math.round(tenDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 10, text = str.tostring(math.round(tenDUTcounter / nineDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 10, text = str.tostring(tenDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 10, text = str.tostring(math.round(tenDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 10, text = str.tostring(math.round(tenDDTcounter / nineDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (elevenDUTcounter >= 1 or elevenDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 11, text = '11-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 11, text = str.tostring(elevenDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 11, text = str.tostring(math.round(elevenDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 11, text = str.tostring(math.round(elevenDUTcounter / tenDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 11, text = str.tostring(elevenDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 11, text = str.tostring(math.round(elevenDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 11, text = str.tostring(math.round(elevenDDTcounter / tenDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twelveDUTcounter >= 1 or twelveDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 12, text = '12-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 12, text = str.tostring(twelveDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 12, text = str.tostring(math.round(twelveDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 12, text = str.tostring(math.round(twelveDUTcounter / elevenDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 12, text = str.tostring(twelveDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 12, text = str.tostring(math.round(twelveDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 12, text = str.tostring(math.round(twelveDDTcounter / elevenDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (thirteenDUTcounter >= 1 or thirteenDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 13, text = '13-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 13, text = str.tostring(thirteenDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 13, text = str.tostring(math.round(thirteenDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 13, text = str.tostring(math.round(thirteenDUTcounter / twelveDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 13, text = str.tostring(thirteenDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 13, text = str.tostring(math.round(thirteenDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 13, text = str.tostring(math.round(thirteenDDTcounter / twelveDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (fourteenDUTcounter >= 1 or fourteenDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 14, text = '14-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 14, text = str.tostring(fourteenDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 14, text = str.tostring(math.round(fourteenDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 14, text = str.tostring(math.round(fourteenDUTcounter / thirteenDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 14, text = str.tostring(fourteenDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 14, text = str.tostring(math.round(fourteenDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 14, text = str.tostring(math.round(fourteenDDTcounter / thirteenDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (fifteenDUTcounter >= 1 or fifteenDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 15, text = '15-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 15, text = str.tostring(fifteenDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 15, text = str.tostring(math.round(fifteenDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 15, text = str.tostring(math.round(fifteenDUTcounter / fourteenDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 15, text = str.tostring(fifteenDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 15, text = str.tostring(math.round(fifteenDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 15, text = str.tostring(math.round(fifteenDDTcounter / fourteenDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (sixteenDUTcounter >= 1 or sixteenDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 16, text = '16-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 16, text = str.tostring(sixteenDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 16, text = str.tostring(math.round(sixteenDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 16, text = str.tostring(math.round(sixteenDUTcounter / fifteenDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 16, text = str.tostring(sixteenDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 16, text = str.tostring(math.round(sixteenDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 16, text = str.tostring(math.round(sixteenDDTcounter / fifteenDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (seventeenDUTcounter >= 1 or seventeenDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 17, text = '17-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 17, text = str.tostring(seventeenDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 17, text = str.tostring(math.round(seventeenDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 17, text = str.tostring(math.round(seventeenDUTcounter / sixteenDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 17, text = str.tostring(seventeenDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 17, text = str.tostring(math.round(seventeenDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 17, text = str.tostring(math.round(seventeenDDTcounter / sixteenDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (eighteenDUTcounter >= 1 or eighteenDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 18, text = '18-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 18, text = str.tostring(eighteenDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 18, text = str.tostring(math.round(eighteenDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 18, text = str.tostring(math.round(eighteenDUTcounter / seventeenDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 18, text = str.tostring(eighteenDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 18, text = str.tostring(math.round(eighteenDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 18, text = str.tostring(math.round(eighteenDDTcounter / seventeenDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (nineteenDUTcounter >= 1 or nineteenDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 19, text = '19-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 19, text = str.tostring(nineteenDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 19, text = str.tostring(math.round(nineteenDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 19, text = str.tostring(math.round(nineteenDUTcounter / eighteenDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 19, text = str.tostring(nineteenDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 19, text = str.tostring(math.round(nineteenDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 19, text = str.tostring(math.round(nineteenDDTcounter / eighteenDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentyDUTcounter >= 1 or twentyDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 20, text = '20-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 20, text = str.tostring(twentyDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 20, text = str.tostring(math.round(twentyDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 20, text = str.tostring(math.round(twentyDUTcounter / nineteenDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 20, text = str.tostring(twentyDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 20, text = str.tostring(math.round(twentyDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 20, text = str.tostring(math.round(twentyDDTcounter / nineteenDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentyoneDUTcounter >= 1 or twentyoneDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 21, text = '21-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 21, text = str.tostring(twentyoneDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 21, text = str.tostring(math.round(twentyoneDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 21, text = str.tostring(math.round(twentyoneDUTcounter / twentyDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 21, text = str.tostring(twentyoneDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 21, text = str.tostring(math.round(twentyoneDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 21, text = str.tostring(math.round(twentyoneDDTcounter / twentyDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentytwoDUTcounter >= 1 or twentytwoDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 22, text = '22-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 22, text = str.tostring(twentytwoDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 22, text = str.tostring(math.round(twentytwoDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 22, text = str.tostring(math.round(twentytwoDUTcounter / twentyoneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 22, text = str.tostring(twentytwoDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 22, text = str.tostring(math.round(twentytwoDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 22, text = str.tostring(math.round(twentytwoDDTcounter / twentyoneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentythreeDUTcounter >= 1 or twentythreeDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 23, text = '23-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 23, text = str.tostring(twentythreeDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 23, text = str.tostring(math.round(twentythreeDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 23, text = str.tostring(math.round(twentythreeDUTcounter / twentytwoDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 23, text = str.tostring(twentythreeDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 23, text = str.tostring(math.round(twentythreeDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 23, text = str.tostring(math.round(twentythreeDDTcounter / twentytwoDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentyfourDUTcounter >= 1 or twentyfourDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 24, text = '24-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 24, text = str.tostring(twentyfourDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 24, text = str.tostring(math.round(twentyfourDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 24, text = str.tostring(math.round(twentyfourDUTcounter / twentythreeDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 24, text = str.tostring(twentyfourDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 24, text = str.tostring(math.round(twentyfourDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 24, text = str.tostring(math.round(twentyfourDDTcounter / twentythreeDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentyfiveDUTcounter >= 1 or twentyfiveDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 25, text = '25-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 25, text = str.tostring(twentyfiveDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 25, text = str.tostring(math.round(twentyfiveDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 25, text = str.tostring(math.round(twentyfiveDUTcounter / twentyfourDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 25, text = str.tostring(twentyfiveDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 25, text = str.tostring(math.round(twentyfiveDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 25, text = str.tostring(math.round(twentyfiveDDTcounter / twentyfourDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentysixDUTcounter >= 1 or twentysixDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 26, text = '26-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 26, text = str.tostring(twentysixDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 26, text = str.tostring(math.round(twentysixDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 26, text = str.tostring(math.round(twentysixDUTcounter / twentyfiveDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 26, text = str.tostring(twentysixDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 26, text = str.tostring(math.round(twentysixDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 26, text = str.tostring(math.round(twentysixDDTcounter / twentyfiveDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentysevenDUTcounter >= 1 or twentysevenDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 27, text = '27-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 27, text = str.tostring(twentysevenDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 27, text = str.tostring(math.round(twentysevenDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 27, text = str.tostring(math.round(twentysevenDUTcounter / twentysixDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 27, text = str.tostring(twentysevenDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 27, text = str.tostring(math.round(twentysevenDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 27, text = str.tostring(math.round(twentysevenDDTcounter / twentysixDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentyeightDUTcounter >= 1 or twentyeightDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 28, text = '28-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 28, text = str.tostring(twentyeightDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 28, text = str.tostring(math.round(twentyeightDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 28, text = str.tostring(math.round(twentyeightDUTcounter / twentysevenDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 28, text = str.tostring(twentyeightDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 28, text = str.tostring(math.round(twentyeightDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 28, text = str.tostring(math.round(twentyeightDDTcounter / twentysevenDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentynineDUTcounter >= 1 or twentynineDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 29, text = '29-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 29, text = str.tostring(twentynineDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 29, text = str.tostring(math.round(twentynineDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 29, text = str.tostring(math.round(twentynineDUTcounter / twentyeightDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 29, text = str.tostring(twentynineDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 29, text = str.tostring(math.round(twentynineDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 29, text = str.tostring(math.round(twentynineDDTcounter / twentyeightDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (thirtyDUTcounter >= 1 or thirtyDDTcounter >= 1)
    table.cell(doubleCandleTrendTable, 0, 30, text = '30-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 1, 30, text = str.tostring(thirtyDUTcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 2, 30, text = str.tostring(math.round(thirtyDUTcounter / oneDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 3, 30, text = str.tostring(math.round(thirtyDUTcounter / twentynineDUTcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 4, 30, text = str.tostring(thirtyDDTcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 5, 30, text = str.tostring(math.round(thirtyDDTcounter / oneDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(doubleCandleTrendTable, 6, 30, text = str.tostring(math.round(thirtyDDTcounter / twentynineDDTcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if showSamplePeriod 
    table.cell(doubleCandleTrendTable, 0, 31, text = startDateText + endDateText, bgcolor = color.black, text_color = color.white, text_size = textSize)


//////////// plots ////////////


showPlots = input(defval = true, title = "Show Plots")

plotshape(oneDUT and showPlots, style = shape.triangleup, color = color.green, text = '1', textcolor = color.green, location = location.belowbar) 
plotshape(twoDUT and showPlots, style = shape.triangleup, color = color.green, text = '2', textcolor = color.green, location = location.belowbar) 
plotshape(threeDUT and showPlots, style = shape.triangleup, color = color.green, text = '3', textcolor = color.green, location = location.belowbar)
plotshape(fourDUT and showPlots, style = shape.triangleup, color = color.green, text = '4', textcolor = color.green, location = location.belowbar)
plotshape(fiveDUT and showPlots, style = shape.triangleup, color = color.green, text = '5', textcolor = color.green, location = location.belowbar)
plotshape(sixDUT and showPlots, style = shape.triangleup, color = color.green, text = '6', textcolor = color.green, location = location.belowbar)
plotshape(sevenDUT and showPlots, style = shape.triangleup, color = color.green, text = '7', textcolor = color.green, location = location.belowbar)
plotshape(eightDUT and showPlots, style = shape.triangleup, color = color.green, text = '8', textcolor = color.green, location = location.belowbar)
plotshape(nineDUT and showPlots, style = shape.triangleup, color = color.green, text = '9', textcolor = color.green, location = location.belowbar)
plotshape(tenDUT and showPlots, style = shape.triangleup, color = color.green, text = '10', textcolor = color.green, location = location.belowbar)
plotshape(elevenDUT and showPlots, style = shape.triangleup, color = color.green, text = '11', textcolor = color.green, location = location.belowbar)
plotshape(twelveDUT and showPlots, style = shape.triangleup, color = color.green, text = '12', textcolor = color.green, location = location.belowbar)
plotshape(thirteenDUT and showPlots, style = shape.triangleup, color = color.green, text = '13', textcolor = color.green, location = location.belowbar)
plotshape(fourteenDUT and showPlots, style = shape.triangleup, color = color.green, text = '14', textcolor = color.green, location = location.belowbar)
plotshape(fifteenDUT and showPlots, style = shape.triangleup, color = color.green, text = '15', textcolor = color.green, location = location.belowbar)
plotshape(sixteenDUT and showPlots, style = shape.triangleup, color = color.green, text = '16', textcolor = color.green, location = location.belowbar)
plotshape(seventeenDUT and showPlots, style = shape.triangleup, color = color.green, text = '17', textcolor = color.green, location = location.belowbar)
plotshape(eighteenDUT and showPlots, style = shape.triangleup, color = color.green, text = '18', textcolor = color.green, location = location.belowbar)
plotshape(nineteenDUT and showPlots, style = shape.triangleup, color = color.green, text = '19', textcolor = color.green, location = location.belowbar)
plotshape(twentyDUT and showPlots, style = shape.triangleup, color = color.green, text = '20', textcolor = color.green, location = location.belowbar)
plotshape(twentyoneDUT and showPlots, style = shape.triangleup, color = color.green, text = '21', textcolor = color.green, location = location.belowbar)
plotshape(twentytwoDUT and showPlots, style = shape.triangleup, color = color.green, text = '22', textcolor = color.green, location = location.belowbar)
plotshape(twentythreeDUT and showPlots, style = shape.triangleup, color = color.green, text = '23', textcolor = color.green, location = location.belowbar)
plotshape(twentyfourDUT and showPlots, style = shape.triangleup, color = color.green, text = '24', textcolor = color.green, location = location.belowbar)
plotshape(twentyfiveDUT and showPlots, style = shape.triangleup, color = color.green, text = '25', textcolor = color.green, location = location.belowbar)
plotshape(twentysixDUT and showPlots, style = shape.triangleup, color = color.green, text = '26', textcolor = color.green, location = location.belowbar)
plotshape(twentysevenDUT and showPlots, style = shape.triangleup, color = color.green, text = '27', textcolor = color.green, location = location.belowbar)
plotshape(twentyeightDUT and showPlots, style = shape.triangleup, color = color.green, text = '28', textcolor = color.green, location = location.belowbar)
plotshape(twentynineDUT and showPlots, style = shape.triangleup, color = color.green, text = '29', textcolor = color.green, location = location.belowbar)
plotshape(thirtyDUT and showPlots, style = shape.triangleup, color = color.green, text = '30', textcolor = color.green, location = location.belowbar)

plotshape(oneDDT and showPlots, style = shape.triangledown, color = color.red, text = '1', textcolor = color.red) 
plotshape(twoDDT and showPlots, style = shape.triangledown, color = color.red, text = '2', textcolor = color.red) 
plotshape(threeDDT and showPlots, style = shape.triangledown, color = color.red, text = '3', textcolor = color.red)
plotshape(fourDDT and showPlots, style = shape.triangledown, color = color.red, text = '4', textcolor = color.red)
plotshape(fiveDDT and showPlots, style = shape.triangledown, color = color.red, text = '5', textcolor = color.red)
plotshape(sixDDT and showPlots, style = shape.triangledown, color = color.red, text = '6', textcolor = color.red)
plotshape(sevenDDT and showPlots, style = shape.triangledown, color = color.red, text = '7', textcolor = color.red)
plotshape(eightDDT and showPlots, style = shape.triangledown, color = color.red, text = '8', textcolor = color.red)
plotshape(nineDDT and showPlots, style = shape.triangledown, color = color.red, text = '9', textcolor = color.red)
plotshape(tenDDT and showPlots, style = shape.triangledown, color = color.red, text = '10', textcolor = color.red)
plotshape(elevenDDT and showPlots, style = shape.triangledown, color = color.red, text = '11', textcolor = color.red)
plotshape(twelveDDT and showPlots, style = shape.triangledown, color = color.red, text = '12', textcolor = color.red)
plotshape(thirteenDDT and showPlots, style = shape.triangledown, color = color.red, text = '13', textcolor = color.red)
plotshape(fourteenDDT and showPlots, style = shape.triangledown, color = color.red, text = '14', textcolor = color.red)
plotshape(fifteenDDT and showPlots, style = shape.triangledown, color = color.red, text = '15', textcolor = color.red)
plotshape(sixteenDDT and showPlots, style = shape.triangledown, color = color.red, text = '16', textcolor = color.red)
plotshape(seventeenDDT and showPlots, style = shape.triangledown, color = color.red, text = '17', textcolor = color.red)
plotshape(eighteenDDT and showPlots, style = shape.triangledown, color = color.red, text = '18', textcolor = color.red)
plotshape(nineteenDDT and showPlots, style = shape.triangledown, color = color.red, text = '19', textcolor = color.red)
plotshape(twentyDDT and showPlots, style = shape.triangledown, color = color.red, text = '20', textcolor = color.red)
plotshape(twentyoneDDT and showPlots, style = shape.triangledown, color = color.red, text = '21', textcolor = color.red)
plotshape(twentytwoDDT and showPlots, style = shape.triangledown, color = color.red, text = '22', textcolor = color.red)
plotshape(twentythreeDDT and showPlots, style = shape.triangledown, color = color.red, text = '23', textcolor = color.red)
plotshape(twentyfourDDT and showPlots, style = shape.triangledown, color = color.red, text = '24', textcolor = color.red)
plotshape(twentyfiveDDT and showPlots, style = shape.triangledown, color = color.red, text = '25', textcolor = color.red)
plotshape(twentysixDDT and showPlots, style = shape.triangledown, color = color.red, text = '26', textcolor = color.red)
plotshape(twentysevenDDT and showPlots, style = shape.triangledown, color = color.red, text = '27', textcolor = color.red)
plotshape(twentyeightDDT and showPlots, style = shape.triangledown, color = color.red, text = '28', textcolor = color.red)
plotshape(twentynineDDT and showPlots, style = shape.triangledown, color = color.red, text = '29', textcolor = color.red)
plotshape(thirtyDDT and showPlots, style = shape.triangledown, color = color.red, text = '30', textcolor = color.red)

