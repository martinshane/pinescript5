// © FriendOfTheTrend
//@version=5

indicator("Indicator Direction Table With Bullish & Bearish Labels", shorttitle="Indicator Direction Table", overlay=true)

//Directions
directions = input.bool(true, title="If you change the Place On The List number, you can reorder the indicators to suit your preferences. Make sure that you change the position number for BOTH indicators for it to show up properly. For example: If you change MACD to position 3, make sure to change whatever indicator is in position 3 to the number that the MACD was before so they switch places(do this even if one of them is turned off). You can also turn off each individual indicator so it only shows the ones you want.")

//Table On/Off
infoDataTableOn = input.bool(true, title="Indicator Directions Table On/Off", group="Info Tables")

//Table Positions
bright = position.bottom_right
bleft = position.bottom_left
bcenter = position.bottom_center
tright = position.top_right
tleft = position.top_left
tcenter = position.top_center
mright = position.middle_right
mleft = position.middle_left
mcenter = position.middle_center
itablePosition = input.string(tright, title="Indicator Table Position", options=[bright, bleft, bcenter, tright, tleft, tcenter, mright, mleft, mcenter], group="Info Tables")

//Lengths
macdOn = input.bool(true, title="Turn MACD Label On/Off", group="MACD Settings")
macdOrder = input.int(1, title="Place On The List", minval=1, maxval=9, group="MACD Settings")
macdFast = input.int(12, title="MACD Fast Length", minval=2, group="MACD Settings")
macdSlow = input.int(26, title="MACD Slow Length", minval=2, group="MACD Settings")
macdSignal = input.int(9, title="MACD Signal Length", minval=2, group="MACD Settings")
stochasticOn = input.bool(true, title="Turn Stochstic RSI Label On/Off", group="Stochastic RSI Settings")
stochasticOrder = input.int(2, title="Place On The List", minval=1, maxval=9, group="Stochastic RSI Settings")
smoothK = input.int(3, "K Value - Stochastic RSI", minval=2, group="Stochastic RSI Settings")
smoothD = input.int(3, "D Value - Stochastic RSI", minval=2, group="Stochastic RSI Settings")
lengthRSI = input.int(14, "Stochastic RSI Length", minval=2, group="Stochastic RSI Settings")
lengthStoch = input.int(14, "Stochastic Length", minval=2, group="Stochastic RSI Settings")
vortexOn = input.bool(true, title="Turn Vortex Label On/Off", group="Vortex Settings")
vortexOrder = input.int(3, title="Place On The List", minval=1, maxval=9, group="Vortex Settings")
vortexLength = input.int(14, title="Vortex Length", minval=2, group="Vortex Settings")
momOn = input.bool(true, title="Turn Momentum Label On/Off", group="Momentum Settings")
momOrder = input.int(4, title="Place On The List", minval=1, maxval=9, group="Momentum Settings")
momLength = input.int(14, title="Momentum Length", minval=2, group="Momentum Settings")
rsiOn = input.bool(true, title="Turn RSI Label On/Off", group="RSI Settings")
rsiOrder = input.int(5, title="Place On The List", minval=1, maxval=9, group="RSI Settings")
rsiLength = input.int(14, title="Relative Strength Index Length", minval=2, group="RSI Settings")
psarOn = input.bool(true, title="Turn PSAR Label On/Off", group="PSAR Settings")
psarOrder = input.int(6, title="Place On The List", minval=1, maxval=9, group="PSAR Settings")
psarStart = input.float(.02, title="PSAR Start", minval=.01, group="PSAR Settings")
psarInc = input.float(.02, title="PSAR Increment", minval=.01, group="PSAR Settings")
psarMax = input.float(.2, title="PSAR Max", minval=.01, group="PSAR Settings")
dmiOn = input.bool(true, title="Turn DMI Label On/Off", group="DMI Settings")
dmiOrder = input.int(7, title="Place On The List", minval=1, maxval=9, group="DMI Settings")
dmiLength = input.int(14, title="DMI Length", minval=2, group="DMI Settings")
dmiSmooth = input.int(14, title="DMI Smoothing", minval=2, group="DMI Settings")
mfiOn = input.bool(true, title="Turn Money Flow Index Label On/Off", group="Money Flow Index Settings")
mfiOrder = input.int(8, title="Place On The List", minval=1, maxval=9, group="Money Flow Index Settings")
mfiLength = input.int(14, title="Money Flow Index Length", minval=2, group="Money Flow Index Settings")
fisherOn = input.bool(true, title="Turn Fisher Label On/Off", group="Fisher Settings")
fisherOrder = input.int(9, title="Place On The List", minval=1, maxval=9, group="Fisher Settings")
fisherLength = input.int(14, title="Fisher Length", minval=2, group="Fisher Settings")

