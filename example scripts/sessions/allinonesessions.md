// // © Texmoonbeam
//@version=5
indicator("All-In-One Sessions, Weekly, Monday, Previous Highs/Lows", overlay = true, max_boxes_count=500, max_lines_count = 500, max_labels_count = 500, max_bars_back=5000)
//timeframe = input.timeframe(defval = '240')

abr = input.bool(defval = true, title="Abbreviate Labels", group="Mondays, Weekly, Monthly", inline="0a")
labelsz = input.string(defval="small", title="Size", options=["tiny","small","normal","large","huge"], group="Mondays, Weekly, Monthly", inline="0a")
sz = labelsz == "tiny" ? size.tiny : labelsz == "small" ? size.small : labelsz == "normal" ? size.normal : labelsz == "large" ? size.large : labelsz == "huge" ? size.huge : size.auto

ShowMonday = input.bool(defval = true, title="Show Monday High/Low", group="Mondays, Weekly, Monthly", inline="1a")
ExtendMonday = input.bool(defval = false, title="Extend?", group="Mondays, Weekly, Monthly", inline="1a")
LabelMonday = input.bool(defval = true, title="Label?", group="Mondays, Weekly, Monthly", inline="1a")
MidMonday = input.bool(defval = false, title="50%?", group="Mondays, Weekly, Monthly", inline="1a")
MonHighCol = input.color(color.green, "High", group="Mondays, Weekly, Monthly", inline="1c")
MonLowCol = input.color(color.red, "Low", group = "Mondays, Weekly, Monthly", inline="1c")
MonMidCol = input.color(color.green, "0.5", group = "Mondays, Weekly, Monthly", inline="1c")
MonWidth = input.int(defval=1, title="Width", minval=1, maxval = 4, group="Mondays, Weekly, Monthly", inline="1c")
MonOff = input.int(defval = 0, title="Label & Line Offset", group="Mondays, Weekly, Monthly", inline="1d", tooltip = "Offsets are measured in bars of the current chart timeframe")

ShowPrevDay = input.bool(defval = true, title="Show Prev Day High/Low", group="Mondays, Weekly, Monthly", inline="2a")
ExtendPrevDay = input.bool(defval = false, title="Extend?", group="Mondays, Weekly, Monthly", inline="2a")
LabelPrevDay = input.bool(defval = true, title="Label?", group="Mondays, Weekly, Monthly", inline="2a")
MidPrevDay = input.bool(defval = false, title="50%?", group="Mondays, Weekly, Monthly", inline="2a")
PrevDayHighCol = input.color(color.green, "High", group="Mondays, Weekly, Monthly", inline="2c")
PrevDayLowCol = input.color(color.red, "Low", group = "Mondays, Weekly, Monthly", inline="2c")
PrevDayMidCol = input.color(color.green, "0.5", group = "Mondays, Weekly, Monthly", inline="2c")
PrevDayWidth = input.int(defval=1, title="Width", minval=1, maxval = 4, group="Mondays, Weekly, Monthly", inline="2c")
PrevDayOff = input.int(defval = 0, title="Label & Line Offset", group="Mondays, Weekly, Monthly", inline="2d")

ShowCurWeek = input.bool(defval = false, title="Show Current Week High/Low", group="Mondays, Weekly, Monthly", inline="3a")
ExtendCurWeek = input.bool(defval = false, title="Extend?", group="Mondays, Weekly, Monthly", inline="3a")
LabelCurWeek = input.bool(defval = false, title="Label?", group="Mondays, Weekly, Monthly", inline="3a")
MidCurWeek = input.bool(defval = false, title="50%?", group="Mondays, Weekly, Monthly", inline="3a")
CurWeekHighCol = input.color(color.green, "High", group="Mondays, Weekly, Monthly", inline="3c")
CurWeekLowCol = input.color(color.red, "Low", group = "Mondays, Weekly, Monthly", inline="3c")
CurWeekMidCol = input.color(color.green, "0.5", group = "Mondays, Weekly, Monthly", inline="3c")
CurWeekWidth = input.int(defval=1, title="Width", minval=1, maxval = 4, group="Mondays, Weekly, Monthly", inline="3c")
CurWeekOff = input.int(defval = 0, title="Label & Line Offset", group="Mondays, Weekly, Monthly", inline="3d")

ShowPrevWeek = input.bool(defval = true, title="Show Previous Week High/Low", group="Mondays, Weekly, Monthly", inline="4a")
ExtendPrevWeek = input.bool(defval = false, title="Extend?", group="Mondays, Weekly, Monthly", inline="4a")
LabelPrevWeek = input.bool(defval = true, title="Label?", group="Mondays, Weekly, Monthly", inline="4a")
MidPrevWeek = input.bool(defval = false, title="50%?", group="Mondays, Weekly, Monthly", inline="4a")
PrevWeekHighCol = input.color(color.green, "High", group="Mondays, Weekly, Monthly", inline="4c")
PrevWeekLowCol = input.color(color.red, "Low", group = "Mondays, Weekly, Monthly", inline="4c")
PrevWeekMidCol = input.color(color.green, "0.5", group = "Mondays, Weekly, Monthly", inline="4c")
PrevWeekWidth = input.int(defval=1, title="Width", minval=1, maxval = 4, group="Mondays, Weekly, Monthly", inline="4c")
PrevWeekOff = input.int(defval = 0, title="Label & Line Offset", group="Mondays, Weekly, Monthly", inline="4d")

ShowCurMth = input.bool(defval = false, title="Show Current Month High/Low", group="Mondays, Weekly, Monthly", inline="5a")
ExtendCurMth = input.bool(defval = false, title="Extend?", group="Mondays, Weekly, Monthly", inline="5a")
LabelCurMth = input.bool(defval = false, title="Label?", group="Mondays, Weekly, Monthly", inline="5a")
MidCurMth = input.bool(defval = false, title="50%?", group="Mondays, Weekly, Monthly", inline="5a")
CurMthHighCol = input.color(color.green, "High", group="Mondays, Weekly, Monthly", inline="5c")
CurMthLowCol = input.color(color.red, "Low", group = "Mondays, Weekly, Monthly", inline="5c")
CurMthMidCol = input.color(color.green, "0.5", group = "Mondays, Weekly, Monthly", inline="5c")
CurMthWidth = input.int(defval=1, title="Width", minval=1, maxval = 4, group="Mondays, Weekly, Monthly", inline="5c")
CurMthOff = input.int(defval = 0, title="Label & Line Offset", group="Mondays, Weekly, Monthly", inline="5d")

