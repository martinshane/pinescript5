//@version=4
study("TradingViews Technical Analysis", overlay=true)
// Inputs
showStrongBuy = input(true, title="Show Strong Buy Signals")
showBuy = input(false, title="Show Buy Signals")
showStrongSell = input(true, title="Show Strong Sell Signals")
showSell = input(false, title="Show Sell Signals")
showNeutral = input(false, title="Show Neutral Bar")
showBG = input(true, title="Show Background")
useEntryExit = input(true, title="Use as Entry/Exit Points")
checklevel = input(15, "Check Level", input.integer, minval=5, maxval=23)
src = input(close, title="Source")

// Indicators
//sma
sma5 = sma(src, 5)
sma10 = sma(src, 10)
sma20 = sma(src, 20)
sma30 = sma(src, 30)
sma50 = sma(src, 50)
sma100 = sma(src, 100)
sma200 = sma(src, 200)
smapoint = 0
smapoint := src > sma5 ? smapoint + 1 : smapoint - 1
smapoint := src > sma10 ? smapoint + 1 : smapoint - 1
smapoint := src > sma20 ? smapoint + 1 : smapoint - 1
smapoint := src > sma30 ? smapoint + 1 : smapoint - 1
smapoint := src > sma50 ? smapoint + 1 : smapoint - 1
smapoint := src > sma100 ? smapoint + 1 : smapoint - 1
smapoint := src > sma200 ? smapoint + 1 : smapoint - 1
//ema
ema5 = ema(src, 5)
ema10 = ema(src, 10)
ema20 = ema(src, 20)
ema30 = ema(src, 30)
ema50 = ema(src, 50)
ema100 = ema(src, 100)
ema200 = ema(src, 200)
emapoint = 0
emapoint := src > ema5 ? emapoint + 1 : emapoint - 1
emapoint := src > ema10 ? emapoint + 1 : emapoint - 1
emapoint := src > ema20 ? emapoint + 1 : emapoint - 1
emapoint := src > ema30 ? emapoint + 1 : emapoint - 1
emapoint := src > ema50 ? emapoint + 1 : emapoint - 1
emapoint := src > ema100 ? emapoint + 1 : emapoint - 1
emapoint := src > ema200 ? emapoint + 1 : emapoint - 1
//rsi
up = rma(max(change(src), 0), 14)
down = rma(-min(change(src), 0), 14)
rsi = down == 0 ? 100 : up == 0 ? 0 : 100 - (100 / (1 + up / down))
band1 = 70
band0 = 30
rsipoint = rsi > band1 ? -1 : rsi < band0 ? 1 : 0
//stochastic
periodK = 14
periodD = 3
smoothK = 3
k = sma(stoch(close, high, low, periodK), smoothK)
d = sma(k, periodD)
h0 = 80
h1 = 20
stochpoint = k > h0 ? -1 : k < h1 ? 1 : 0
//CCI
ma = sma(src, 20)
cci = (src - ma) / (0.015 * dev(src, 20))
cciband1 = 100
cciband0 = -100
ccipoint = cci > cciband1 ? -1 : cci < cciband0 ? 1 : 0
//ADX
adxlen = 14
dilen = 14
dirmov(len) =>
	up = change(high)
	down = -change(low)
	plusDM = na(up) ? na : (up > down and up > 0 ? up : 0)
    minusDM = na(down) ? na : (down > up and down > 0 ? down : 0)
	truerange = rma(tr, len)
	plus = fixnan(100 * rma(plusDM, len) / truerange)
	minus = fixnan(100 * rma(minusDM, len) / truerange)
	[plus, minus]

adx(dilen, adxlen) =>
	[plus, minus] = dirmov(dilen)
	sum = plus + minus
	adx = 100 * rma(abs(plus - minus) / (sum == 0 ? 1 : sum), adxlen)

