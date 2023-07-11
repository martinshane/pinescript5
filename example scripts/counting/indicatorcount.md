//@version=4
study("Indicator Count", overlay = true)

float indicatorcount = 0

//TD sequential
TD = 0
TS = 0
TD := close > close[4] ?nz(TD[1])+1:0
TS := close < close[4] ?nz(TS[1])+1:0

TDUp = TD - valuewhen(TD < TD[1], TD , 1 )
TDDn = TS - valuewhen(TS < TS[1], TS , 1 )

//Bollinger Band
length = input(30, minval=1, title="BB Length")
src = input(close, title="BB Source")
mult = input(2.2, minval=0.001, maxval=50, title=" BB Mult")
basis = sma(src, length)
dev = mult * stdev(src, length)
upperBB = basis + dev
lowerBB = basis - dev

//Stoch
periodK = input(14, title="K", minval=1)
smoothK = input(3, title="Smooth", minval=1)
k = sma(stoch(close, high, low, periodK), smoothK)

//Volume oscillator
short = ema(volume, 5)
long = ema(volume, 10)
osc = 100 * (short - long) / long

//Aroon
AroonUp = 100 * (highestbars(high,15) + 14)/14
AroonDown = 100 * (lowestbars(low,15) + 14)/14

//Indicator count arithmetic
if (TDUp >= 5)
	indicatorcount := indicatorcount - 1
if (TDUp >= 9)
	indicatorcount := indicatorcount - 1
if (TDDn >= 5)
	indicatorcount := indicatorcount + 1
if (TDDn >= 9)
	indicatorcount := indicatorcount + 1
if (ema(close,8) > ema(close,13) and ema(close,13) > ema(close,21) and ema(close,21) > ema(close,55))
    indicatorcount := indicatorcount + 1
if (ema(close,8) < ema(close,13) and ema(close,13) < ema(close,21) and ema(close,21) < ema(close,55))
    indicatorcount := indicatorcount - 1
if (crossover(high, upperBB))
	indicatorcount := indicatorcount - 1
if (crossover(low, lowerBB))
	indicatorcount := indicatorcount + 1
if (rsi(close,14) > 75)
	indicatorcount := indicatorcount - 1
if (rsi(close,14) < 25)
	indicatorcount := indicatorcount + 1
if (rsi(close,14) > 85)
	indicatorcount := indicatorcount - 1
if (rsi(close,14) < 15)
	indicatorcount := indicatorcount + 1
if (k > 80)
	indicatorcount := indicatorcount - 1
if (k < 20)
	indicatorcount := indicatorcount + 1
if (close > sma(close, 30))
	indicatorcount := indicatorcount - 1
if (close < sma(close, 30))
	indicatorcount := indicatorcount + 1
if (osc > 0.4) and indicatorcount > 1
	indicatorcount := indicatorcount + 1
if (osc > 0.4) and indicatorcount < 1
	indicatorcount := indicatorcount - 1
if ((AroonUp > 90) and (AroonDown < 10))
	indicatorcount := indicatorcount + 1
if ((AroonUp < 10) and (AroonDown > 90))
	indicatorcount := indicatorcount - 1

//Plotting
plotchar(indicatorcount==-10, location = location.top, char = '10', color = color.red)
plotchar(indicatorcount==-9, location = location.top, char = '9', color = color.red)
plotchar(indicatorcount==-8, location = location.top, char = '8', color = color.red)
plotchar(indicatorcount==-7, location = location.top, char = '7', color = color.red)
plotchar(indicatorcount==-6, location = location.top, char = '6', color = color.red)
plotchar(indicatorcount==-5, location = location.top, char = '5', color = color.red)
plotchar(indicatorcount==-4, location = location.top, char = '4', color = color.red)
plotchar(indicatorcount==-3, location = location.top, char = '3', color = color.red)
plotchar(indicatorcount==-2, location = location.top, char = '2', color = color.red)
plotchar(indicatorcount==-1, location = location.top, char = '1', color = color.red)
plotchar(indicatorcount==0, location = location.top, char = '0', color = color.black)
plotchar(indicatorcount==1, location = location.top, char = '1', color = color.green)
plotchar(indicatorcount==2, location = location.top, char = '2', color = color.green)
plotchar(indicatorcount==3, location = location.top, char = '3', color = color.green)
plotchar(indicatorcount==4, location = location.top, char = '4', color = color.green)
plotchar(indicatorcount==5, location = location.top, char = '5', color = color.green)
plotchar(indicatorcount==6, location = location.top, char = '6', color = color.green)
plotchar(indicatorcount==7, location = location.top, char = '7', color = color.green)
plotchar(indicatorcount==8, location = location.top, char = '8', color = color.green)
plotchar(indicatorcount==9, location = location.top, char = '9', color = color.green)
plotchar(indicatorcount==10, location = location.top, char = '10', color = color.green)

//POSITIVE = BUY
//NEGATIVE = SELL