ShowPrevMth = input.bool(defval = true, title="Show Previous Month High/Low", group="Mondays, Weekly, Monthly", inline="6a")
ExtendPrevMth = input.bool(defval = false, title="Extend?", group="Mondays, Weekly, Monthly", inline="6a")
LabelPrevMth = input.bool(defval = true, title="Label?", group="Mondays, Weekly, Monthly", inline="6a")
MidPrevMth = input.bool(defval = false, title="50%?", group="Mondays, Weekly, Monthly", inline="6a")
PrevMthHighCol = input.color(color.green, "High", group="Mondays, Weekly, Monthly", inline="6c")
PrevMthLowCol = input.color(color.red, "Low", group = "Mondays, Weekly, Monthly", inline="6c")
PrevMthMidCol = input.color(color.green, "0.5", group = "Mondays, Weekly, Monthly", inline="6c")
PrevMthWidth = input.int(defval=1, title="Width", minval=1, maxval = 4, group="Mondays, Weekly, Monthly", inline="6c")
PrevMthOff = input.int(defval = 0, title="Label & Line Offset", group="Mondays, Weekly, Monthly", inline="6d")

ShowWeekOpen = input.bool(defval = false, title="Show Current Week Open", group="Opens", inline="3a")
ExtendWeekOpen = input.bool(defval = false, title="Extend?", group="Opens", inline="3a")
LabelWeekOpen = input.bool(defval = false, title="Label?", group="Opens", inline="3a")
WeekOpenCol = input.color(color.blue, "Open", group="Opens", inline="3c")
WeekOpenWidth = input.int(defval=1, title="Width", minval=1, maxval = 4, group="Opens", inline="3c")

ShowMthOpen = input.bool(defval = false, title="Show Current Month Open", group="Opens", inline="4a")
ExtendMthOpen = input.bool(defval = false, title="Extend?", group="Opens", inline="4a")
LabelMthOpen = input.bool(defval = false, title="Label?", group="Opens", inline="4a")
MthOpenCol = input.color(color.blue, "Open", group="Opens", inline="4c")
MthOpenWidth = input.int(defval=1, title="Width", minval=1, maxval = 4, group="Opens", inline="4c")

zone = input.string('GMT+1', title='Timezone', options=['GMT-11', 'GMT-10', 'GMT-9', 'GMT-8', 'GMT-7', 'GMT-6', 'GMT-5', 'GMT-4', 'GMT-3', 'GMT-2', 'GMT-1', 'GMT', 'GMT+1', 'GMT+2', 'GMT+3', 'GMT+330', 'GMT+4', 'GMT+430', 'GMT+5', 'GMT+530', 'GMT+6', 'GMT+7', 'GMT+8', 'GMT+9', 'GMT+10', 'GMT+11', 'GMT+12'], tooltip='e.g. \'America/New_York\', \'Asia/Tokyo\', \'GMT-4\', \'GMT+9\'...', group="Sessions")

ShowAsia = input.bool(defval = true, title="Show Asia Session   ", group="Sessions", inline="1")
ExtendAsia = input.bool(defval = false, title="Extend?", group="Sessions", inline="1")
AsiaOpen = input.bool(defval = false, title="Show Open?", group="Sessions", inline="1")
AsiaOpenCol = input.color(color.new(color.green,25), title="Open Colour", group="Sessions", inline="1")
AsiaSession = input.session("2200-0700:1234567", "Asia Session   ", group="Sessions", inline="2")
AsiaCol = input.color(color.new(color.green,90), "Colour", group="Sessions", inline="2")

ShowLon = input.bool(defval = true, title="Show London Session", group="Sessions", inline="4")
ExtendLon = input.bool(defval = false, title="Extend?", group="Sessions", inline="4")
LonOpen = input.bool(defval = false, title="Show Open?", group="Sessions", inline="4")
LonOpenCol = input.color(color.new(color.red,25), title="Open Colour", group="Sessions", inline="4")
LonSession = input.session("0800-1700:1234567", "London Session", group="Sessions", inline="5")
LonCol = input.color(color.new(color.red,90), "Colour", group="Sessions", inline="5")

ShowNY = input.bool(defval = true, title="Show NY Session    ", group="Sessions", inline="7")
ExtendNY = input.bool(defval = false, title="Extend?", group="Sessions", inline="7")
NYOpen = input.bool(defval = false, title="Show Open?", group="Sessions", inline="7")
NYOpenCol = input.color(color.new(color.aqua,25), title="Open Colour", group="Sessions", inline="7")
NYSession = input.session("1300-2100:1234567", "NY Session    ", group="Sessions", inline="8")
NYCol = input.color(color.new(color.aqua,90), "Colour", group="Sessions", inline="8")

i_show_history = input.bool(defval=false, title='History?', group="Sessions", inline="A")
i_sess_border_style = input.string(line.style_solid, 'Line Style', options=[line.style_solid, line.style_dotted, line.style_dashed], group="Sessions", inline="B")
i_sess_border_width = input.int(1, 'Line Width', minval=0, group="Sessions", inline="B")
i_sess_bgopacity = input.int(80, 'Background Transparency', minval=0, maxval=100, step=1, group="Sessions", tooltip='100 = No Background', inline="C")

Custom1 = input.bool(defval = false, title="Show Custom Level 1  ", group="Custom Levels", inline="1")
Custom1Label = input.string(defval="", title="Label", group="Custom Levels", inline="1")
Custom1Type = input.string('high', title='  Level', options=["high", "low", "open"], group="Custom Levels", inline="1")
Custom1Time = input.session("0000-2345", "Custom Time Range", group="Custom Levels", inline="1a")
Custom1Days = input.string("23456", title="Days Included     ", tooltip="Enter a digit for each day of the week\n1 = Sunday, 2 = Monday, 3 = Tuesday, etc\nFor example Monday to Friday would be 23456.\nFor a continous line through all included days, use time 00:00 - 00:00\nFor a separate line per day, use any time from 00:00 - 23:45.", group="Custom Levels", inline="1a")
Custom1Col = input.color(defval = color.black, title="Custom Level 1 Colour", group="Custom Levels", inline="1b")
ExtendCustom1 = input.bool(defval = false, title="Extend?", group="Custom Levels", inline="1b")
Custom1History = input.bool(defval=false, title='History?', group="Custom Levels", inline="1b")

Custom2 = input.bool(defval = false, title="Show Custom Level 2  ", group="Custom Levels", inline="2")
Custom2Label = input.string(defval="", title="Label", group="Custom Levels", inline="2")
Custom2Type = input.string('high', title='  Level', options=["high", "low", "open"], group="Custom Levels", inline="2")
Custom2Time = input.session("0015-2345", "Custom Time Range",  tooltip="", group="Custom Levels", inline="2a")
Custom2Days = input.string("23456", title="Days Included     ", tooltip="Enter a digit for each day of the week\n1 = Sunday, 2 = Monday, 3 = Tuesday, etc\nFor example Monday to Friday would be 23456.\nFor a continous line through all included days, use time 00:00 - 00:00\nFor a separate line per day, use any time from 00:00 - 23:45.", group="Custom Levels", inline="2a")
Custom2Col = input.color(defval = color.black, title="Custom Level 2 Colour", group="Custom Levels", inline="2b")
ExtendCustom2 = input.bool(defval = false, title="Extend?", group="Custom Levels", inline="2b")
Custom2History = input.bool(defval=false, title='History?', group="Custom Levels", inline="2b")

