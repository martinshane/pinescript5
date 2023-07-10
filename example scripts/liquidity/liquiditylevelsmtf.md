// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/

// © Sonarlab
// ps. we love you

//@version=5
indicator("Liquidity Levels - Sonarlab", overlay=true)
_tfSettings = "TimeFrame Settings"
currentTF  = input.bool(true, title = "Liquidity Levels", group=_tfSettings)
htfBool  = input.bool(false, title = "Higher Timeframe",inline="1", group=_tfSettings)
htfTF  = input.timeframe("", title = "", inline="1",group=_tfSettings, tooltip="Display Liquidity Levels for a Higher Timeframe")
// --
_lvlsGrp = "Liquidity Levels"
leftBars  = input.int(15, title = "Left Bars", group=_lvlsGrp, tooltip="Set the lookback point for what determines a Liquidity Level")
rightBars  = input.int(5, title = "Right Bars", group=_lvlsGrp, tooltip="Set the number of bars to confirm a Liquidity Level")
// --
_removeGrp = "Mitigation Settings"
removeMitigated  = input.bool(true, title = "Mitigated", inline="1", group=_removeGrp)
mitiOptions  = input.string("Show", title = "    ",  inline="1", options=["Remove", "Show"], group=_removeGrp, tooltip="Show: Liquidity Levels will stop printing when mitigated and remain on the chart.\nRemove: Liquidity Levels will be removed from the chart when mitigated.")
_candleType  = input.string("Close", title = "Candle type", options=["Close", "Wick"], group=_removeGrp, tooltip="Choose whether a candle close or a candles high/low is needed to determine a mitigated Liquidity Level")
// --
_displayStyleGrp = "Display Styles"
displayStyle  = input.string("Lines", title = "Display Style", options=["Lines", "Boxes"], group=_displayStyleGrp, tooltip="Choose how Liquidity Levels are displayed on the chart")
extentionOptions  = input.string("Current", title = "Extention Options", options=["Short", "Current", "Max"], group=_displayStyleGrp, tooltip="Choose how Liquidity Levels are extended on the chart")
extentionMax = extentionOptions=="Max" ? true : false
extentionCurrent = extentionOptions=="Current" ? true : false
displayLimit  = input.int(5, title = "Display Limit", group=_displayStyleGrp, tooltip="")
// --
_styleGrp = "Line Styles and Colors"
_highLineStyle  = input.string("Solid", title = "High Line Style", options=["Solid", "Dashed", "Dotted"], group=_styleGrp)
highLineStyle = _highLineStyle=="Solid" ? line.style_solid : _highLineStyle=="Dashed" ? line.style_dashed : line.style_dotted
_lowLineStyle  = input.string("Solid", title = "Low Line Style", options=["Solid", "Dashed", "Dotted"], group=_styleGrp)
lowLineStyle = _lowLineStyle=="Solid" ? line.style_solid : _lowLineStyle=="Dashed" ? line.style_dashed : line.style_dotted
lineWidth  = input.int(1, title = "Line Width", group=_styleGrp, tooltip="")

