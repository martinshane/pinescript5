// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// Logic by @TradesByMatt
// Tool by @TheBitBrine

//@version=5
indicator("Support & Resistance Hit Counter", overlay=true)
var price = input.float(0, "Price line", step = 0.1)
enableTrend = input.bool(false, "Trend")

tradingMode = input.bool(false, "Enabled", group="Trade Simulator")
lev = input.float(10, "Leverage", group="Trade Simulator")
pos = input.float(10000, "Cointracts", group="Trade Simulator")

pos :=  pos / close
var crossed = 0

green = false
if(close > open)
    green := true


highestBody = math.max(close, open)
lowestBody = math.min(close, open)

var highValue = 0.0
var lowValue = 9999999.0
var highValue2 = 0.0
var lowValue2 = 9999999.0
var lastHitPrice = 0.0

if(price < high and price > highestBody)
    crossed := crossed+1
    if(low < lowValue2)
        lowValue2 := low
    if(high > highValue)
        highValue := high
    lastHitPrice := close
        
if(price > low and price < lowestBody)
    crossed := crossed+1
    if(low < lowValue)
        lowValue := low
    if(high > highValue2)
        highValue2 := high
    lastHitPrice := close

plotshape(crossed != crossed[1] ? price : na, style = shape.cross, size=size.small, location=location.absolute, color=color.orange)
plot(lowValue < 9999999.0 ? price : na, color=color.orange, transp=40)
hvp = plot(highValue > 0 ? highValue : na, color=color.green, transp=0, style= plot.style_circles)
lvp = plot(lowValue < 9999999.0 ? lowValue : na, color=color.red, transp=0, style= plot.style_circles)

hvp2 = plot(highValue2 > 0 ? highValue2 : na, color=color.green, transp=0, style= plot.style_circles)
lvp2 = plot(lowValue2 < 9999999.0 ? lowValue2 : na, color=color.red, transp=0, style= plot.style_circles)
fill(hvp, lvp, color=color.blue, transp=95)
fill(hvp2, lvp2, color=color.blue, transp=95)

fallingside = price > close
risingside = price < close

side = "NA"
if(fallingside)
    side := "Short"
    
if(risingside)
    side := "Long"

fallingtrend = ta.falling(ta.ema(close, 9), 3)
risingtrend = ta.rising(ta.ema(close, 9), 3)

trend = "\n[ NA ]"
if(fallingtrend)
    trend := "\n[ Bearish ]"
    
if(risingtrend)
    trend := "\n[ Bullish ]"


if(enableTrend == false)
    trend := ""

currLvl1 = fallingside ? lowValue : highValue
currLvl2 = fallingside ? lowValue2 : highValue2

tradeString = tradingMode ? "\n\nSide: " + side  + "\nProfit: $" + str.tostring(math.round(math.abs(pos * ((close - lastHitPrice) * lev)), 2)) : ""
tradeString := tradingMode ? tradeString + "\nTP 1: $" + str.tostring(math.round(math.abs(pos * ((currLvl1 - lastHitPrice) * lev)), 2)) : ""
tradeString := tradingMode ? tradeString + "\nTP 2: $" + str.tostring(math.round(math.abs(pos * ((currLvl2 - lastHitPrice) * lev)), 2)) : ""

if(lowValue == 9999999.0 and lowValue2 == 9999999.0 and highValue == 0.0 and highValue2 == 0.0)
    lab_l = label.new(
              bar_index + 10, close, '[ S/R Hit Counter ]\n Please enter entry price',
              color     = color.black, 
              textcolor = color.orange, 
              style     = label.style_diamond,
              yloc      = yloc.price)
    label.delete(lab_l[1])
else
    lab_l = label.new(
              bar_index + 10, price, '[ ' + str.tostring(price) + ' ]' + trend + '\n\nHits: ' + str.tostring(crossed) + 'x\nHigh: ' + str.tostring(highValue) + '\nLow: ' + str.tostring(lowValue) + tradeString + '\n\nOffset: $' + str.tostring(close - price), 
              color     = color.black, 
              textcolor = color.orange, 
              style     = label.style_diamond,
              yloc      = yloc.price)
    label.delete(lab_l[1])