Custom3 = input.bool(defval = false, title="Show Custom Level 3  ", group="Custom Levels", inline="3")
Custom3Label = input.string(defval="", title="Label", group="Custom Levels", inline="3")
Custom3Type = input.string('high', title='  Level', options=["high", "low", "open"], group="Custom Levels", inline="3")
Custom3Time = input.session("1200-1400", "Custom Time Range", group="Custom Levels", inline="3a")
Custom3Days = input.string("23456", title="Days Included     ", tooltip="Enter a digit for each day of the week\n1 = Sunday, 2 = Monday, 3 = Tuesday, etc\nFor example Monday to Friday would be 23456.\nFor a continous line through all included days, use time 00:00 - 00:00\nFor a separate line per day, use any time from 00:00 - 23:45.", group="Custom Levels", inline="3a")
Custom3Col = input.color(defval = color.black, title="Custom Level 3 Colour", group="Custom Levels", inline="3b")
ExtendCustom3 = input.bool(defval = false, title="Extend?", group="Custom Levels", inline="3b")
Custom3History = input.bool(defval=false, title='History?', group="Custom Levels", inline="3b")

Custom4 = input.bool(defval = false, title="Show Custom Level 4  ", group="Custom Levels", inline="4")
Custom4Label = input.string(defval="", title="Label", group="Custom Levels", inline="4")
Custom4Type = input.string('high', title='  Level', options=["high", "low", "open"], group="Custom Levels", inline="4")
Custom4Time = input.session("0000-2345", "Custom Time Range", group="Custom Levels", inline="4a")
Custom4Days = input.string("23456", title="Days Included     ", tooltip="Enter a digit for each day of the week\n1 = Sunday, 2 = Monday, 3 = Tuesday, etc\nFor example Monday to Friday would be 23456.\nFor a continous line through all included days, use time 00:00 - 00:00\nFor a separate line per day, use any time from 00:00 - 23:45.", group="Custom Levels", inline="4a")
Custom4Col = input.color(defval = color.black, title="Custom Level 4 Colour", group="Custom Levels", inline="4b")
ExtendCustom4 = input.bool(defval = false, title="Extend?", group="Custom Levels", inline="4b")
Custom4History = input.bool(defval=false, title='History?', group="Custom Levels", inline="4b")

Custom5 = input.bool(defval = false, title="Show Custom Level 5  ", group="Custom Levels", inline="5")
Custom5Label = input.string(defval="", title="Label", group="Custom Levels", inline="5")
Custom5Type = input.string('high', title='  Level', options=["high", "low", "open"], group="Custom Levels", inline="5")
Custom5Time = input.session("0015-2345", "Custom Time Range",  tooltip="", group="Custom Levels", inline="5a")
Custom5Days = input.string("23456", title="Days Included     ", tooltip="Enter a digit for each day of the week\n1 = Sunday, 2 = Monday, 3 = Tuesday, etc\nFor example Monday to Friday would be 23456.\nFor a continous line through all included days, use time 00:00 - 00:00\nFor a separate line per day, use any time from 00:00 - 23:45.", group="Custom Levels", inline="5a")
Custom5Col = input.color(defval = color.black, title="Custom Level 5 Colour", group="Custom Levels", inline="5b")
ExtendCustom5 = input.bool(defval = false, title="Extend?", group="Custom Levels", inline="5b")
Custom5History = input.bool(defval=false, title='History?', group="Custom Levels", inline="5b")

Custom6 = input.bool(defval = false, title="Show Custom Level 6  ", group="Custom Levels", inline="6")
Custom6Label = input.string(defval="", title="Label", group="Custom Levels", inline="6")
Custom6Type = input.string('high', title='  Level', options=["high", "low", "open"], group="Custom Levels", inline="6")
Custom6Time = input.session("1200-1400", "Custom Time Range", group="Custom Levels", inline="6a")
Custom6Days = input.string("23456", title="Days Included     ", tooltip="Enter a digit for each day of the week\n1 = Sunday, 2 = Monday, 3 = Tuesday, etc\nFor example Monday to Friday would be 23456.\nFor a continous line through all included days, use time 00:00 - 00:00\nFor a separate line per day, use any time from 00:00 - 23:45.", group="Custom Levels", inline="6a")
Custom6Col = input.color(defval = color.black, title="Custom Level 6 Colour", group="Custom Levels", inline="6b")
ExtendCustom6 = input.bool(defval = false, title="Extend?", group="Custom Levels", inline="6b")
Custom6History = input.bool(defval=false, title='History?', group="Custom Levels", inline="6b")


Custom1Session = Custom1Time+":"+Custom1Days
Custom2Session = Custom2Time+":"+Custom2Days
Custom3Session = Custom3Time+":"+Custom3Days
Custom4Session = Custom4Time+":"+Custom4Days
Custom5Session = Custom5Time+":"+Custom5Days
Custom6Session = Custom6Time+":"+Custom6Days

ExtendMon = (ExtendMonday == true ? extend.right : extend.none)
ExtendPD = (ExtendPrevDay == true ? extend.right : extend.none)
ExtendPrevM = (ExtendPrevMth == true ? extend.right : extend.none)
ExtendPrevW = (ExtendPrevWeek == true ? extend.right : extend.none)
ExtendCurW = (ExtendCurWeek == true ? extend.right : extend.none)
ExtendCurM = (ExtendCurMth == true ? extend.right : extend.none)
ExtendWeekO = (ExtendWeekOpen == true ? extend.right : extend.none)
ExtendMthO = (ExtendMthOpen == true ? extend.right : extend.none)


ResolutionToSec(res)=>
    mins = res == "1" ? 1 :
           res == "3" ? 3 :
           res == "5" ? 5 :
           res == "10" ? 10 :
           res == "15" ? 15 :
           res == "30" ? 30 :
           res == "45" ? 45 :
           res == "60" ? 60 :
           res == "120" ? 120 :
           res == "180" ? 180 :
           res == "240" ? 240 :
           res == "D" or res == "1D" ? 1440 :
           res == "W" or res == "1W" ? 10080 :
           res == "M" or res == "1M" ? 43200 : 0
    ms = mins * 60 * 1000

bar = ResolutionToSec(timeframe.period)

get_Monday()=>
    highs= array.new_float(0)
    lows= array.new_float(0)
    starttimes= array.new_int(0)
    for n = 0 to 6
        array.push(highs, high[n])
        array.push(lows, low[n])
        array.push(starttimes, time[n])
    [highs, lows, starttimes]
    
   
get_Weekly()=>
    highs= array.new_float(0)
    lows= array.new_float(0)
    opens = array.new_float(0)
    starttimes= array.new_int(0)
    for n = 0 to 1
        array.push(highs, high[n])
        array.push(lows, low[n])
        array.push(opens, open[n])
        array.push(starttimes, time[n])
    [highs, lows, opens, starttimes]
    
    
