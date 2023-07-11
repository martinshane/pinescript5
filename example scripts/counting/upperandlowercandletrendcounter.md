// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © theEccentricTrader
//@version=5


indicator('Upper and Lower Candle Trend Counter [theEccentricTrader]', overlay = true)


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


oneHH = high[1] <= high[2] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
twoHH = oneHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
threeHH = twoHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
fourHH = threeHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
fiveHH = fourHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
sixHH = fiveHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
sevenHH = sixHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
eightHH = sevenHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
nineHH = eightHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
tenHH = nineHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
elevenHH = tenHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
twelveHH = elevenHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
thirteenHH = twelveHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
fourteenHH = thirteenHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
fifteenHH = fourteenHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
sixteenHH = fifteenHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
seventeenHH = sixteenHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
eighteenHH = seventeenHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
nineteenHH = eighteenHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
twentyHH = nineteenHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
twentyoneHH = twentyHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
twentytwoHH = twentyoneHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
twentythreeHH = twentytwoHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
twentyfourHH = twentythreeHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
twentyfiveHH = twentyfourHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
twentysixHH = twentyfiveHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
twentysevenHH = twentysixHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
twentyeightHH = twentysevenHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
twentynineHH = twentyeightHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter
thirtyHH = twentynineHH[1] and high > high[1] and barstate.isconfirmed and samplePeriodFilter

oneHL = low[1] <= low[2] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twoHL = oneHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
threeHL = twoHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
fourHL = threeHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
fiveHL = fourHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
sixHL = fiveHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
sevenHL = sixHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
eightHL = sevenHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
nineHL = eightHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
tenHL = nineHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
elevenHL = tenHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twelveHL = elevenHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
thirteenHL = twelveHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
fourteenHL = thirteenHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
fifteenHL = fourteenHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
sixteenHL = fifteenHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
seventeenHL = sixteenHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
eighteenHL = seventeenHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
nineteenHL = eighteenHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentyHL = nineteenHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentyoneHL = twentyHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentytwoHL = twentyoneHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentythreeHL = twentytwoHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentyfourHL = twentythreeHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentyfiveHL = twentyfourHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentysixHL = twentyfiveHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentysevenHL = twentysixHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentyeightHL = twentysevenHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
twentynineHL = twentyeightHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter
thirtyHL = twentynineHL[1] and low > low[1] and barstate.isconfirmed and samplePeriodFilter

oneLH = high[1] >= high[2] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
twoLH = oneLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
threeLH = twoLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
fourLH = threeLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
fiveLH = fourLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
sixLH = fiveLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
sevenLH = sixLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
eightLH = sevenLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
nineLH = eightLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
tenLH = nineLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
elevenLH = tenLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
twelveLH = elevenLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
thirteenLH = twelveLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
fourteenLH = thirteenLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
fifteenLH = fourteenLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
sixteenLH = fifteenLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
seventeenLH = sixteenLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
eighteenLH = seventeenLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
nineteenLH = eighteenLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
twentyLH = nineteenLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
twentyoneLH = twentyLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
twentytwoLH = twentyoneLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
twentythreeLH = twentytwoLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
twentyfourLH = twentythreeLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
twentyfiveLH = twentyfourLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
twentysixLH = twentyfiveLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
twentysevenLH = twentysixLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
twentyeightLH = twentysevenLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
twentynineLH = twentyeightLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter
thirtyLH = twentynineLH[1] and high < high[1] and barstate.isconfirmed and samplePeriodFilter

oneLL = low[1] >= low[2] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twoLL = oneLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
threeLL = twoLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
fourLL = threeLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
fiveLL = fourLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
sixLL = fiveLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
sevenLL = sixLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
eightLL = sevenLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
nineLL = eightLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
tenLL = nineLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
elevenLL = tenLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twelveLL = elevenLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
thirteenLL = twelveLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
fourteenLL = thirteenLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
fifteenLL = fourteenLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
sixteenLL = fifteenLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
seventeenLL = sixteenLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
eighteenLL = seventeenLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
nineteenLL = eighteenLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentyLL = nineteenLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentyoneLL = twentyLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentytwoLL = twentyoneLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentythreeLL = twentytwoLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentyfourLL = twentythreeLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentyfiveLL = twentyfourLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentysixLL = twentyfiveLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentysevenLL = twentysixLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentyeightLL = twentysevenLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
twentynineLL = twentyeightLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter
thirtyLL = twentynineLL[1] and low < low[1] and barstate.isconfirmed and samplePeriodFilter

var oneHHcounter = 0
var twoHHcounter = 0
var threeHHcounter = 0
var fourHHcounter = 0
var fiveHHcounter = 0
var sixHHcounter = 0
var sevenHHcounter = 0
var eightHHcounter = 0
var nineHHcounter = 0
var tenHHcounter = 0
var elevenHHcounter = 0
var twelveHHcounter = 0
var thirteenHHcounter = 0
var fourteenHHcounter = 0
var fifteenHHcounter = 0
var sixteenHHcounter = 0
var seventeenHHcounter = 0
var eighteenHHcounter = 0
var nineteenHHcounter = 0
var twentyHHcounter = 0
var twentyoneHHcounter = 0
var twentytwoHHcounter = 0
var twentythreeHHcounter = 0
var twentyfourHHcounter = 0
var twentyfiveHHcounter = 0
var twentysixHHcounter = 0
var twentysevenHHcounter = 0
var twentyeightHHcounter = 0
var twentynineHHcounter = 0
var thirtyHHcounter = 0