//Fisher Transform
high_ = ta.highest(hl2, fisherLength)
low_ = ta.lowest(hl2, fisherLength)
round_(val) => val > .99 ? .999 : val < -.99 ? -.999 : val
value = 0.0
value := round_(.66 * ((hl2 - low_) / (high_ - low_) - .5) + .67 * nz(value[1]))
fish1 = 0.0
fish1 := .5 * math.log((1 + value) / (1 - value)) + .5 * nz(fish1[1])
fish2 = fish1[1]

//Stochastic RSI
rsi1 = ta.rsi(close, lengthRSI)
k = ta.sma(ta.stoch(rsi1, rsi1, rsi1, lengthStoch), smoothK)
d = ta.sma(k, smoothD)

//Vortex
VMP = math.sum( math.abs( high - low[1]), vortexLength)
VMM = math.sum( math.abs( low - high[1]), vortexLength)
STR = math.sum( ta.atr(1), vortexLength)
VIP = VMP / STR
VIM = VMM / STR

//DMI
[diplus, diminus, adx] = ta.dmi(dmiLength, dmiSmooth)

//PSAR
psar = ta.sar(psarStart, psarInc, psarMax)

//Momentum
mom = ta.mom(close, momLength)

//Money Flow Index
mfi = ta.mfi(close, mfiLength)

//RSI
rsi = ta.rsi(close, rsiLength)

//MACD
[macdLine, signalLine, histLine] = ta.macd(close, macdFast, macdSlow, macdSignal)

//Create MACD indicator label table data
macdIndicatorLabel = 'MACD Neutral'
macdLabel = color.blue
if macdLine > signalLine
    macdLabel := color.lime
    macdIndicatorLabel := "MACD Bullish"
else if macdLine < signalLine
    macdLabel := color.red
    macdIndicatorLabel := "MACD Bearish"
    
//Create Stochastic RSI indicator label table data
stochIndicatorLabel = 'Stochastic Neutral'
stochLabel = color.blue
if k > d
    stochLabel := color.lime
    stochIndicatorLabel := "Stochastic Bullish"
else if k < d
    stochLabel := color.red
    stochIndicatorLabel := "Stochastic Bearish"
    
//Create Vortex indicator label table data
vortexIndicatorLabel = 'Vortex Neutral'
vortexLabel = color.blue
if VIP > VIM
    vortexLabel := color.lime
    vortexIndicatorLabel := "Vortex Bullish"
else if VIP < VIM
    vortexLabel := color.red
    vortexIndicatorLabel := "Vortex Bearish"

//Create MFI indicator label table data
mfiIndicatorLabel = 'MFI Neutral'
mfiLabel = color.blue
if mfi > mfi[1]
    mfiLabel := color.lime
    mfiIndicatorLabel := "MFI Bullish"
else if mfi < mfi[1]
    mfiLabel := color.red
    mfiIndicatorLabel := "MFI Bearish"
    
//Create Fisher indicator label table data
fisherIndicatorLabel = 'Fisher Neutral'
fisherLabel = color.blue
if fish1 > fish2
    fisherLabel := color.lime
    fisherIndicatorLabel := "Fisher Bullish"
else if fish1 < fish2
    fisherLabel := color.red
    fisherIndicatorLabel := "Fisher Bearish"
    
//Create DMI indicator label table data
dmiIndicatorLabel = 'DMI Neutral'
dmiLabel = color.blue
if diplus > diminus
    dmiLabel := color.lime
    dmiIndicatorLabel := "DMI Bullish"
else if diplus < diminus
    dmiLabel := color.red
    dmiIndicatorLabel := "DMI Bearish"
    
//Create Momentum indicator label table data
momIndicatorLabel = 'Momentum Neutral'
momLabel = color.blue
if mom > mom[1]
    momLabel := color.lime
    momIndicatorLabel := "Momentum Bullish"