get_Monthly()=>
    highs= array.new_float(0)
    lows= array.new_float(0)
    opens = array.new_float(0)
    starttimes= array.new_int(0)
    for n = 0 to 1
        array.push(highs, high[n])
        array.push(lows, low[n])
        array.push(opens, open[n])
        array.push(starttimes, time[n])
    [highs, lows, opens, starttimes]
    
   
get_Sessions()=>
    highs= array.new_float(0)
    lows= array.new_float(0)
    starttimes= array.new_int(0)
    for n = 0 to 96
        array.push(highs, high[n])
        array.push(lows, low[n])
        array.push(starttimes, time[n])
    [highs, lows, starttimes]
    
[shighs, slows, sstarttimes] = request.security(syminfo.tickerid, "15", get_Sessions(), lookahead = barmerge.lookahead_off)
[mhighs, mlows, mstarttimes] = request.security(syminfo.tickerid, "1D", get_Monday(), lookahead = barmerge.lookahead_off)
[whighs, wlows, wopens, wstarttimes] = request.security(syminfo.tickerid, "1W", get_Weekly(), lookahead = barmerge.lookahead_off)
[mthhighs, mthlows, mthopens, mthstarttimes] = request.security(syminfo.tickerid, "1M", get_Monthly(), lookahead = barmerge.lookahead_off)

MHt = ""
MMt = ""
MLt = ""
WHt = ""
WLt = ""
WMt = ""
PWHt = ""
PWLt = ""
PWMt = ""
PDHt = ""
PDLt = ""
PDMt = ""
MthHt = ""
MthLt = ""
MthMt = ""
PMthHt = ""
PMthMt = ""
PMthLt = ""
WOt = ""
MthOt = ""

MH_cross = false
ML_cross = false
Mm_cross = false
PDH_cross = false
PDL_cross = false
PDm_cross = false
WH_cross = false
WL_cross = false
Wm_cross = false
WO_cross = false
PWH_cross = false
PWL_cross = false
PWm_cross = false
PMthH_cross = false
PMthL_cross = false
PMthm_cross = false
MthH_cross = false
MthL_cross = false
Mthm_cross = false
MO_cross = false