var oneHLcounter = 0
var twoHLcounter = 0
var threeHLcounter = 0
var fourHLcounter = 0
var fiveHLcounter = 0
var sixHLcounter = 0
var sevenHLcounter = 0
var eightHLcounter = 0
var nineHLcounter = 0
var tenHLcounter = 0
var elevenHLcounter = 0
var twelveHLcounter = 0
var thirteenHLcounter = 0
var fourteenHLcounter = 0
var fifteenHLcounter = 0
var sixteenHLcounter = 0
var seventeenHLcounter = 0
var eighteenHLcounter = 0
var nineteenHLcounter = 0
var twentyHLcounter = 0
var twentyoneHLcounter = 0
var twentytwoHLcounter = 0
var twentythreeHLcounter = 0
var twentyfourHLcounter = 0
var twentyfiveHLcounter = 0
var twentysixHLcounter = 0
var twentysevenHLcounter = 0
var twentyeightHLcounter = 0
var twentynineHLcounter = 0
var thirtyHLcounter = 0

var oneLHcounter = 0
var twoLHcounter = 0
var threeLHcounter = 0
var fourLHcounter = 0
var fiveLHcounter = 0
var sixLHcounter = 0
var sevenLHcounter = 0
var eightLHcounter = 0
var nineLHcounter = 0
var tenLHcounter = 0
var elevenLHcounter = 0
var twelveLHcounter = 0
var thirteenLHcounter = 0
var fourteenLHcounter = 0
var fifteenLHcounter = 0
var sixteenLHcounter = 0
var seventeenLHcounter = 0
var eighteenLHcounter = 0
var nineteenLHcounter = 0
var twentyLHcounter = 0
var twentyoneLHcounter = 0
var twentytwoLHcounter = 0
var twentythreeLHcounter = 0
var twentyfourLHcounter = 0
var twentyfiveLHcounter = 0
var twentysixLHcounter = 0
var twentysevenLHcounter = 0
var twentyeightLHcounter = 0
var twentynineLHcounter = 0
var thirtyLHcounter = 0

var oneLLcounter = 0
var twoLLcounter = 0
var threeLLcounter = 0
var fourLLcounter = 0
var fiveLLcounter = 0
var sixLLcounter = 0
var sevenLLcounter = 0
var eightLLcounter = 0
var nineLLcounter = 0
var tenLLcounter = 0
var elevenLLcounter = 0
var twelveLLcounter = 0
var thirteenLLcounter = 0
var fourteenLLcounter = 0
var fifteenLLcounter = 0
var sixteenLLcounter = 0
var seventeenLLcounter = 0
var eighteenLLcounter = 0
var nineteenLLcounter = 0
var twentyLLcounter = 0
var twentyoneLLcounter = 0
var twentytwoLLcounter = 0
var twentythreeLLcounter = 0
var twentyfourLLcounter = 0
var twentyfiveLLcounter = 0
var twentysixLLcounter = 0
var twentysevenLLcounter = 0
var twentyeightLLcounter = 0
var twentynineLLcounter = 0
var thirtyLLcounter = 0

if oneHH
    oneHHcounter += 1
if twoHH
    twoHHcounter += 1
if threeHH
    threeHHcounter += 1
if fourHH
    fourHHcounter += 1
if fiveHH
    fiveHHcounter += 1
if sixHH
    sixHHcounter += 1
if sevenHH
    sevenHHcounter += 1
if eightHH
    eightHHcounter += 1
if nineHH
    nineHHcounter += 1
if tenHH
    tenHHcounter += 1
if elevenHH
    elevenHHcounter += 1
if twelveHH
    twelveHHcounter += 1
if thirteenHH
    thirteenHHcounter += 1
if fourteenHH
    fourteenHHcounter += 1
if fifteenHH
    fifteenHHcounter += 1
if sixteenHH
    sixteenHHcounter += 1
if seventeenHH
    seventeenHHcounter += 1
if eighteenHH
    eighteenHHcounter += 1
if nineteenHH
    nineteenHHcounter += 1
if twentyHH
    twentyHHcounter += 1
if twentyoneHH
    twentyoneHHcounter += 1
if twentytwoHH
    twentytwoHHcounter += 1
if twentythreeHH
    twentythreeHHcounter += 1
if twentyfourHH
    twentyfourHHcounter += 1
if twentyfiveHH
    twentyfiveHHcounter += 1
if twentysixHH
    twentysixHHcounter += 1
if twentysevenHH
    twentysevenHHcounter += 1
if twentyeightHH
    twentyeightHHcounter += 1
if twentynineHH
    twentynineHHcounter += 1
if thirtyHH
    thirtyHHcounter += 1

if oneHL
    oneHLcounter += 1
if twoHL
    twoHLcounter += 1
if threeHL
    threeHLcounter += 1
if fourHL
    fourHLcounter += 1
if fiveHL
    fiveHLcounter += 1
if sixHL
    sixHLcounter += 1
if sevenHL
    sevenHLcounter += 1
if eightHL
    eightHLcounter += 1
if nineHL
    nineHLcounter += 1