// --
highLineColor = input.color(#1f4ef5, "High Line   ", group = _styleGrp, inline = "1")
lowLineColor = input.color(#fd441c, "Low Line", group = _styleGrp, inline = "1")
highBoxBgColor = input.color(color.new(#1f4ef5, 80), "High Box Bg ", group = _styleGrp, inline = "2")
highBoxBorderColor = input.color(color.new(#1f4ef5, 80), "Box Border", group = _styleGrp, inline = "2")
lowBoxBgColor = input.color(color.new(#fd441c, 80), "Low Box Bg  ", group = _styleGrp, inline = "3")
lowBoxBorderColor = input.color(color.new(#fd441c, 80), "Box Border", group = _styleGrp, inline = "3")
// --
// --
_styleGrpHTF = "Line Styles and Colors - Higher TimeFrame"
_highLineStyleHTF  = input.string("Solid", title = "High Line HTF", options=["Solid", "Dashed", "Dotted"], group=_styleGrpHTF)
highLineStyleHTF = _highLineStyleHTF=="Solid" ? line.style_solid : _highLineStyleHTF=="Dashed" ? line.style_dashed : line.style_dotted
_lowLineStyleHTF  = input.string("Solid", title = "Low Line HTF", options=["Solid", "Dashed", "Dotted"], group=_styleGrpHTF)
lowLineStyleHTF = _lowLineStyleHTF=="Solid" ? line.style_solid : _lowLineStyleHTF=="Dashed" ? line.style_dashed : line.style_dotted
lineWidthHTF  = input.int(1, title = "Line Width HTF", group=_styleGrpHTF, tooltip="")

// --
highLineColorHTF = input.color(#4c9650, "High Line   ", group = _styleGrpHTF, inline = "1")
lowLineColorHTF = input.color(#fd1c49, "Low Line", group = _styleGrpHTF, inline = "1")
highBoxBgColorHTF = input.color(color.new(#4c9650, 80), "High Box Bg ", group = _styleGrpHTF, inline = "2")
highBoxBorderColorHTF = input.color(color.new(#4c9650, 80), "Box Border", group = _styleGrpHTF, inline = "2")
lowBoxBgColorHTF = input.color(color.new(#fd1c49, 80), "Low Box Bg  ", group = _styleGrpHTF, inline = "3")
lowBoxBorderColorHTF = input.color(color.new(#fd1c49, 80), "Box Border", group = _styleGrpHTF, inline = "3")
// --
// ----------------------------------------------------
// Functions 
// ----------------------------------------------------
tf_multi(tf) =>
    ts = timeframe.in_seconds("")
    htfs = timeframe.in_seconds(tf)
    htfs/ts

display_limit_line(_array) =>
    if array.size(_array) > displayLimit/2
        a = array.shift(_array)
        line.delete(a)

display_limit_box(_array) =>
    if array.size(_array) > displayLimit/2
        a = array.shift(_array)
        box.delete(a)

remove_mitigated_lines(_array, _hl) =>
    m = false
    if array.size(_array) > 0 and removeMitigated        
        for i = array.size(_array) - 1 to 0 by 1
            l = array.get(_array, i)
            hh = _candleType == "Close" ? close[1] : high
            ll = _candleType == "Close" ? close[1] : low
            if _hl == "High" and hh > line.get_y1(l)
                array.remove(_array, i)
                if mitiOptions == "Show"
                    line.new(line.get_x1(l),line.get_y1(l),time,line.get_y1(l), xloc=xloc.bar_time, color = color.new(highLineColor, 70))
                line.delete(l)
                m := true
            if _hl == "Low" and ll < line.get_y1(l)
                array.remove(_array, i)
                if mitiOptions == "Show"
                    line.new(line.get_x1(l),line.get_y1(l),time,line.get_y1(l), xloc=xloc.bar_time, color = color.new(lowLineColor, 70))
                line.delete(l) 
                m := true  
    display_limit_line(_array) 
    m

remove_mitigated_boxes(_array, _hl) =>
    m = false
    if array.size(_array) > 0 and removeMitigated
        for i = array.size(_array) - 1 to 0 by 1
            l = array.get(_array, i)
            hh = _candleType == "Close" ? close[1] : high
            ll = _candleType == "Close" ? close[1] : low
            if _hl == "High" and hh > box.get_top(l)
                array.remove(_array, i)
                if mitiOptions == "Show"
                    box.new(box.get_left(l),box.get_top(l),time,box.get_bottom(l), xloc=xloc.bar_time, bgcolor = color.new(highBoxBgColor, 90), border_color = color.new(highBoxBorderColor, 90), border_style = highLineStyle)
                box.delete(l)
                m := true
            if _hl == "Low" and ll < box.get_top(l)
                array.remove(_array, i)
                if mitiOptions == "Show"
                    box.new(box.get_left(l),box.get_top(l),time,box.get_bottom(l), xloc=xloc.bar_time, bgcolor = color.new(lowBoxBgColor, 90), border_color = color.new(lowBoxBorderColor, 90), border_style = lowLineStyle)
                box.delete(l)
                m := true
    display_limit_box(_array) 
    m

extend_line_to_current(lineArray) =>
    if array.size(lineArray) > 0
        for i = array.size(lineArray) - 1 to 0 by 1
            l = array.get(lineArray, i)
            timeExt = time + ((time[1]-time[2])*20)
            line.set_x2(l, timeExt)

extend_box_to_current(boxArray) =>
    if array.size(boxArray) > 0
        for i = array.size(boxArray) - 1 to 0 by 1
            b = array.get(boxArray, i)
            timeExt = time + ((time[1]-time[2])*20)
            box.set_right(b, timeExt)

// ----------------------------------------------------
// Current TimeFrame
// ----------------------------------------------------
// Varibles 
// Lines
var highLineArray = array.new_line()
var lowLineArray = array.new_line()
// Boxes
var highBoxArray = array.new_box()
var lowBoxArray = array.new_box()

// Pivots
pivotHigh = ta.pivothigh(leftBars, rightBars)[1]
pivotLow = ta.pivotlow(leftBars, rightBars)[1]

// Run Calculations
if currentTF
    if pivotHigh
        if displayStyle == "Lines"
            array.push(highLineArray, line.new(time[rightBars+1],high[rightBars+1],time[+1],high[rightBars+1],color = highLineColor, style=highLineStyle, xloc=xloc.bar_time, extend=extentionMax?extend.right:extend.none, width = lineWidth))
        else
            y1 = math.max(open[rightBars+1], close[rightBars+1])
            array.push(highBoxArray, box.new(time[rightBars+1],high[rightBars+1],time[+1],y1,bgcolor = highBoxBgColor, border_color=highBoxBorderColor, xloc=xloc.bar_time, border_style = highLineStyle, extend=extentionMax?extend.right:extend.none, border_width = lineWidth))        
    if pivotLow
        if displayStyle == "Lines"
            array.push(lowLineArray, line.new(time[rightBars+1],low[rightBars+1],time[+1],low[rightBars+1],color = lowLineColor, style=lowLineStyle, xloc=xloc.bar_time, extend=extentionMax?extend.right:extend.none, width = lineWidth))
        else
            y1 = math.min(open[rightBars+1], close[rightBars+1])
            array.push(lowBoxArray, box.new(time[rightBars+1],low[rightBars+1],time[+1],y1,bgcolor = lowBoxBgColor, border_color=lowBoxBorderColor, xloc=xloc.bar_time, border_style = lowLineStyle, extend=extentionMax?extend.right:extend.none, border_width = lineWidth))

// ----------------------------------------------------
// Run Functions
// ----------------------------------------------------
highLineAlert = remove_mitigated_lines(highLineArray, "High")
lowLineAlert = remove_mitigated_lines(lowLineArray, "Low")
highBoxAlert = remove_mitigated_boxes(highBoxArray, "High")
lowBoxAlert = remove_mitigated_boxes(lowBoxArray, "Low")

if extentionCurrent
    extend_line_to_current(highLineArray)
    extend_line_to_current(lowLineArray)
    extend_box_to_current(highBoxArray)
    extend_box_to_current(lowBoxArray)

// Alerts
alertcondition(highLineAlert or highBoxAlert, "New High", "Price is breaking out!")
alertcondition(lowLineAlert or lowBoxAlert, "New Low", "Price is breaking down!")

// ----------------------------------------------------
// Higher TimeFrame
// ----------------------------------------------------
// Varibles 
// Lines
var highLineArrayHTF = array.new_line()
var lowLineArrayHTF = array.new_line()
// Boxes
var highBoxArrayHTF = array.new_box()
var lowBoxArrayHTF = array.new_box()

// Get HTF
[_time, _open, _high, _low, _close] = request.security(syminfo.tickerid, htfTF, [time, open, high, low, close])

// Pivots
pivotHighHTF = ta.pivothigh(_high, leftBars*tf_multi(htfTF), rightBars+tf_multi(htfTF))
pivotLowHTF = ta.pivotlow(_low, leftBars*tf_multi(htfTF), rightBars+tf_multi(htfTF))

if htfBool
    timeExt = time+((time[1]-time[2])*10)
    dis = rightBars+tf_multi(htfTF)
    if pivotHighHTF
        if displayStyle == "Lines"
            array.push(highLineArrayHTF, line.new(_time[dis],_high[dis],_time[+1],_high[dis],color = highLineColorHTF, style=highLineStyleHTF, xloc=xloc.bar_time, extend=extentionMax?extend.right:extend.none, width = lineWidthHTF))
        else
            y1 = math.max(_open[dis], _close[dis])
            array.push(highBoxArrayHTF, box.new(_time[dis],_high[dis],_time[+1],y1,bgcolor = highBoxBgColorHTF, border_color=highBoxBorderColorHTF, xloc=xloc.bar_time, border_style = highLineStyleHTF, extend=extentionMax?extend.right:extend.none, border_width = lineWidthHTF))  
    if pivotLowHTF
        if displayStyle == "Lines"
            array.push(lowLineArrayHTF, line.new(_time[dis],_low[dis],_time[+1],_low[dis],color = lowLineColorHTF, style=lowLineStyleHTF, xloc=xloc.bar_time, extend=extentionMax?extend.right:extend.none, width = lineWidthHTF))
        else
            y1 = math.min(_open[dis], _close[dis])
            array.push(lowBoxArrayHTF, box.new(_time[dis],_low[dis],_time[+1],y1,bgcolor = lowBoxBgColorHTF, border_color=lowBoxBorderColorHTF, xloc=xloc.bar_time, border_style = lowLineStyleHTF, extend=extentionMax?extend.right:extend.none, border_width = lineWidthHTF))

// ----------------------------------------------------
// Run Functions
// ----------------------------------------------------
highLineAlertHTF = remove_mitigated_lines(highLineArrayHTF, "High")
lowLineAlertHTF = remove_mitigated_lines(lowLineArrayHTF, "Low")
highBoxAlertHTF = remove_mitigated_boxes(highBoxArrayHTF, "High")
lowBoxAlertHTF = remove_mitigated_boxes(lowBoxArrayHTF, "Low")

if extentionCurrent
    extend_line_to_current(highLineArrayHTF)
    extend_line_to_current(lowLineArrayHTF)
    extend_box_to_current(highBoxArrayHTF)
    extend_box_to_current(lowBoxArrayHTF)

// Alerts
alertcondition(highLineAlertHTF or highBoxAlertHTF, "New HTF High", "Price is breaking out!")
alertcondition(lowLineAlertHTF or lowBoxAlertHTF, "New HTF Low", "Price is breaking down!")