if (bar_index == last_bar_index)  

    if (abr == true)
        MHt := "MH"
        MLt := "ML"
        MMt := "M.5"
        WHt := "WH"
        WLt := "WL"
        WMt := "W.5"
        PWHt := "PWH"
        PWLt := "PWL"
        PWMt := "PW.5"
        PDHt := "PDH"
        PDLt := "PDL"
        PDMt := "PD.5"
        MthHt := "MthH"
        MthLt := "MthL"
        MthMt := "Mth.5"
        PMthHt := "PMthH"
        PMthLt := "PMthL"
        PMthMt := "PMth.5"
        WOt := "WO"
        MthOt := "MthO"
    else
        MHt := "Mon High"
        MLt := "Mon Low"
        MMt := "Mon 0.5" 
        WHt := "Weekly High"
        WMt := "Weekly 0.5"
        WLt := "Weekly Low"
        PWHt := "Prev Week High"
        PWMt := "Prev Week 0.5"
        PWLt := "Prev Week Low"
        PDHt := "Prev Day High"
        PDLt := "Prev Day Low"
        PDMt := "Prev Day 0.5"
        MthHt := "Month High"
        MthMt := "Month 0.5"
        MthLt := "Month Low"
        PMthHt := "Prev Month High"
        PMthMt := "Prev Month 0.5"
        PMthLt := "Prev Month Low"
        WOt := "Week Open"
        MthOt := "Month Open"
    
    
    
    for x = 0 to array.size(mhighs)-1
        if (dayofweek(array.get(mstarttimes, x), syminfo.timezone) == 2 and ShowMonday == true)
            MH = line.new(x1 = array.get(mstarttimes, x), y1 = array.get(mhighs, x), x2 = array.get(mstarttimes, x)+(86400000*(dayofweek(timenow, syminfo.timezone)-1))+(bar*MonOff), y2 = array.get(mhighs, x), xloc= xloc.bar_time, extend=ExtendMon, color=MonHighCol, style= line.style_solid, width=MonWidth)
            ML = line.new(x1 = array.get(mstarttimes, x), y1 = array.get(mlows, x), x2 = array.get(mstarttimes, x)+(86400000*(dayofweek(timenow, syminfo.timezone)-1))+(bar*MonOff), y2 = array.get(mlows, x), xloc= xloc.bar_time, extend=ExtendMon, color= MonLowCol, style= line.style_solid, width= MonWidth)
            line.delete(MH[1])
            line.delete(ML[1])
            MH_cross := ta.cross(close, array.get(mhighs, x))
            ML_cross := ta.cross(close, array.get(mlows, x))
            if (MidMonday)
                MM = line.new(x1 = array.get(mstarttimes, x), y1 = array.get(mhighs, x)-(array.get(mhighs, x)-array.get(mlows, x))/2, x2 = array.get(mstarttimes, x)+(86400000*(dayofweek(timenow, syminfo.timezone)-1))+(bar*MonOff), y2 = array.get(mhighs, x)-(array.get(mhighs, x)-array.get(mlows, x))/2, xloc= xloc.bar_time, extend=ExtendMon, color= MonMidCol, style= line.style_solid, width= MonWidth)
                Mm_cross := ta.cross(close, array.get(mhighs, x)-(array.get(mhighs, x)-array.get(mlows, x))/2)
                line.delete(MM[1])
            if (LabelMonday == true)   
                MHL = label.new(x=array.get(mstarttimes, x)+(86400000*(dayofweek(timenow, syminfo.timezone)-1))+(bar*MonOff), y=array.get(mhighs, x)*1.0001, text=MHt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=MonHighCol, size=sz, textalign=text.align_right)
                MLL = label.new(x=array.get(mstarttimes, x)+(86400000*(dayofweek(timenow, syminfo.timezone)-1))+(bar*MonOff), y=array.get(mlows, x)*1.0001, text=MLt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=MonLowCol, size=sz, textalign=text.align_right)
                label.delete(MHL[1])
                label.delete(MLL[1])
                if (MidMonday)
                    MML = label.new(x=array.get(mstarttimes, x)+(86400000*(dayofweek(timenow, syminfo.timezone)-1))+(bar*MonOff), y=(array.get(mhighs, x)-(array.get(mhighs, x)-array.get(mlows, x))/2)*1.0001, text=MMt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=MonMidCol, size=sz, textalign=text.align_right)
                    label.delete(MML[1])
                
    if (ShowPrevDay == true)
        PDH = line.new(x1 = array.get(mstarttimes, 1), y1 = array.get(mhighs, 1), x2 = array.get(mstarttimes, 1)+(86400000*2)+(bar*PrevDayOff), y2 = array.get(mhighs, 1), xloc= xloc.bar_time, extend=ExtendPD, color=PrevDayHighCol, style= line.style_solid, width=PrevDayWidth)
        PDL = line.new(x1 = array.get(mstarttimes, 1), y1 = array.get(mlows, 1), x2 = array.get(mstarttimes, 1)+(86400000*2)+(bar*PrevDayOff), y2 = array.get(mlows, 1), xloc= xloc.bar_time, extend=ExtendPD, color= PrevDayLowCol, style= line.style_solid, width= PrevDayWidth)
        line.delete(PDH[1])
        line.delete(PDL[1])
        PDH_cross := ta.cross(close, array.get(mhighs, 1))
        PDL_cross := ta.cross(close, array.get(mlows, 1))
        if (MidPrevDay)
            PDM = line.new(x1 = array.get(mstarttimes, 1), y1 = array.get(mhighs, 1)-(array.get(mhighs, 1)-array.get(mlows, 1))/2, x2 = array.get(mstarttimes, 1)+(86400000*2)+(bar*PrevDayOff), y2 = array.get(mhighs, 1)-(array.get(mhighs, 1)-array.get(mlows, 1))/2, xloc= xloc.bar_time, extend=ExtendPD, color= PrevDayMidCol, style= line.style_solid, width= PrevDayWidth)
            PDm_cross := ta.cross(close, array.get(mhighs, 1)-(array.get(mhighs, 1)-array.get(mlows, 1))/2)
            line.delete(PDM[1])
        if (LabelPrevDay == true)   
            PDHL = label.new(x=array.get(mstarttimes, 1)+(86400000*2)+(bar*PrevDayOff), y=array.get(mhighs, 1)*1.0001, text=PDHt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=PrevDayHighCol, size=sz, textalign=text.align_right)
            PDLL = label.new(x=array.get(mstarttimes, 1)+(86400000*2)+(bar*PrevDayOff), y=array.get(mlows, 1)*1.0001, text=PDLt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=PrevDayLowCol, size=sz, textalign=text.align_right)
            label.delete(PDHL[1])
            label.delete(PDLL[1])
            if (MidPrevDay)
                PDML = label.new(x=array.get(mstarttimes, 1)+(86400000*2)+(bar*PrevDayOff), y=array.get(mhighs, 1)-(array.get(mhighs, 1)-array.get(mlows, 1))/2*1.0001, text=PDMt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=PrevDayMidCol, size=sz, textalign=text.align_right)
                label.delete(PDML[1])

                
    if (ShowPrevWeek == true)
        PWH = line.new(x1 = array.get(wstarttimes, 1), y1 = array.get(whighs, 1), x2 = array.get(wstarttimes, 1)+(86400000*(dayofweek(timenow, syminfo.timezone)+6))+(bar*PrevWeekOff), y2 = array.get(whighs, 1), xloc= xloc.bar_time, extend=ExtendPrevW, color=PrevWeekHighCol, style= line.style_solid, width=PrevWeekWidth)
        PWL = line.new(x1 = array.get(wstarttimes, 1), y1 = array.get(wlows, 1), x2 = array.get(wstarttimes, 1)+(86400000*(dayofweek(timenow, syminfo.timezone)+6))+(bar*PrevWeekOff), y2 = array.get(wlows, 1), xloc= xloc.bar_time, extend=ExtendPrevW, color= PrevWeekLowCol, style= line.style_solid, width= PrevWeekWidth)
        line.delete(PWH[1])
        line.delete(PWL[1])
        PWH_cross := ta.cross(close, array.get(whighs, 1))
        PWL_cross := ta.cross(close, array.get(wlows, 1))
        if (MidPrevWeek)
            PWM = line.new(x1 = array.get(wstarttimes, 1), y1 = array.get(whighs, 1) -(array.get(whighs, 1)-array.get(wlows, 1))/2, x2 = array.get(wstarttimes, 1)+(86400000*(dayofweek(timenow, syminfo.timezone)+6))+(bar*PrevWeekOff), y2 = array.get(whighs, 1) -(array.get(whighs, 1)-array.get(wlows, 1))/2, xloc= xloc.bar_time, extend=ExtendPrevW, color= PrevWeekMidCol, style= line.style_solid, width= PrevWeekWidth)
            line.delete(PWM[1])
            PWm_cross := ta.cross(close, array.get(whighs, 1) -(array.get(whighs, 1)-array.get(wlows, 1))/2)
        if (LabelPrevWeek == true)   
            PWHL = label.new(x=array.get(wstarttimes, 1)+(86400000*(dayofweek(timenow, syminfo.timezone)+6))+(bar*PrevWeekOff), y=array.get(whighs, 1)*1.0001, text=PWHt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=PrevWeekHighCol, size=sz, textalign=text.align_right)
            PWLL = label.new(x=array.get(wstarttimes, 1)+(86400000*(dayofweek(timenow, syminfo.timezone)+6))+(bar*PrevWeekOff), y=array.get(wlows, 1)*1.0001, text=PWLt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=PrevWeekLowCol, size=sz, textalign=text.align_right)
            label.delete(PWHL[1])
            label.delete(PWLL[1])
            if (MidPrevWeek)
                PWML = label.new(x=array.get(wstarttimes, 1)+(86400000*(dayofweek(timenow, syminfo.timezone)+6))+(bar*PrevWeekOff), y=array.get(whighs, 1) -(array.get(whighs, 1)-array.get(wlows, 1))/2*1.0001, text=PWMt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=PrevWeekMidCol, size=sz, textalign=text.align_right)
                label.delete(PWML[1])

    if (ShowCurWeek == true)
        CWH = line.new(x1 = array.get(wstarttimes, 0), y1 = array.get(whighs, 0), x2 = array.get(wstarttimes, 0)+(86400000*(dayofweek(timenow, syminfo.timezone)-1))+(bar*CurWeekOff), y2 = array.get(whighs, 0), xloc= xloc.bar_time, extend=ExtendCurW, color=CurWeekHighCol, style= line.style_solid, width=CurWeekWidth)
        CWL = line.new(x1 = array.get(wstarttimes, 0), y1 = array.get(wlows, 0), x2 = array.get(wstarttimes, 0)+(86400000*(dayofweek(timenow, syminfo.timezone)-1))+(bar*CurWeekOff), y2 = array.get(wlows, 0), xloc= xloc.bar_time, extend=ExtendCurW, color= CurWeekLowCol, style= line.style_solid, width= CurWeekWidth)
        line.delete(CWH[1])
        line.delete(CWL[1])
        WH_cross := ta.cross(close, array.get(whighs, 0))
        WL_cross := ta.cross(close, array.get(wlows, 0))
        if (MidCurWeek)
            CWM = line.new(x1 = array.get(wstarttimes, 0), y1 = array.get(whighs, 0)-(array.get(whighs, 0)-array.get(wlows, 0))/2, x2 = array.get(wstarttimes, 0)+(86400000*(dayofweek(timenow, syminfo.timezone)-1))+(bar*CurWeekOff), y2 = array.get(whighs, 0)-(array.get(whighs, 0)-array.get(wlows, 0))/2, xloc= xloc.bar_time, extend=ExtendCurW, color= CurWeekMidCol, style= line.style_solid, width= CurWeekWidth)
            line.delete(CWM[1])
            Wm_cross := ta.cross(close, array.get(whighs, 0)-(array.get(whighs, 0)-array.get(wlows, 0))/2)
        if (LabelCurWeek == true)   
            CWHL = label.new(x=array.get(wstarttimes, 0)+(86400000*(dayofweek(timenow, syminfo.timezone)-1))+(bar*CurWeekOff), y=array.get(whighs, 0)*1.0001, text=WHt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=CurWeekHighCol, size=sz, textalign=text.align_right)
            CWLL = label.new(x=array.get(wstarttimes, 0)+(86400000*(dayofweek(timenow, syminfo.timezone)-1))+(bar*CurWeekOff), y=array.get(wlows, 0)*1.0001, text=WLt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=CurWeekLowCol, size=sz, textalign=text.align_right)
            label.delete(CWHL[1])
            label.delete(CWLL[1])
            if (MidCurWeek)
                CWML = label.new(x=array.get(wstarttimes, 0)+(86400000*(dayofweek(timenow, syminfo.timezone)-1))+(bar*CurWeekOff), y=array.get(whighs, 0)-(array.get(whighs, 0)-array.get(wlows, 0))/2*1.0001, text=WMt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=CurWeekMidCol, size=sz, textalign=text.align_right)
                label.delete(CWML[1])
    
    if (ShowPrevMth == true)
        PMthH = line.new(x1 = array.get(mthstarttimes, 1), y1 = array.get(mthhighs, 1), x2 = array.get(mthstarttimes, 1)+(86400000*(dayofmonth(timenow, syminfo.timezone)+31))+(bar*PrevMthOff), y2 = array.get(mthhighs, 1), xloc= xloc.bar_time, extend=ExtendPrevM, color=PrevMthHighCol, style= line.style_solid, width=PrevMthWidth)
        PMthL = line.new(x1 = array.get(mthstarttimes, 1), y1 = array.get(mthlows, 1), x2 = array.get(mthstarttimes, 1)+(86400000*(dayofmonth(timenow, syminfo.timezone)+31))+(bar*PrevMthOff), y2 = array.get(mthlows, 1), xloc= xloc.bar_time, extend=ExtendPrevM, color= PrevMthLowCol, style= line.style_solid, width= PrevMthWidth)
        line.delete(PMthH[1])
        line.delete(PMthL[1])
        PMthH_cross := ta.cross(close, array.get(mthhighs, 1))
        PMthL_cross := ta.cross(close, array.get(mthlows, 1))
        if (MidPrevMth)
            PMthM = line.new(x1 = array.get(mthstarttimes, 1), y1 = array.get(mthhighs, 1)-(array.get(mthhighs, 1)-array.get(mthlows, 1))/2, x2 = array.get(mthstarttimes, 1)+(86400000*(dayofmonth(timenow, syminfo.timezone)+31))+(bar*PrevMthOff), y2 = array.get(mthhighs, 1)-(array.get(mthhighs, 1)-array.get(mthlows, 1))/2, xloc= xloc.bar_time, extend=ExtendPrevM, color= PrevMthMidCol, style= line.style_solid, width= PrevMthWidth)
            line.delete(PMthM[1])
            PMthm_cross := ta.cross(close, array.get(mthhighs, 1)-(array.get(mthhighs, 1)-array.get(mthlows, 1))/2)
        if (LabelPrevMth == true)   
            PMthHL = label.new(x=array.get(mthstarttimes, 1)+(86400000*(dayofmonth(timenow, syminfo.timezone)+31))+(bar*PrevMthOff), y=array.get(mthhighs, 1)*1.0001, text=PMthHt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=PrevMthHighCol, size=sz, textalign=text.align_right)
            PMthLL = label.new(x=array.get(mthstarttimes, 1)+(86400000*(dayofmonth(timenow, syminfo.timezone)+31))+(bar*PrevMthOff), y=array.get(mthlows, 1)*1.0001, text=PMthLt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=PrevMthLowCol, size=sz, textalign=text.align_right)
            label.delete(PMthHL[1])
            label.delete(PMthLL[1])
            if (MidPrevMth)
                PMthML = label.new(x=array.get(mthstarttimes, 1)+(86400000*(dayofmonth(timenow, syminfo.timezone)+31))+(bar*PrevMthOff), y=array.get(mthhighs, 1)-(array.get(mthhighs, 1)-array.get(mthlows, 1))/2*1.0001, text=PMthMt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=PrevMthMidCol, size=sz, textalign=text.align_right)
                label.delete(PMthML[1])
            
    if (ShowCurMth == true)
        CMthH = line.new(x1 = array.get(mthstarttimes, 0), y1 = array.get(mthhighs, 0), x2 = array.get(mthstarttimes, 0)+(86400000*(dayofmonth(timenow, syminfo.timezone)))+(bar*CurMthOff), y2 = array.get(mthhighs, 0), xloc= xloc.bar_time, extend=ExtendCurM, color=CurMthHighCol, style= line.style_solid, width=CurMthWidth)
        CMthL = line.new(x1 = array.get(mthstarttimes, 0), y1 = array.get(mthlows, 0), x2 = array.get(mthstarttimes, 0)+(86400000*(dayofmonth(timenow, syminfo.timezone)))+(bar*CurMthOff), y2 = array.get(mthlows, 0), xloc= xloc.bar_time, extend=ExtendCurM, color= CurMthLowCol, style= line.style_solid, width= CurMthWidth)
        line.delete(CMthH[1])
        line.delete(CMthL[1])
        MthH_cross := ta.cross(close, array.get(mthhighs, 0))
        MthL_cross := ta.cross(close, array.get(mthlows, 0))
        if (MidCurMth)
            CMthM = line.new(x1 = array.get(mthstarttimes, 0), y1 = array.get(mthhighs, 0)-(array.get(mthhighs, 0)-array.get(mthlows, 0))/2, x2 = array.get(mthstarttimes, 0)+(86400000*(dayofmonth(timenow, syminfo.timezone)))+(bar*CurMthOff), y2 = array.get(mthhighs, 0)-(array.get(mthhighs, 0)-array.get(mthlows, 0))/2, xloc= xloc.bar_time, extend=ExtendCurM, color= CurMthMidCol, style= line.style_solid, width= CurMthWidth)
            line.delete(CMthM[1])
            Mthm_cross := ta.cross(close, array.get(mthhighs, 0)-(array.get(mthhighs, 0)-array.get(mthlows, 0))/2)
        if (LabelCurMth == true)   
            CMthHL = label.new(x=array.get(mthstarttimes, 0)+(86400000*(dayofmonth(timenow, syminfo.timezone)))+(bar*CurMthOff), y=array.get(mthhighs, 0)*1.0001, text=MthHt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=CurMthHighCol, size=sz, textalign=text.align_right)
            CMthLL = label.new(x=array.get(mthstarttimes, 0)+(86400000*(dayofmonth(timenow, syminfo.timezone)))+(bar*CurMthOff), y=array.get(mthlows, 0)*1.0001, text=MthLt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=CurMthLowCol, size=sz, textalign=text.align_right)
            label.delete(CMthHL[1])
            label.delete(CMthLL[1])
            if (MidCurMth)
                CMthML = label.new(x=array.get(mthstarttimes, 0)+(86400000*(dayofmonth(timenow, syminfo.timezone)))+(bar*CurMthOff), y=array.get(mthhighs, 0)-(array.get(mthhighs, 0)-array.get(mthlows, 0))/2*1.0001, text=MthMt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=CurMthMidCol, size=size.small, textalign=text.align_right)
                label.delete(CMthML[1])
    
    
    if (ShowWeekOpen == true)
        WO = line.new(x1 = array.get(wstarttimes, 0), y1 = array.get(wopens, 0), x2 = array.get(wstarttimes, 0)+(86400000*(dayofweek(timenow, syminfo.timezone)-1)), y2 = array.get(wopens, 0), xloc= xloc.bar_time, extend=ExtendWeekO, color=WeekOpenCol, style= line.style_solid, width=WeekOpenWidth)
        line.delete(WO[1])
        WO_cross := ta.cross(close,  array.get(wopens, 0))    
        if (LabelWeekOpen == true)   
            WOL = label.new(x=array.get(wstarttimes, 0)+(86400000*(dayofweek(timenow, syminfo.timezone)-1)), y=array.get(wopens, 0)*1.0001, text=WOt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=WeekOpenCol, size=sz, textalign=text.align_right)
            label.delete(WOL[1])
            
    if (ShowMthOpen == true)
        MthO = line.new(x1 = array.get(mthstarttimes, 0), y1 = array.get(mthopens, 0), x2 = array.get(mthstarttimes, 0)+(86400000*(dayofmonth(timenow, syminfo.timezone))), y2 = array.get(mthopens, 0), xloc= xloc.bar_time, extend=ExtendMthO, color=MthOpenCol, style= line.style_solid, width=MthOpenWidth)
        line.delete(MthO[1])
        MO_cross := ta.cross(close,  array.get(mthopens, 0))    
        if (LabelMthOpen == true)   
            MthOL = label.new(x=array.get(mthstarttimes, 0)+(86400000*(dayofmonth(timenow, syminfo.timezone))), y=array.get(mthopens, 0)*1.0001, text=MthOt, xloc=xloc.bar_time, color=color.white, style=label.style_none, textcolor=MthOpenCol, size=sz, textalign=text.align_right)
            label.delete(MthOL[1])
            
    