if tenHL
    tenHLcounter += 1
if elevenHL
    elevenHLcounter += 1
if twelveHL
    twelveHLcounter += 1
if thirteenHL
    thirteenHLcounter += 1
if fourteenHL
    fourteenHLcounter += 1
if fifteenHL
    fifteenHLcounter += 1
if sixteenHL
    sixteenHLcounter += 1
if seventeenHL
    seventeenHLcounter += 1
if eighteenHL
    eighteenHLcounter += 1
if nineteenHL
    nineteenHLcounter += 1
if twentyHL
    twentyHLcounter += 1
if twentyoneHL
    twentyoneHLcounter += 1
if twentytwoHL
    twentytwoHLcounter += 1
if twentythreeHL
    twentythreeHLcounter += 1
if twentyfourHL
    twentyfourHLcounter += 1
if twentyfiveHL
    twentyfiveHLcounter += 1
if twentysixHL
    twentysixHLcounter += 1
if twentysevenHL
    twentysevenHLcounter += 1
if twentyeightHL
    twentyeightHLcounter += 1
if twentynineHL
    twentynineHLcounter += 1
if thirtyHL
    thirtyHLcounter += 1

if oneLH
    oneLHcounter += 1
if twoLH
    twoLHcounter += 1
if threeLH
    threeLHcounter += 1
if fourLH
    fourLHcounter += 1
if fiveLH
    fiveLHcounter += 1
if sixLH
    sixLHcounter += 1
if sevenLH
    sevenLHcounter += 1
if eightLH
    eightLHcounter += 1
if nineLH
    nineLHcounter += 1
if tenLH
    tenLHcounter += 1
if elevenLH
    elevenLHcounter += 1
if twelveLH
    twelveLHcounter += 1
if thirteenLH
    thirteenLHcounter += 1
if fourteenLH
    fourteenLHcounter += 1
if fifteenLH
    fifteenLHcounter += 1
if sixteenLH
    sixteenLHcounter += 1
if seventeenLH
    seventeenLHcounter += 1
if eighteenLH
    eighteenLHcounter += 1
if nineteenLH
    nineteenLHcounter += 1
if twentyLH
    twentyLHcounter += 1
if twentyoneLH
    twentyoneLHcounter += 1
if twentytwoLH
    twentytwoLHcounter += 1
if twentythreeLH
    twentythreeLHcounter += 1
if twentyfourLH
    twentyfourLHcounter += 1
if twentyfiveLH
    twentyfiveLHcounter += 1
if twentysixLH
    twentysixLHcounter += 1
if twentysevenLH
    twentysevenLHcounter += 1
if twentyeightLH
    twentyeightLHcounter += 1
if twentynineLH
    twentynineLHcounter += 1
if thirtyLH
    thirtyLHcounter += 1

if oneLL
    oneLLcounter += 1
if twoLL
    twoLLcounter += 1
if threeLL
    threeLLcounter += 1
if fourLL
    fourLLcounter += 1
if fiveLL
    fiveLLcounter += 1
if sixLL
    sixLLcounter += 1
if sevenLL
    sevenLLcounter += 1
if eightLL
    eightLLcounter += 1
if nineLL
    nineLLcounter += 1
if tenLL
    tenLLcounter += 1
if elevenLL
    elevenLLcounter += 1
if twelveLL
    twelveLLcounter += 1
if thirteenLL
    thirteenLLcounter += 1
if fourteenLL
    fourteenLLcounter += 1
if fifteenLL
    fifteenLLcounter += 1
if sixteenLL
    sixteenLLcounter += 1
if seventeenLL
    seventeenLLcounter += 1
if eighteenLL
    eighteenLLcounter += 1
if nineteenLL
    nineteenLLcounter += 1
if twentyLL
    twentyLLcounter += 1
if twentyoneLL
    twentyoneLLcounter += 1
if twentytwoLL
    twentytwoLLcounter += 1
if twentythreeLL
    twentythreeLLcounter += 1
if twentyfourLL
    twentyfourLLcounter += 1
if twentyfiveLL
    twentyfiveLLcounter += 1
if twentysixLL
    twentysixLLcounter += 1
if twentysevenLL
    twentysevenLLcounter += 1
if twentyeightLL
    twentyeightLLcounter += 1
if twentynineLL
    twentynineLLcounter += 1
if thirtyLL
    thirtyLLcounter += 1


////////////  table //////////// 


upperLowerCandleTrendTablePositionInput = input.string(title = 'Position', defval = 'Top Right', options=['Top Right', 'Top Center'], group = 'Table Positioning')
upperLowerCandleTrendTablePosition = upperLowerCandleTrendTablePositionInput == 'Top Right' ? position.top_right : upperLowerCandleTrendTablePositionInput == 'Top Center' ? position.top_center : 
 upperLowerCandleTrendTablePositionInput == 'Top Left' ? position.top_left : upperLowerCandleTrendTablePositionInput == 'Bottom Right' ? position.bottom_right : 
 upperLowerCandleTrendTablePositionInput == 'Bottom Center' ? position.bottom_center : upperLowerCandleTrendTablePositionInput == 'Bottom Left' ? position.bottom_left : 
 upperLowerCandleTrendTablePositionInput == 'Middle Right' ? position.middle_right : upperLowerCandleTrendTablePositionInput == 'Middle Center' ? position.middle_center : 
 upperLowerCandleTrendTablePositionInput == 'Middle Left' ? position.middle_left : na

