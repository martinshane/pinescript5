// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © toodegrees
//@version=5
indicator("ICT IPDA Look Back°", 
          shorttitle = "ICT IPDA LB°", 
          overlay = true)


//#region [Functions & Errors]
percenter(top, bottom) =>
    math.round(((close - bottom) / (top - bottom)) * 100, 1)
//#endregion


//#region [Settings]
plotType = input.string("Boxes", 
                       title = "IPDA Plot Format",
                       group = "PLOT SETTINGS",
                       inline = 'tt', 
                       options = ["Boxes", "Table A", "Table B"], 
                       tooltip = "'Boxes' plots proper IPDA ranges;\n" + 
                                 "'Table A' shows Discount / Premium label;\n" + 
                                 "'Table B' shows the distance from Equilibirum in %.")

_showT = input.bool(true,
                     title = "Show Text",
                     group = "PLOT SETTINGS",
                     inline = 'tt')

discountC = input.color(color.new(color.maroon, 80), 
                         title = "Discount",
                         group = "PLOT SETTINGS" ,
                         inline = "pd")

equilibriumC = input.color(color.new(#000000, 0), 
                         title = "Equilibrium",
                         group = "PLOT SETTINGS" ,
                         inline = "pd")

premiumC = input.color(color.new(color.navy, 80), 
                         title = "Premium",
                         group = "PLOT SETTINGS" ,
                         inline = "pd")

ipda20 = input.bool(true, 
                   title = "IPDA20", 
                   group = "IPDA INTERVALS", 
                   inline = "int")

ipda40 = input.bool(true, 
                   title = "IPDA40", 
                   group = "IPDA INTERVALS", 
                   inline = "int")

ipda60 = input.bool(true, 
                   title = "IPDA60", 
                   group = "IPDA INTERVALS", 
                   inline = "int")

_ipdaType = input.string("CLASSIC",
                         title = "IPDA Data Ranges Type",
                         group = "IPDA INTERVALS",
                         options = ["CLASSIC", "CLASSIC+LTF", "LTF"],
                         tooltip = "CLASSIC: standard Daily IPDA Data Ranges visible only on 1D timeframe.\n\nCLASSIC+LTF: plots the Daily 20, 40, 60 IPDA Data Ranges regardless of timeframe.\n\nLTF: plots last 20, 40, 60 IPDA Data Ranges of the current timeframe."
                         )

_showDLTF = _ipdaType == "CLASSIC+LTF"
_showLTF  = _ipdaType == "LTF"
//#endregion


//#region [Logic]
_showDLTF := _showLTF ? true : _showDLTF

time20     = _showLTF ? time[20] : request.security(syminfo.tickerid, "D", time[20])
low20      = _showLTF ? ta.lowest(low, 20)[1] : request.security(syminfo.tickerid, "D", ta.lowest(low, 20))[1]
high20     = _showLTF ? ta.highest(high, 20)[1] : request.security(syminfo.tickerid, "D", ta.highest(high, 20))[1]
eq20       = (low20 + high20) / 2

time40     = _showLTF ? time[40] : request.security(syminfo.tickerid, "D", time[40])
low40      = _showLTF ? ta.lowest(low, 40)[1] : request.security(syminfo.tickerid, "D", ta.lowest(low, 40))[1]
high40     = _showLTF ? ta.highest(high, 40)[1] : request.security(syminfo.tickerid, "D", ta.highest(high, 40))[1]
eq40       = (low40 + high40) / 2

time60     = _showLTF ? time[60] : request.security(syminfo.tickerid, "D", time[60])
low60      = _showLTF ? ta.lowest(low, 60)[1] : request.security(syminfo.tickerid, "D", ta.lowest(low, 60))[1]
high60     = _showLTF ? ta.highest(high, 60)[1] : request.security(syminfo.tickerid, "D", ta.highest(high, 60))[1]
eq60       = (low60 + high60) / 2

loc20      = close < eq20
loc40      = close < eq40
loc60      = close < eq60

percent20  = percenter(high20, low20)
percent40  = percenter(high40, low40)
percent60  = percenter(high60, low60)
//#endregion


//#region [Plot]
var box  ipda20P = na
var line ipda20E = na
var box  ipda20D = na

var box  ipda40P = na
var line ipda40E = na
var box  ipda40D = na

var box  ipda60P = na
var line ipda60E = na
var box  ipda60D = na

if barstate.islast and plotType == "Boxes" and (_showDLTF ? true : timeframe.period == "D")
    if ipda60
        ipda60P := box.new(time60,
                           high60, 
                           time[1], 
                           eq60, 
                           xloc = xloc.bar_time, 
                           border_color = color.new(color.black,100), 
                           bgcolor = premiumC)
        
        ipda60E := line.new(time60,
                           eq60,
                           time[1],
                           eq60,
                           xloc = xloc.bar_time,
                           color = equilibriumC)

        ipda60D := box.new(time60,
                           eq60, 
                           time[1], 
                           low60, 
                           xloc = xloc.bar_time, 
                           border_color = color.new(color.black,100), 
                           bgcolor = discountC,
                           text = "IPDA60",
                           text_size = size.small,
                           text_color = _showT ? #000000 : color.new(#000000,100),
                           text_halign = text.align_left,
                           text_valign = text.align_bottom)

        box.delete(ipda60P[1])
        line.delete(ipda60E[1])
        box.delete(ipda60D[1])


    if ipda40
        ipda40P := box.new(time40,
                           high40, 
                           time[1], 
                           eq40, 
                           xloc = xloc.bar_time, 
                           border_color = color.new(color.black,100), 
                           bgcolor = premiumC)
        
        ipda40E := line.new(time40,
                           eq40,
                           time[1],
                           eq40,
                           xloc = xloc.bar_time,
                           color = equilibriumC)

        ipda40D := box.new(time40,
                           eq40, 
                           time[1], 
                           low40, 
                           xloc = xloc.bar_time, 
                           border_color = color.new(color.black,100), 
                           bgcolor = discountC, 
                           text = "IPDA40",
                           text_size = size.small,
                           text_color = _showT ? #000000 : color.new(#000000,100),
                           text_halign = text.align_left,
                           text_valign = text.align_bottom)

        box.delete(ipda40P[1])
        line.delete(ipda40E[1])
        box.delete(ipda40D[1])


    if ipda20
        ipda20P := box.new(time20,
                           high20, 
                           time[1], 
                           eq20, 
                           xloc = xloc.bar_time, 
                           border_color = color.new(color.black,100), 
                           bgcolor = premiumC)
        
        ipda20E := line.new(time20,
                           eq20,
                           time[1],
                           eq20,
                           xloc = xloc.bar_time,
                           color = equilibriumC)

        ipda20D := box.new(time20,
                           eq20, 
                           time[1], 
                           low20, 
                           xloc = xloc.bar_time, 
                           border_color = color.new(color.black,100), 
                           bgcolor = discountC, 
                           text = "IPDA20",
                           text_size = size.small,
                           text_color = _showT ? #000000 : color.new(#000000,100),
                           text_halign = text.align_left,
                           text_valign = text.align_bottom)

        box.delete(ipda20P[1])
        line.delete(ipda20E[1])
        box.delete(ipda20D[1])


locTable = table.new("top_right", 2, 3, frame_color = #000000, frame_width = 1)

if plotType == "Table A"
    table.cell(locTable, 0, 0, "IPDA60", 0, 0, #000000, "center", text_size = size.normal, bgcolor = #d1d4dc)
    table.cell(locTable, 0, 1, "IPDA40", 0, 0, #000000, "center", text_size = size.normal, bgcolor = #d1d4dc)
    table.cell(locTable, 0, 2, "IPDA20", 0, 0, #000000, "center", text_size = size.normal, bgcolor = #d1d4dc)
    table.cell(locTable, 1, 0, loc60 ? "Discount" : "Premium", text_color = loc60 ? color.maroon : color.navy,text_size = size.normal, bgcolor = #d1d4dc)
    table.cell(locTable, 1, 1, loc40 ? "Discount" : "Premium", text_color = loc40 ? color.maroon : color.navy,text_size = size.normal, bgcolor = #d1d4dc)
    table.cell(locTable, 1, 2, loc20 ? "Discount" : "Premium", text_color = loc20 ? color.maroon : color.navy,text_size = size.normal, bgcolor = #d1d4dc)

if plotType == "Table B"
    table.cell(locTable, 0, 0, "IPDA60", 0, 0, #000000, "center", text_size = size.normal, bgcolor = #d1d4dc)
    table.cell(locTable, 0, 1, "IPDA40", 0, 0, #000000, "center", text_size = size.normal, bgcolor = #d1d4dc)
    table.cell(locTable, 0, 2, "IPDA20", 0, 0, #000000, "center", text_size = size.normal, bgcolor = #d1d4dc)
    table.cell(locTable, 1, 0, str.tostring(percent60) + "%", text_color = loc60 ? color.maroon : color.navy,text_size = size.normal, bgcolor = #d1d4dc)
    table.cell(locTable, 1, 1, str.tostring(percent40) + "%", text_color = loc40 ? color.maroon : color.navy,text_size = size.normal, bgcolor = #d1d4dc)
    table.cell(locTable, 1, 2, str.tostring(percent20) + "%", text_color = loc20 ? color.maroon : color.navy,text_size = size.normal, bgcolor = #d1d4dc)
//#endregion