///////////////
// Defined
///////////////
show = true
pips = syminfo.mintick * 10
max_bars = 500

fmt_price = '{0,number,#.#####}'
fmt_pips  = '{0,number,#.#}'

icon_separator = ' • '

c_none = color.new(color.black, 100)

is_weekends = dayofweek == 7 or dayofweek == 1

f_get_time_by_bar(bar_count) => timeframe.multiplier * bar_count * 60 * 1000

f_get_period (_session, _start, _lookback) =>
    result = math.max(_start, 1)
    for i = result to _lookback
        if na(_session[i+1]) and _session[i]
            result := i+1
            break
    result

f_get_label_position (_y, _side) =>
    switch _y
        'top'    => _side == 'outside' ? label.style_label_lower_left : label.style_label_upper_left
        'bottom' => _side == 'outside' ? label.style_label_upper_left : label.style_label_lower_left

f_get_day (n) =>
    switch n
        1 => 'Sun'
        2 => 'Mon'
        3 => 'Tue'
        4 => 'Wed'
        5 => 'Thu'
        6 => 'Fri'
        7 => 'Sat'

f_get_started (_session) => na(_session[1]) and _session

f_get_ended (_session) => na(_session) and _session[1]

i_lookback = 12 * 120

f_render_session (_show, _session, _showOpen, _is_started, _is_ended, _color, _openColor, _top, _bottom, _is_extend, _delete_history) =>
    var box my_box = na
    var line my_line = na
    var label my_label = na
    
    x0_1 = ta.valuewhen(na(_session[1]) and _session, bar_index, 1)
    x0_2 = ta.valuewhen(na(_session) and _session[1], bar_index, 0)
    var x1 = 0
    var x2 = 0
    var session_open  = 0.0
    var session_high  = 0.0
    var session_low   = 0.0

    if _show
        if _is_started
            diff = math.abs(x0_2 - x0_1)
            x1 := bar_index
            x2 := bar_index + (math.min(diff, max_bars))
            my_box := box.new(x1, _top, x2, _bottom, color.new(_color, i_sess_bgopacity/1.1), i_sess_border_width, i_sess_border_style, bgcolor=color.new(_color, i_sess_bgopacity))
            back = 0
            if (_showOpen == true)
                my_line := line.new(x1, open, x2, open, color = _openColor, extend= extend.none, style= i_sess_border_style, width=i_sess_border_width)
                my_label := label.new(x=x2-2, y=open, text=(abr == true?"O":"Open"), color=color.white, style=label.style_none, textcolor=_openColor, size=size.small, textalign=text.align_left)
            
            session_open := open
            session_high := _top
            session_low  := _bottom

            if _is_extend
                box.set_extend(my_box, extend.right)
                line.set_extend(my_line, extend.right)
                
            if _delete_history
                box.delete(my_box[1])
                line.delete(my_line[1])
                label.delete(my_label[1])
            else
                back := ((2000 *  60) / int(str.tonumber(timeframe.period))) >= 5000 ? 4999 : ((2000 *  60) / int(str.tonumber(timeframe.period)))
                box.delete(my_box[back])
                
        else if _session
            box.set_top(my_box, _top)
            box.set_bottom(my_box, _bottom)

            session_high := _top
            session_low  := _bottom
        
        else if _is_ended
            session_open := na
            box.set_right(my_box, bar_index)

    [x1, x2, session_open, session_high, session_low]
    