sig = adx(dilen, adxlen)
[adxplus, adxminus] = dirmov(dilen)
adxline = 20
adxpoint = sig > adxline and crossover(adxplus, adxminus) ? 1 : sig > adxline and crossunder(adxplus, adxminus) ? -1 : 0
//AO
ao = sma(hl2,5) - sma(hl2,34)
aopoint = ao > 0 and ao > ao[1] ? 1 : ao < 0 and ao < ao[1] ? -1 : 0
//momentum
mom = src - src[10]
mompoint = mom > mom[1] ? 1 : mom < mom[1] ? -1 : 0
//MACD
fast_ma = ema(src, 12)
slow_ma = ema(src, 26)
macd = fast_ma - slow_ma
signal = ema(macd, 9)
hist = macd - signal
histpoint = hist > hist[1] ? 1 : -1
//Stoch RSI
rsi1 = rsi(src, 14)
rsik = sma(stoch(rsi1, rsi1, rsi1, 14), 3)
rsid = sma(rsik, 3)
rsih0 = 80
rsih1 = 20
stochrsipoint = rsik > rsih0 ? -1 : rsik < rsih1 ? 1 : 0
//%R
upper = highest(14)
lower = lowest(14)
out = 100 * (src - upper) / (upper - lower)
rband1 = -20
rband0 = -80
rpoint = out > rband1 ? -1 : out < rband0 ? 1 : 0
//Bull bear
Length = 30
r1=iff(close[1]<open,max(open-close[1],high-low),high-low)
r2=iff(close[1]>open,max(close[1]-open,high-low),high-low)
bull=iff(close==open,iff(high-close==close-low,iff(close[1]>open,max(high-open,close-low),r1),iff(high-close>close-low,iff(close[1]<open, max(high-close[1],close-low), high-open),r1)),iff(close<open,iff(close[1]<open,max(high-close[1],close-low), max(high-open,close-low)),r1))
bear=iff(close==open,iff(high-close==close-low,iff(close[1]<open,max(open-low,high-close),r2),iff(high-close>close-low,r2,iff(close[1]>open,max(close[1]-low,high-close), open-low))),iff(close<open,r2,iff(close[1]>open,max(close[1]-low,high-close),max(open-low,high-close))))
colors=iff(sma(bull-bear,Length)>0, color.green, color.red)
// barcolor(colors)
bbpoint = sma(bull-bear,Length)>0 ? 1 : -1
//UO
length7 = 7,
length14 = 14,
length28 = 28
average(bp, tr_, length) => sum(bp, length) / sum(tr_, length)
high_ = max(high, src[1])
low_ = min(low, src[1])
bp = src - low_
tr_ = high_ - low_
avg7 = average(bp, tr_, length7)
avg14 = average(bp, tr_, length14)
avg28 = average(bp, tr_, length28)
uoout = 100 * (4*avg7 + 2*avg14 + avg28)/7
uopoint = uoout > 70 ? 1 : uoout < 30 ? -1 : 0
//IC
conversionPeriods = 9
basePeriods = 26
laggingSpan2Periods = 52
displacement = 26
donchian(len) => avg(lowest(len), highest(len))
baseLine = donchian(basePeriods)
icpoint = src > baseLine ? 1 : -1
//VWMA
vwma = vwma(src, 20)
vwmapoint = src > vwma ? 1 : -1
//HMA
hullma = wma(2*wma(src, 9/2)-wma(src, 9), round(sqrt(9)))
hmapoint = src > hullma ? 1 : -1


// Calculations
totalpoints = smapoint + emapoint + rsipoint + stochpoint + ccipoint + adxpoint + aopoint + mompoint + histpoint + stochrsipoint + rpoint + bbpoint + uopoint + icpoint + vwmapoint + hmapoint
    
// PLots
plotshape(showStrongBuy and totalpoints >= checklevel and not useEntryExit, text='Strong Buy', style=shape.labelup, location=location.belowbar, color=color.blue, textcolor=color.white, transp=0, offset=0)
plotshape(showBuy and totalpoints < checklevel and totalpoints > 0 and not useEntryExit, text='Buy', style=shape.labelup, location=location.belowbar, color=color.blue, textcolor=color.white, transp=0, offset=0)
plotshape(showStrongSell and totalpoints <= -checklevel and not useEntryExit, text='Strong Sell', style=shape.labeldown, location=location.abovebar, color=color.red, textcolor=color.white, transp=0, offset=0)
plotshape(showSell and totalpoints > -checklevel and totalpoints < 0 and not useEntryExit, text='Sell', style=shape.labeldown, location=location.abovebar, color=color.red, textcolor=color.white, transp=0, offset=0)

plotshape(showStrongBuy and totalpoints >= checklevel and totalpoints[1] <= 0 and useEntryExit, text='Strong Buy', style=shape.labelup, location=location.belowbar, color=color.blue, textcolor=color.white, transp=0, offset=0)
plotshape(showBuy and totalpoints < checklevel and totalpoints > 0 and totalpoints[1] <= 0 and useEntryExit, text='Buy', style=shape.labelup, location=location.belowbar, color=color.blue, textcolor=color.white, transp=0, offset=0)
plotshape(showStrongSell and totalpoints <= -checklevel and totalpoints[1] >= 0 and useEntryExit, text='Strong Sell', style=shape.labeldown, location=location.abovebar, color=color.red, textcolor=color.white, transp=0, offset=0)
plotshape(showSell and totalpoints > -checklevel and totalpoints < 0 and totalpoints[1] >= 0 and useEntryExit, text='Sell', style=shape.labeldown, location=location.abovebar, color=color.red, textcolor=color.white, transp=0, offset=0)

barcolor(totalpoints == 0 and showNeutral ? color.white : na, offset=0)
bgcolor(totalpoints > 0 and showBG ? color.blue : totalpoints < 0 and showBG ? color.red : showBG ? color.white : na, transp=90)

// Alerts
alertcondition(totalpoints >= checklevel, title="Tradingview Technical Analysis", message="Strong Buy")
alertcondition(totalpoints <= -checklevel, title="Tradingview Technical Analysis", message="Strong Sell")