textSizeInput = input.string(title = 'Text Size', defval = 'Tiny', options = ['Tiny', 'Small', 'Normal', 'Large'], group = 'Table Text Sizing')
textSize = textSizeInput == 'Tiny' ? size.tiny : textSizeInput == 'Small' ? size.small : textSizeInput == 'Normal' ? size.normal : textSizeInput == 'Large' ? size.large : na

var upperLowerCandleTrendTable = table.new(upperLowerCandleTrendTablePosition, 100, 100, border_width=1)

table.cell(upperLowerCandleTrendTable, 1, 0, text = 'Higher Highs', bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 2, 0, text = '% Total', bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 3, 0, text = '% Last', bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 0, 1, text = '1-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 1, 1, text = str.tostring(oneHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 2, 1, text = '-', bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 3, 1, text = '-', bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 4, 0, text = 'Lower Highs', bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 5, 0, text = '% Total', bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 6, 0, text = '% Last', bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 4, 1, text = str.tostring(oneLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 5, 1, text = '-', bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 6, 1, text = '-', bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twoHHcounter >= 1 or twoLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 2, text = '2-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 2, text = str.tostring(twoHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 2, text = str.tostring(math.round(twoHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 2, text = '-', bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 2, text = str.tostring(twoLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 2, text = str.tostring(math.round(twoLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 2, text = '-', bgcolor = color.red, text_color = color.white, text_size = textSize)

if (threeHHcounter >= 1 or threeLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 3, text = '3-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 3, text = str.tostring(threeHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 3, text = str.tostring(math.round(threeHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 3, text = str.tostring(math.round(threeHHcounter / twoHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 3, text = str.tostring(threeLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 3, text = str.tostring(math.round(threeLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 3, text = str.tostring(math.round(threeLHcounter / twoLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (fourHHcounter >= 1 or fourLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 4, text = '4-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 4, text = str.tostring(fourHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 4, text = str.tostring(math.round(fourHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 4, text = str.tostring(math.round(fourHHcounter / threeHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 4, text = str.tostring(fourLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 4, text = str.tostring(math.round(fourLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 4, text = str.tostring(math.round(fourLHcounter / threeLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (fiveHHcounter >= 1 or fiveLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 5, text = '5-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 5, text = str.tostring(fiveHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 5, text = str.tostring(math.round(fiveHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 5, text = str.tostring(math.round(fiveHHcounter / fourHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 5, text = str.tostring(fiveLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 5, text = str.tostring(math.round(fiveLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 5, text = str.tostring(math.round(fiveLHcounter / fourLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (sixHHcounter >= 1 or sixLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 6, text = '6-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 6, text = str.tostring(sixHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 6, text = str.tostring(math.round(sixHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 6, text = str.tostring(math.round(sixHHcounter / fiveHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 6, text = str.tostring(sixLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 6, text = str.tostring(math.round(sixLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 6, text = str.tostring(math.round(sixLHcounter / fiveLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (sevenHHcounter >= 1 or sevenLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 7, text = '7-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 7, text = str.tostring(sevenHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 7, text = str.tostring(math.round(sevenHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 7, text = str.tostring(math.round(sevenHHcounter / sixHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 7, text = str.tostring(sevenLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 7, text = str.tostring(math.round(sevenLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 7, text = str.tostring(math.round(sevenLHcounter / sixLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (eightHHcounter >= 1 or eightLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 8, text = '8-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 8, text = str.tostring(eightHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 8, text = str.tostring(math.round(eightHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 8, text = str.tostring(math.round(eightHHcounter / sevenHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 8, text = str.tostring(eightLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 8, text = str.tostring(math.round(eightLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 8, text = str.tostring(math.round(eightLHcounter / sevenLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (nineHHcounter >= 1 or nineLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 9, text = '9-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 9, text = str.tostring(nineHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 9, text = str.tostring(math.round(nineHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 9, text = str.tostring(math.round(nineHHcounter / eightHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 9, text = str.tostring(nineLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 9, text = str.tostring(math.round(nineLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 9, text = str.tostring(math.round(nineLHcounter / eightLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (tenHHcounter >= 1 or tenLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 10, text = '10-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 10, text = str.tostring(tenHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 10, text = str.tostring(math.round(tenHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 10, text = str.tostring(math.round(tenHHcounter / nineHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 10, text = str.tostring(tenLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 10, text = str.tostring(math.round(tenLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 10, text = str.tostring(math.round(tenLHcounter / nineLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (elevenHHcounter >= 1 or elevenLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 11, text = '11-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 11, text = str.tostring(elevenHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 11, text = str.tostring(math.round(elevenHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 11, text = str.tostring(math.round(elevenHHcounter / tenHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 11, text = str.tostring(elevenLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 11, text = str.tostring(math.round(elevenLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 11, text = str.tostring(math.round(elevenLHcounter / tenLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twelveHHcounter >= 1 or twelveLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 12, text = '12-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 12, text = str.tostring(twelveHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 12, text = str.tostring(math.round(twelveHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 12, text = str.tostring(math.round(twelveHHcounter / elevenHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 12, text = str.tostring(twelveLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 12, text = str.tostring(math.round(twelveLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 12, text = str.tostring(math.round(twelveLHcounter / elevenLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (thirteenHHcounter >= 1 or thirteenLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 13, text = '13-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 13, text = str.tostring(thirteenHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 13, text = str.tostring(math.round(thirteenHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 13, text = str.tostring(math.round(thirteenHHcounter / twelveHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 13, text = str.tostring(thirteenLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 13, text = str.tostring(math.round(thirteenLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 13, text = str.tostring(math.round(thirteenLHcounter / twelveLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (fourteenHHcounter >= 1 or fourteenLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 14, text = '14-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 14, text = str.tostring(fourteenHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 14, text = str.tostring(math.round(fourteenHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 14, text = str.tostring(math.round(fourteenHHcounter / thirteenHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 14, text = str.tostring(fourteenLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 14, text = str.tostring(math.round(fourteenLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 14, text = str.tostring(math.round(fourteenLHcounter / thirteenLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (fifteenHHcounter >= 1 or fifteenLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 15, text = '15-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 15, text = str.tostring(fifteenHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 15, text = str.tostring(math.round(fifteenHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 15, text = str.tostring(math.round(fifteenHHcounter / fourteenHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 15, text = str.tostring(fifteenLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 15, text = str.tostring(math.round(fifteenLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 15, text = str.tostring(math.round(fifteenLHcounter / fourteenLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (sixteenHHcounter >= 1 or sixteenLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 16, text = '16-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 16, text = str.tostring(sixteenHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 16, text = str.tostring(math.round(sixteenHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 16, text = str.tostring(math.round(sixteenHHcounter / fifteenHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 16, text = str.tostring(sixteenLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 16, text = str.tostring(math.round(sixteenLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 16, text = str.tostring(math.round(sixteenLHcounter / fifteenLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (seventeenHHcounter >= 1 or seventeenLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 17, text = '17-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 17, text = str.tostring(seventeenHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 17, text = str.tostring(math.round(seventeenHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 17, text = str.tostring(math.round(seventeenHHcounter / sixteenHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 17, text = str.tostring(seventeenLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 17, text = str.tostring(math.round(seventeenLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 17, text = str.tostring(math.round(seventeenLHcounter / sixteenLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (eighteenHHcounter >= 1 or eighteenLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 18, text = '18-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 18, text = str.tostring(eighteenHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 18, text = str.tostring(math.round(eighteenHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 18, text = str.tostring(math.round(eighteenHHcounter / seventeenHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 18, text = str.tostring(eighteenLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 18, text = str.tostring(math.round(eighteenLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 18, text = str.tostring(math.round(eighteenLHcounter / seventeenLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (nineteenHHcounter >= 1 or nineteenLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 19, text = '19-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 19, text = str.tostring(nineteenHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 19, text = str.tostring(math.round(nineteenHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 19, text = str.tostring(math.round(nineteenHHcounter / eighteenHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 19, text = str.tostring(nineteenLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 19, text = str.tostring(math.round(nineteenLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 19, text = str.tostring(math.round(nineteenLHcounter / eighteenLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentyHHcounter >= 1 or twentyLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 20, text = '20-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 20, text = str.tostring(twentyHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 20, text = str.tostring(math.round(twentyHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 20, text = str.tostring(math.round(twentyHHcounter / nineteenHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 20, text = str.tostring(twentyLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 20, text = str.tostring(math.round(twentyLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 20, text = str.tostring(math.round(twentyLHcounter / nineteenLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentyoneHHcounter >= 1 or twentyoneLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 21, text = '21-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 21, text = str.tostring(twentyoneHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 21, text = str.tostring(math.round(twentyoneHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 21, text = str.tostring(math.round(twentyoneHHcounter / twentyHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 21, text = str.tostring(twentyoneLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 21, text = str.tostring(math.round(twentyoneLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 21, text = str.tostring(math.round(twentyoneLHcounter / twentyLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentytwoHHcounter >= 1 or twentytwoLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 22, text = '22-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 22, text = str.tostring(twentytwoHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 22, text = str.tostring(math.round(twentytwoHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 22, text = str.tostring(math.round(twentytwoHHcounter / twentyoneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 22, text = str.tostring(twentytwoLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 22, text = str.tostring(math.round(twentytwoLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 22, text = str.tostring(math.round(twentytwoLHcounter / twentyoneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentythreeHHcounter >= 1 or twentythreeLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 23, text = '23-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 23, text = str.tostring(twentythreeHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 23, text = str.tostring(math.round(twentythreeHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 23, text = str.tostring(math.round(twentythreeHHcounter / twentytwoHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 23, text = str.tostring(twentythreeLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 23, text = str.tostring(math.round(twentythreeLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 23, text = str.tostring(math.round(twentythreeLHcounter / twentytwoLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentyfourHHcounter >= 1 or twentyfourLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 24, text = '24-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 24, text = str.tostring(twentyfourHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 24, text = str.tostring(math.round(twentyfourHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 24, text = str.tostring(math.round(twentyfourHHcounter / twentythreeHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 24, text = str.tostring(twentyfourLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 24, text = str.tostring(math.round(twentyfourLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 24, text = str.tostring(math.round(twentyfourLHcounter / twentythreeLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentyfiveHHcounter >= 1 or twentyfiveLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 25, text = '25-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 25, text = str.tostring(twentyfiveHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 25, text = str.tostring(math.round(twentyfiveHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 25, text = str.tostring(math.round(twentyfiveHHcounter / twentyfourHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 25, text = str.tostring(twentyfiveLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 25, text = str.tostring(math.round(twentyfiveLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 25, text = str.tostring(math.round(twentyfiveLHcounter / twentyfourLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentysixHHcounter >= 1 or twentysixLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 26, text = '26-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 26, text = str.tostring(twentysixHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 26, text = str.tostring(math.round(twentysixHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 26, text = str.tostring(math.round(twentysixHHcounter / twentyfiveHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 26, text = str.tostring(twentysixLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 26, text = str.tostring(math.round(twentysixLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 26, text = str.tostring(math.round(twentysixLHcounter / twentyfiveLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentysevenHHcounter >= 1 or twentysevenLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 27, text = '27-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 27, text = str.tostring(twentysevenHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 27, text = str.tostring(math.round(twentysevenHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 27, text = str.tostring(math.round(twentysevenHHcounter / twentysixHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 27, text = str.tostring(twentysevenLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 27, text = str.tostring(math.round(twentysevenLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 27, text = str.tostring(math.round(twentysevenLHcounter / twentysixLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentyeightHHcounter >= 1 or twentyeightLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 28, text = '28-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 28, text = str.tostring(twentyeightHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 28, text = str.tostring(math.round(twentyeightHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 28, text = str.tostring(math.round(twentyeightHHcounter / twentysevenHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 28, text = str.tostring(twentyeightLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 28, text = str.tostring(math.round(twentyeightLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 28, text = str.tostring(math.round(twentyeightLHcounter / twentysevenLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentynineHHcounter >= 1 or twentynineLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 29, text = '29-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 29, text = str.tostring(twentynineHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 29, text = str.tostring(math.round(twentynineHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 29, text = str.tostring(math.round(twentynineHHcounter / twentyeightHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 29, text = str.tostring(twentynineLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 29, text = str.tostring(math.round(twentynineLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 29, text = str.tostring(math.round(twentynineLHcounter / twentyeightLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (thirtyHHcounter >= 1 or thirtyLHcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 30, text = '30-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 30, text = str.tostring(thirtyHHcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 30, text = str.tostring(math.round(thirtyHHcounter / oneHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 30, text = str.tostring(math.round(thirtyHHcounter / twentynineHHcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 30, text = str.tostring(thirtyLHcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 30, text = str.tostring(math.round(thirtyLHcounter / oneLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 30, text = str.tostring(math.round(thirtyLHcounter / twentynineLHcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

table.cell(upperLowerCandleTrendTable, 1, 31, text = 'Higher Lows', bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 2, 31, text = '% Total', bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 3, 31, text = '% Last', bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 0, 32, text = '1-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 1, 32, text = str.tostring(oneHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 2, 32, text = '-', bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 3, 32, text = '-', bgcolor = color.green, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 4, 31, text = 'Lower Lows', bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 5, 31, text = '% Total', bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 6, 31, text = '% Last', bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 4, 32, text = str.tostring(oneLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 5, 32, text = '-', bgcolor = color.red, text_color = color.white, text_size = textSize)
table.cell(upperLowerCandleTrendTable, 6, 32, text = '-', bgcolor = color.red, text_color = color.white, text_size = textSize)


if (twoHLcounter >= 1 or twoLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 33, text = '2-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 33, text = str.tostring(twoHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 33, text = str.tostring(math.round(twoHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 33, text = '-', bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 33, text = str.tostring(twoLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 33, text = str.tostring(math.round(twoLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 33, text = '-', bgcolor = color.red, text_color = color.white, text_size = textSize)

if (threeHLcounter >= 1 or threeLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 34, text = '3-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 34, text = str.tostring(threeHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 34, text = str.tostring(math.round(threeHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 34, text = str.tostring(math.round(threeHLcounter / twoHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 34, text = str.tostring(threeLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 34, text = str.tostring(math.round(threeLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 34, text = str.tostring(math.round(threeLLcounter / twoLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (fourHLcounter >= 1 or fourLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 35, text = '4-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 35, text = str.tostring(fourHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 35, text = str.tostring(math.round(fourHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 35, text = str.tostring(math.round(fourHLcounter / threeHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 35, text = str.tostring(fourLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 35, text = str.tostring(math.round(fourLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 35, text = str.tostring(math.round(fourLLcounter / threeLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (fiveHLcounter >= 1 or fiveLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 36, text = '5-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 36, text = str.tostring(fiveHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 36, text = str.tostring(math.round(fiveHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 36, text = str.tostring(math.round(fiveHLcounter / fourHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 36, text = str.tostring(fiveLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 36, text = str.tostring(math.round(fiveLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 36, text = str.tostring(math.round(fiveLLcounter / fourLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (sixHLcounter >= 1 or sixLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 37, text = '6-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 37, text = str.tostring(sixHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 37, text = str.tostring(math.round(sixHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 37, text = str.tostring(math.round(sixHLcounter / fiveHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 37, text = str.tostring(sixLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 37, text = str.tostring(math.round(sixLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 37, text = str.tostring(math.round(sixLLcounter / fiveLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (sevenHLcounter >= 1 or sevenLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 38, text = '7-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 38, text = str.tostring(sevenHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 38, text = str.tostring(math.round(sevenHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 38, text = str.tostring(math.round(sevenHLcounter / sixHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 38, text = str.tostring(sevenLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 38, text = str.tostring(math.round(sevenLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 38, text = str.tostring(math.round(sevenLLcounter / sixLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (eightHLcounter >= 1 or eightLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 39, text = '8-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 39, text = str.tostring(eightHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 39, text = str.tostring(math.round(eightHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 39, text = str.tostring(math.round(eightHLcounter / sevenHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 39, text = str.tostring(eightLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 39, text = str.tostring(math.round(eightLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 39, text = str.tostring(math.round(eightLLcounter / sevenLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (nineHLcounter >= 1 or nineLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 40, text = '9-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 40, text = str.tostring(nineHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 40, text = str.tostring(math.round(nineHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 40, text = str.tostring(math.round(nineHLcounter / eightHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 40, text = str.tostring(nineLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 40, text = str.tostring(math.round(nineLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 40, text = str.tostring(math.round(nineLLcounter / eightLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (tenHLcounter >= 1 or tenLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 41, text = '10-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 41, text = str.tostring(tenHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 41, text = str.tostring(math.round(tenHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 41, text = str.tostring(math.round(tenHLcounter / nineHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 41, text = str.tostring(tenLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 41, text = str.tostring(math.round(tenLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 41, text = str.tostring(math.round(tenLLcounter / nineLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (elevenHLcounter >= 1 or elevenLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 42, text = '11-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 42, text = str.tostring(elevenHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 42, text = str.tostring(math.round(elevenHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 42, text = str.tostring(math.round(elevenHLcounter / tenHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 42, text = str.tostring(elevenLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 42, text = str.tostring(math.round(elevenLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 42, text = str.tostring(math.round(elevenLLcounter / tenLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twelveHLcounter >= 1 or twelveLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 43, text = '12-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 43, text = str.tostring(twelveHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 43, text = str.tostring(math.round(twelveHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 43, text = str.tostring(math.round(twelveHLcounter / elevenHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 43, text = str.tostring(twelveLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 43, text = str.tostring(math.round(twelveLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 43, text = str.tostring(math.round(twelveLLcounter / elevenLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (thirteenHLcounter >= 1 or thirteenLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 44, text = '13-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 44, text = str.tostring(thirteenHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 44, text = str.tostring(math.round(thirteenHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 44, text = str.tostring(math.round(thirteenHLcounter / twelveHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 44, text = str.tostring(thirteenLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 44, text = str.tostring(math.round(thirteenLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 44, text = str.tostring(math.round(thirteenLLcounter / twelveLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (fourteenHLcounter >= 1 or fourteenLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 45, text = '14-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 45, text = str.tostring(fourteenHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 45, text = str.tostring(math.round(fourteenHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 45, text = str.tostring(math.round(fourteenHLcounter / thirteenHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 45, text = str.tostring(fourteenLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 45, text = str.tostring(math.round(fourteenLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 45, text = str.tostring(math.round(fourteenLLcounter / thirteenLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (fifteenHLcounter >= 1 or fifteenLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 46, text = '15-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 46, text = str.tostring(fifteenHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 46, text = str.tostring(math.round(fifteenHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 46, text = str.tostring(math.round(fifteenHLcounter / fourteenHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 46, text = str.tostring(fifteenLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 46, text = str.tostring(math.round(fifteenLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 46, text = str.tostring(math.round(fifteenLLcounter / fourteenLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (sixteenHLcounter >= 1 or sixteenLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 47, text = '16-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 47, text = str.tostring(sixteenHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 47, text = str.tostring(math.round(sixteenHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 47, text = str.tostring(math.round(sixteenHLcounter / fifteenHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 47, text = str.tostring(sixteenLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 47, text = str.tostring(math.round(sixteenLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 47, text = str.tostring(math.round(sixteenLLcounter / fifteenLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (seventeenHLcounter >= 1 or seventeenLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 48, text = '17-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 48, text = str.tostring(seventeenHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 48, text = str.tostring(math.round(seventeenHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 48, text = str.tostring(math.round(seventeenHLcounter / sixteenHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 48, text = str.tostring(seventeenLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 48, text = str.tostring(math.round(seventeenLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 48, text = str.tostring(math.round(seventeenLLcounter / sixteenLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (eighteenHLcounter >= 1 or eighteenLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 49, text = '18-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 49, text = str.tostring(eighteenHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 49, text = str.tostring(math.round(eighteenHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 49, text = str.tostring(math.round(eighteenHLcounter / seventeenHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 49, text = str.tostring(eighteenLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 49, text = str.tostring(math.round(eighteenLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 49, text = str.tostring(math.round(eighteenLLcounter / seventeenLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (nineteenHLcounter >= 1 or nineteenLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 50, text = '19-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 50, text = str.tostring(nineteenHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 50, text = str.tostring(math.round(nineteenHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 50, text = str.tostring(math.round(nineteenHLcounter / eighteenHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 50, text = str.tostring(nineteenLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 50, text = str.tostring(math.round(nineteenLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 50, text = str.tostring(math.round(nineteenLLcounter / eighteenLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentyHLcounter >= 1 or twentyLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 51, text = '20-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 51, text = str.tostring(twentyHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 51, text = str.tostring(math.round(twentyHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 51, text = str.tostring(math.round(twentyHLcounter / nineteenHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 51, text = str.tostring(twentyLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 51, text = str.tostring(math.round(twentyLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 51, text = str.tostring(math.round(twentyLLcounter / nineteenLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentyoneHLcounter >= 1 or twentyoneLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 52, text = '21-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 52, text = str.tostring(twentyoneHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 52, text = str.tostring(math.round(twentyoneHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 52, text = str.tostring(math.round(twentyoneHLcounter / twentyHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 52, text = str.tostring(twentyoneLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 52, text = str.tostring(math.round(twentyoneLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 52, text = str.tostring(math.round(twentyoneLLcounter / twentyLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentytwoHLcounter >= 1 or twentytwoLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 53, text = '22-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 53, text = str.tostring(twentytwoHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 53, text = str.tostring(math.round(twentytwoHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 53, text = str.tostring(math.round(twentytwoHLcounter / twentyoneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 53, text = str.tostring(twentytwoLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 53, text = str.tostring(math.round(twentytwoLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 53, text = str.tostring(math.round(twentytwoLLcounter / twentyoneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentythreeHLcounter >= 1 or twentythreeLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 54, text = '23-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 54, text = str.tostring(twentythreeHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 54, text = str.tostring(math.round(twentythreeHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 54, text = str.tostring(math.round(twentythreeHLcounter / twentytwoHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 54, text = str.tostring(twentythreeLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 54, text = str.tostring(math.round(twentythreeLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 54, text = str.tostring(math.round(twentythreeLLcounter / twentytwoLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentyfourHLcounter >= 1 or twentyfourLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 55, text = '24-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 55, text = str.tostring(twentyfourHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 55, text = str.tostring(math.round(twentyfourHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 55, text = str.tostring(math.round(twentyfourHLcounter / twentythreeHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 55, text = str.tostring(twentyfourLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 55, text = str.tostring(math.round(twentyfourLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 55, text = str.tostring(math.round(twentyfourLLcounter / twentythreeLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentyfiveHLcounter >= 1 or twentyfiveLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 56, text = '25-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 56, text = str.tostring(twentyfiveHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 56, text = str.tostring(math.round(twentyfiveHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 56, text = str.tostring(math.round(twentyfiveHLcounter / twentyfourHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 56, text = str.tostring(twentyfiveLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 56, text = str.tostring(math.round(twentyfiveLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 56, text = str.tostring(math.round(twentyfiveLLcounter / twentyfourLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentysixHLcounter >= 1 or twentysixLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 57, text = '26-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 57, text = str.tostring(twentysixHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 57, text = str.tostring(math.round(twentysixHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 57, text = str.tostring(math.round(twentysixHLcounter / twentyfiveHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 57, text = str.tostring(twentysixLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 57, text = str.tostring(math.round(twentysixLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 57, text = str.tostring(math.round(twentysixLLcounter / twentyfiveLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentysevenHLcounter >= 1 or twentysevenLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 58, text = '27-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 58, text = str.tostring(twentysevenHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 58, text = str.tostring(math.round(twentysevenHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 58, text = str.tostring(math.round(twentysevenHLcounter / twentysixHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 58, text = str.tostring(twentysevenLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 58, text = str.tostring(math.round(twentysevenLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 58, text = str.tostring(math.round(twentysevenLLcounter / twentysixLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentyeightHLcounter >= 1 or twentyeightLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 59, text = '28-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 59, text = str.tostring(twentyeightHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 59, text = str.tostring(math.round(twentyeightHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 59, text = str.tostring(math.round(twentyeightHLcounter / twentysevenHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 59, text = str.tostring(twentyeightLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 59, text = str.tostring(math.round(twentyeightLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 59, text = str.tostring(math.round(twentyeightLLcounter / twentysevenLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (twentynineHLcounter >= 1 or twentynineLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 60, text = '29-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 60, text = str.tostring(twentynineHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 60, text = str.tostring(math.round(twentynineHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 60, text = str.tostring(math.round(twentynineHLcounter / twentyeightHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 60, text = str.tostring(twentynineLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 60, text = str.tostring(math.round(twentynineLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 60, text = str.tostring(math.round(twentynineLLcounter / twentyeightLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if (thirtyHLcounter >= 1 or thirtyLLcounter >= 1)
    table.cell(upperLowerCandleTrendTable, 0, 60, text = '30-Candle', bgcolor = color.blue, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 1, 60, text = str.tostring(thirtyHLcounter), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 2, 60, text = str.tostring(math.round(thirtyHLcounter / oneHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 3, 60, text = str.tostring(math.round(thirtyHLcounter / twentynineHLcounter * 100, 2)), bgcolor = color.green, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 4, 60, text = str.tostring(thirtyLLcounter), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 5, 60, text = str.tostring(math.round(thirtyLLcounter / oneLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)
    table.cell(upperLowerCandleTrendTable, 6, 60, text = str.tostring(math.round(thirtyLLcounter / twentynineLLcounter * 100, 2)), bgcolor = color.red, text_color = color.white, text_size = textSize)

if showSamplePeriod 
    table.cell(upperLowerCandleTrendTable, 0, 61, text = startDateText + endDateText, bgcolor = color.black, text_color = color.white, text_size = textSize)