f_render_custom (_show, _session, _is_started, _is_ended, _color, _label, _type, _top, _bottom, _is_extend, _delete_history) =>
    var line my_line = na
    var label my_label = na
    
    x0_1 = ta.valuewhen(na(_session[1]) and _session, bar_index, 1)
    x0_2 = ta.valuewhen(na(_session) and _session[1], bar_index, 0)
    var x1 = 0
    var x2 = 0
    var session_open  = 0.0
    var session_high  = 0.0
    var session_low   = 0.0
    var level = 0.0
    if _show
        level := _type == "high" ? _top : _type == "low" ? _bottom : session_open 
        if _is_started
            diff = math.abs(x0_2 - x0_1)
            x1 := bar_index
            x2 := bar_index + (math.min(diff, max_bars))
            my_line := line.new(x1, level, x2, level, color = _color, extend= extend.none, style= i_sess_border_style, width=i_sess_border_width)
            my_label := label.new(x=x2-2, y=level, text=_label, color=color.white, style=label.style_none, textcolor=_color, size=sz, textalign=text.align_left)
            
            session_open := open
            session_high := _top
            session_low  := _bottom

            if _is_extend
                line.set_extend(my_line, extend.right)
                
            if _delete_history
                line.delete(my_line[1])
                label.delete(my_label[1])
            
            
        else if _session
            line.set_y1(my_line, level)
            line.set_y2(my_line, level)
            label.set_y(my_label, level)
            

            session_high := _top
            session_low  := _bottom
        
        else if _is_ended
            session_open := na
            line.set_x2(my_line, bar_index)
            label.set_x(my_label, bar_index)

    level

    