else if mom < mom[1]
    momLabel := color.red
    momIndicatorLabel := "Momentum Bearish"
    
//Create PSAR indicator label table data
psarIndicatorLabel = 'PSAR Neutral'
psarLabel = color.blue
if close > psar
    psarLabel := color.lime
    psarIndicatorLabel := "PSAR Bullish"
else if close < psar
    psarLabel := color.red
    psarIndicatorLabel := "PSAR Bearish"
    
//Create RSI indicator label table data
rsiIndicatorLabel = 'RSI Neutral'
rsiLabel = color.blue
if rsi > rsi[1]
    rsiLabel := color.lime
    rsiIndicatorLabel := "RSI Bullish"
else if rsi < rsi[1]
    rsiLabel := color.red
    rsiIndicatorLabel := "RSI Bearish"

//Plot Price Difference Table
infoDataTable = table.new(itablePosition, columns=1, rows=10)
if infoDataTableOn and barstate.islast
    table.cell(table_id=infoDataTable, column=0, row=mfiOrder, text=mfiOn ? mfiIndicatorLabel : na, height=0, text_color=color.white, text_halign=text.align_left, text_valign=text.align_top, bgcolor=mfiLabel)
    table.cell(table_id=infoDataTable, column=0, row=fisherOrder, text=fisherOn ? fisherIndicatorLabel : na, height=0, text_color=color.white, text_halign=text.align_left, text_valign=text.align_top, bgcolor=fisherLabel)
    table.cell(table_id=infoDataTable, column=0, row=dmiOrder, text=dmiOn ? dmiIndicatorLabel : na, height=0, text_color=color.white, text_halign=text.align_left, text_valign=text.align_top, bgcolor=dmiLabel)
    table.cell(table_id=infoDataTable, column=0, row=momOrder, text=momOn ? momIndicatorLabel : na, height=0, text_color=color.white, text_halign=text.align_left, text_valign=text.align_top, bgcolor=momLabel)
    table.cell(table_id=infoDataTable, column=0, row=psarOrder, text=psarOn ? psarIndicatorLabel : na, height=0, text_color=color.white, text_halign=text.align_left, text_valign=text.align_top, bgcolor=psarLabel)
    table.cell(table_id=infoDataTable, column=0, row=rsiOrder, text=rsiOn ? rsiIndicatorLabel : na, height=0, text_color=color.white, text_halign=text.align_left, text_valign=text.align_top, bgcolor=rsiLabel)
    table.cell(table_id=infoDataTable, column=0, row=macdOrder, text=macdOn ? macdIndicatorLabel : na, height=0, text_color=color.white, text_halign=text.align_left, text_valign=text.align_top, bgcolor=macdLabel)
    table.cell(table_id=infoDataTable, column=0, row=stochasticOrder, text=stochasticOn ? stochIndicatorLabel : na, height=0, text_color=color.white, text_halign=text.align_left, text_valign=text.align_top, bgcolor=stochLabel)
    table.cell(table_id=infoDataTable, column=0, row=vortexOrder, text=vortexOn ? vortexIndicatorLabel : na, height=0, text_color=color.white, text_halign=text.align_left, text_valign=text.align_top, bgcolor=vortexLabel)
    table.cell(table_id=infoDataTable, column=0, row=0, text="Indicator Directions", height=0, text_color=color.white, text_halign=text.align_left, text_valign=text.align_top, bgcolor=color.purple)

//Alerts
alertcondition(rsiLabel == color.red and mfiLabel == color.red and fisherLabel == color.red and dmiLabel == color.red and momLabel == color.red and psarLabel == color.red and 
 macdLabel == color.red and stochLabel == color.red and vortexLabel == color.red, "All Bearish Indicators Alert", "All Bearish Indicators {{ticker}} {{interval}}")
 
alertcondition(rsiLabel == color.lime and mfiLabel == color.lime and fisherLabel == color.lime and dmiLabel == color.lime and momLabel == color.lime and psarLabel == color.lime and 
 macdLabel == color.lime and stochLabel == color.lime and vortexLabel == color.lime, "All Bullish Indicators Alert", "All Bullish Indicators {{ticker}} {{interval}}")