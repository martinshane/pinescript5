//@version=3
study("Reversal","Fibbonaci Accuracy",overlay=false)


trend_candles = input(5,"#Trend Candles")
fib0 = input(5948.7,"#FibMin")
fib100 = input(19892.1,"FibMax")

fibRange = (fib100-fib0)
fib23 = fib0+fibRange*0.236
fib38 = fib0+fibRange*0.382
fib50 = fib0+fibRange*0.50
fib61 = fib0+fibRange*0.618
fib78 = fib0+fibRange*0.786

faultLine2 = fib0+fibRange*0.02
faultLine4 = fib0+fibRange*0.04
faultLine6 = fib0+fibRange*0.06
faultLine7 = fib0+fibRange*0.07
faultLine10 = fib0+fibRange*0.1


closest_fib(x) => 
    abs(fib0-x) < abs(fib23-x) ? fib0 : abs(fib23-x) < abs(fib38-x) ? fib23 : abs(fib38-x) < abs(fib50-x) ? fib38 : abs(fib50-x) < abs(fib61-x) ? fib50 : abs(fib61-x) < abs(fib78-x) ? fib61 : abs(fib78-x) < abs(fib100-x) ? fib78 : fib100


last_high = pivothigh(high, trend_candles, trend_candles)
last_low = pivotlow(low, trend_candles, trend_candles)

rel_close = last_high ? high[trend_candles] : last_low ? low[trend_candles] : na

points_step = last_high or last_low ? rel_close : na
closest_steps = last_high or last_low ? closest_fib(points_step) : na

pointsp = (points_step-fib0)*100/fibRange
stepsp  = (closest_steps-fib0)*100/fibRange

diffp = pointsp-stepsp

plot(diffp, linewidth=3, style=cross, color= last_low ? red : green, offset=-trend_candles)
plot(0)
//plot(pointsp, linewidth=1, color= last_low ? red : green, offset=-trend_candles)
//plot(stepsp, linewidth=1, color= last_low ? red : green, offset=-trend_candles)



//cdiff = abs(rel_close-closest_fib(rel_close))
//pdiff = (cdiff*100)/(fib100-fib0)

//adiff = last_high or last_low ? pdiff : na

//plotshape(last_high,style=shape.cross,color= red, offset=-trend_candles,size=size.tiny)
//plotshape(last_low,style=shape.arrowup,color= green, offset=-trend_candles,size=size.tiny,location=location.belowbar)


//plot(last_low, style=cross, linewidth=3, color= green, offset=-trend_candles)


//pl = pivotlow(close, 2, 2)

//ph = pivothigh(close, 2, 2)

//plotshape(pl,style=shape.arrowdown,offset=-trend_candles,size=size.tiny,text=tostring(pdiff))
//plotshape(last_high,style=shape.arrowdown,offset=-trend_candles,size=size.tiny,text=tostring(pdiff))
//plotshape(last_low,style=shape.arrowup,offset=-trend_candles, location=location.belowbar,size=size.tiny, text=tostring(pdiff))

//plot(fib0, linewidth=2, color=gray)
//plot(fib23, linewidth=2, color=red)
//plot(fib38, linewidth=2, color=yellow)
//plot(fib50, linewidth=2, color=green)
//plot(fib61, linewidth=2, color=lime)
//plot(fib78, linewidth=2, color=blue)
//plot(fib100, linewidth=2, color=gray)

//disp = adiff/100*fibRange+fib0

//plot(points_step, linewidth=3, style=cross, color= last_low ? green : red)
//plot(closest_step, linewidth=3, style=cross, offset=-trend_candles, color= last_low ? green : red)
//plot(cdiff, linewidth=3, style=cross, offset=-trend_candles, color= last_low ? green : red)
//plot(ema)
//plot(downwards,offset=-trend_candles)

//plotchar(reversal,char='D',offset=trend_candles)

//down = close < open 
//up = close > open 

//reversal = down[1] and not (up[2] and up)
//reversal2 = reversal and not reversal[1]

//plotchar(up,char='U')
    