draw (_show,_showOpen, _session, _color, _openColor, is_extend, _lookback) =>
    max = f_get_period(_session, 1, _lookback)
    top = ta.highest(high, max)
    bottom = ta.lowest(low, max)
    
    is_started = f_get_started(_session)
    is_ended = f_get_ended(_session)
    
    delete_history = (not i_show_history) or is_extend

    [x1, x2, _open, _high, _low] = f_render_session(_show, _session, _showOpen, is_started, is_ended, _color, _openColor, top, bottom, is_extend, delete_history)

    [_session, _open, _high, _low]
    
drawcustom (_show, _session, _color, _label, _type, is_extend, _lookback, custom_history) =>
    max = f_get_period(_session, 1, _lookback)
    top = ta.highest(high, max)
    bottom = ta.lowest(low, max)
    
    is_started = f_get_started(_session)
    is_ended = f_get_ended(_session)
    
    delete_history = (not custom_history) or is_extend

    level = f_render_custom(_show, _session, is_started, is_ended,  _color, _label, _type, top, bottom, is_extend, delete_history)

    level
    


int sess1 = time(timeframe.period, AsiaSession, zone)
int sess2 = time(timeframe.period, LonSession, zone)
int sess3 = time(timeframe.period, NYSession, zone)

int customsess1 = time(timeframe.period, Custom1Session, zone)
int customsess2 = time(timeframe.period, Custom2Session, zone)
int customsess3 = time(timeframe.period, Custom3Session, zone)
int customsess4 = time(timeframe.period, Custom4Session, zone)
int customsess5 = time(timeframe.period, Custom5Session, zone)
int customsess6 = time(timeframe.period, Custom6Session, zone)

draw(ShowAsia, AsiaOpen, sess1, AsiaCol, AsiaOpenCol, ExtendAsia, i_lookback)
draw(ShowLon, LonOpen, sess2, LonCol, LonOpenCol,ExtendLon, i_lookback)
draw(ShowNY, NYOpen, sess3, NYCol, NYOpenCol,ExtendNY, i_lookback)

cust1level = drawcustom(Custom1, customsess1, Custom1Col, Custom1Label, Custom1Type, ExtendCustom1, i_lookback, Custom1History)
cust2level = drawcustom(Custom2, customsess2, Custom2Col, Custom2Label, Custom2Type, ExtendCustom2, i_lookback, Custom2History)
cust3level = drawcustom(Custom3, customsess3, Custom3Col, Custom3Label, Custom3Type, ExtendCustom3, i_lookback, Custom3History)
cust4level = drawcustom(Custom4, customsess4, Custom4Col, Custom4Label, Custom4Type, ExtendCustom1, i_lookback, Custom4History)
cust5level = drawcustom(Custom5, customsess5, Custom5Col, Custom5Label, Custom5Type, ExtendCustom2, i_lookback, Custom5History)
cust6level = drawcustom(Custom6, customsess6, Custom6Col, Custom6Label, Custom6Type, ExtendCustom3, i_lookback, Custom6History)

custom1_cross = false
custom2_cross = false
custom3_cross = false
custom4_cross = false
custom5_cross = false
custom6_cross = false


custom1_cross := ta.cross(close, cust1level)
custom2_cross := ta.cross(close, cust2level)
custom3_cross := ta.cross(close, cust3level)
custom4_cross := ta.cross(close, cust4level)
custom5_cross := ta.cross(close, cust5level)
custom6_cross := ta.cross(close, cust6level)

//alerts

alertcondition(MH_cross, title="Crossing Monday High", message="Price Crossing Monday High")
alertcondition(ML_cross, title="Crossing Monday Low", message="Price Crossing Monday Low")
alertcondition(Mm_cross, title="Crossing Monday Mid", message="Price Crossing Monday Mid")

alertcondition(PDH_cross, title="Crossing Prev Day High", message="Price Crossing Prev Day High")
alertcondition(PDL_cross, title="Crossing Prev Day Low", message="Price Crossing Prev Day Low")
alertcondition(PDm_cross, title="Crossing Prev Day Mid", message="Price Crossing Prev Day Mid")

alertcondition(WH_cross, title="Crossing Current Week High", message="Price Crossing Current Week High")
alertcondition(WL_cross, title="Crossing Current Week Low", message="Price Crossing Current Week Low")
alertcondition(Wm_cross, title="Crossing Current Week Mid", message="Price Crossing Current Week Mid")
alertcondition(WO_cross, title="Crossing Current Week Open", message="Price Crossing Current Week Open")

alertcondition(PWH_cross, title="Crossing Pev Week High", message="Price Crossing Pev Week High")
alertcondition(PWL_cross, title="Crossing Pev Week Low", message="Price Crossing Pev Week Low")
alertcondition(PWm_cross, title="Crossing Pev Week Mid", message="Price Crossing Pev Week Mid")

alertcondition(MthH_cross, title="Crossing Current Month High", message="Price Crossing Current Month High")
alertcondition(MthL_cross, title="Crossing Current Month Low", message="Price Crossing Current Month Low")
alertcondition(Mthm_cross, title="Crossing Current Month Mid", message="Price Crossing Current Month Mid")
alertcondition(MO_cross, title="Crossing Current Month Open", message="Price Crossing Current Month Open")

alertcondition(PMthH_cross, title="Crossing Prev Month High", message="Price Crossing Prev Month High")
alertcondition(PMthL_cross, title="Crossing Prev Month Low", message="Price Crossing Prev Month Low")
alertcondition(PMthm_cross, title="Crossing Prev Month Mid", message="Price Crossing Prev Month Mid")

// alertcondition(custom1_cross, title="Crossing Custom Level 1", message="Price Crossing Custom Level 1")
// alertcondition(custom2_cross, title="Crossing Custom Level 2", message="Price Crossing Custom Level 2")
// alertcondition(custom3_cross, title="Crossing Custom Level 3", message="Price Crossing Custom Level 3")
// alertcondition(custom4_cross, title="Crossing Custom Level 4", message="Price Crossing Custom Level 4")
// alertcondition(custom5_cross, title="Crossing Custom Level 5", message="Price Crossing Custom Level 5")
// alertcondition(custom6_cross, title="Crossing Custom Level 6", message="Price Crossing Custom Level 6")