//This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
//@malk1903
//@version=5
indicator(title='ICT MTF Order Block Wicks [MK]', shorttitle = 'OB Wicks [MK]',overlay=true, max_boxes_count = 500, max_labels_count = 500)

grp_tfs_enabled     =   "ENABLED TIME FRAMES"
tfcstrg             =   "Order Block Fill Colors"

inSession           =   not na(time(timeframe.period, "0930-1600"))

display             =   true
//hover1              =   input.bool(true,"Hover over (!) for information about FVG's",group = "IMPORTANT INFORMATION",tooltip = descTip1+descTip2+descTip3+descTip4+descTip5)
//hover2              =   input.bool(true,"Hover over (!) for alert usage",group = "IMPORTANT INFORMATION",tooltip = alertTip1+alertTip2+alertTip3+alertTip4+alertTip5)
OnlyMktHrs          =   false
fmthds              =   "Body"
fvgmethod           =   fmthds == "Body" ? true : false
show_labels         =   true
show_timeonlabels   =   false
hoursOffsetInput    =   input.float(-5.0, "Timezone offset", minval = -12.0, maxval = 14.0, step = 0.5, group='', inline="0")
label_shift         =   input.int(defval=10, minval=1, maxval=100, step=1, title="OB Offset to Right", group = '', inline ='0')
var msOffsetInput   =   hoursOffsetInput * 1000 * 60 * 60
labelcolor          =   input.color(color.new(color.orange,0),"Label Colour",group = "",inline="1")
incursion_alerts    =   true
incursion_pct       =   20
intrusion_percentage=   incursion_pct / 100

mitiaction          =   "Normal"
mitigationaction    =   if (mitiaction == 'Normal')
    1
else
    if (mitiaction == 'Dynamic')
        2
    else 
        if (mitiaction == 'None')
            3
        else
            4

nomiticolor         =   color.new(color.yellow,85)
nmc_trans           =   color.t(nomiticolor)
show_mitigated_text =   false
mitig_type          =   "Wicks"
UseBodyForMitigation=   mitig_type == "Body" ? true : false
entrychangecolor    =   input.bool(true,"Change OB Color On Entry          ",inline='3')
entry_bull_color    =   input.color(color.new(color.white,90),"Bull",inline='3')
entry_bear_color    =   input.color(color.new(color.white,90),"Bear",inline='3')

// ============================================ //
// FVG Time Frame inputs and settings
// ============================================ //
enable_current_timeframe    = false
enable_5min                 = input.bool(false,"5 Minute",group = grp_tfs_enabled,inline="0")
enable_10min                = input.bool(true,"10 Minute",group = grp_tfs_enabled,inline="0")
enable_15min                = input.bool(true,"15 Minute",group = grp_tfs_enabled,inline="0")
enable_30min                = input.bool(true,"30 Minute",group = grp_tfs_enabled,inline="0")
enable_1hr                  = input.bool(true,"1 Hour",group = grp_tfs_enabled,inline="0")
enable_4hr                  = input.bool(false,"4 Hour",group = grp_tfs_enabled,inline="0")
enable_8hr                  = input.bool(false,"8 Hour",group = grp_tfs_enabled,inline="0")
enable_12hr                 = input.bool(false,"12 Hour",group = grp_tfs_enabled,inline="0")
enable_Daily                = input.bool(false,"Daily",group = grp_tfs_enabled,inline="0")
enable_Week                 = input.bool(false,"Weekly",group = grp_tfs_enabled,inline="0")
enable_Month                = input.bool(false,"Monthly",group = grp_tfs_enabled,inline="0")

current_timeframe = timeframe.period
var timestring = ""
if timeframe.isintraday
    if str.tonumber(current_timeframe) > 59
        timestring := str.tostring(str.tonumber(current_timeframe) / 60) + " Hr" 
    else
        timestring := current_timeframe + " Min"
else if timeframe.isdaily
    timestring := "Daily"
else if timeframe.isweekly
    timestring := "Weekly"
else
    timestring := "Monthly"

// ============================================ //
// FVG Supported Timeframe strings used for labels and alertcondition at end
// ============================================ //
currtfstring        = timestring

maxgroup            = "MAX OBs SETTINGS (COMBINED MAXIMUM MUST BE LESS THAN 500)"   
curr_MaxArraySize   = 8
MaxArraySize_5min   = input.int(8,"5 Min",group = maxgroup,inline="0",minval = 1)
MaxArraySize_10min  = input.int(8,"10 Min",group = maxgroup,inline="0",minval = 1)
MaxArraySize_15min  = input.int(8,"15 Min",group = maxgroup,inline="0",minval = 1)
MaxArraySize_30min  = input.int(8,"30 Min",group = maxgroup,inline="1",minval = 1)
MaxArraySize_1hr    = input.int(8,"1 Hr",group = maxgroup,inline="1",minval = 1)
MaxArraySize_4hr    = input.int(8,"4 Hr",group = maxgroup,inline="1",minval = 1)
MaxArraySize_8hr    = input.int(8,"8 Hr",group = maxgroup,inline="2",minval = 1)
MaxArraySize_12hr    = input.int(8,"12 Hr",group = maxgroup,inline="2",minval = 1)
MaxArraySize_Daily  = input.int(8,"Daily",group = maxgroup,inline="2",minval = 1)
MaxArraySize_Wkly   = input.int(8,"Weekly",group = maxgroup,inline="3",minval = 1)
MaxArraySize_Mthly  = input.int(8,"Monthly",group = maxgroup,inline="3",minval = 1)

//Create error box template (as table) if user entered over 500 fvgs total
var table ErrorBox  = table.new(position.bottom_right, 1, 1, frame_color=color.red,frame_width = 1)
tot_user_max_boxes  = curr_MaxArraySize + MaxArraySize_5min + MaxArraySize_10min + MaxArraySize_15min + MaxArraySize_1hr + MaxArraySize_4hr + MaxArraySize_8hr + MaxArraySize_Daily + MaxArraySize_Wkly + MaxArraySize_Mthly
isError             = tot_user_max_boxes > 500

if isError
    table.cell(ErrorBox, 0, 0, "MTF OB INDICATOR ERROR\n\n"+"Max Number of OBs exceeded, please change settings.", bgcolor = color.new(color.blue, 50), 
     text_color = color.red,text_halign=text.align_center)
    bgcolor=color.new(color.maroon,90)

// SET DEFAULT COLORS FOR ALL TIMEFRAMES
bullfvgcolor        = input.color(color.new(color.yellow,80),"Bull OB Fill Color",group = tfcstrg,inline="0") //"    "
bearfvgcolor        = input.color(color.new(color.blue,80),"Bear OB Fill Color",group = tfcstrg,inline="0")
boxright = true
OBwick_extend = false

// FVG Box Border Inputs
box_border_bull_color   = (color.new(color.yellow,100))
box_border_bear_color   = (color.new(color.blue,100))
box_border_width        = 1
bbstyle_string          = "Solid"
box_border_style        = line.style_dotted
//show_mts =   input.bool(true,'Show OB Thresholds',group = 'mk test',inline="1")

// --------------------------------------------------------------- //
// |    FUNCTIONS|                                                 //
// --------------------------------------------------------------- //

// simple strings required for request.security function in latter main code calls to handle_all function
_gettf_interval_str(simple int index) =>
    string result = switch index
        0 => "5"
        1 => "10"
        2 => "15"
        3 => "30"
        4 => "60"
        5 => "240"
        6 => "480"
        7 => "720"
        8 => "D"
        9 => "W"
        10 => "M"
        => "Unsupported Timeframe"

// simple strings require for label displays used in add_label ultimately
_gettf_label_str(simple int index) =>
    string result = switch index
        0 => "5 Min"
        1 => "10 Min"
        2 => "15 Min"
        3 => "30 Min"
        4 => "1 Hr"
        5 => "4 Hr"
        6 => "8 Hr"
        7 => "12Hr"
        8 => "Daily"
        9 => "Weekly"
        10 => "Monthly"
        => "Unsupported Timeframe"

// used if user selected use chart timeframe option to see not double work if chart is set to same timeframe as another enabled timeframe for fvg creation        
_not_current_timeframe_equal_enabled_tfs(_current_timeframe) =>
    result = switch _current_timeframe
        "5"     => not enable_5min
        "10"    => not enable_10min
        "15"    => not enable_15min
        "30"    => not enable_30min
        "60"    => not enable_1hr
        "240"   => not enable_4hr
        "480"   => not enable_8hr
        "720"   => not enable_12hr
        "D"     => not enable_Daily
        "W"     => not enable_Week
        "M"     => not enable_Month
        => true

// ************* SEE IF NEW BULL FVGs DETECTED   
_isfvgbull(_fvgmethod,_open1,_close1,_open,_close,_high1,_low1) =>
    if display
        if _fvgmethod  // if body fvg detection
            _open1 > _close1 and _open < _close and _close > _high1
        else   // else wick detection
            _open < _high1 
    else
        false

// ************* SEE IF NEW BEAR FVGs DETECTED     
_isfvgbear(_fvgmethod,_open1,_close1,_open,_close,_high1,_low1) =>
    if display
        if _fvgmethod
            _open1 < _close1 and _open > _close and _close < _low1
        else
            _open < _low1
    else
        false

// *************** DELETE A BOX AND ASSOCIATED LABEL        
_box_n_label_delete(_boxarray,_i,_labelarray,_labelflag) =>
    bptr = array.get(_boxarray,_i)
    box.delete(bptr)
    array.remove(_boxarray, _i)
    if _labelflag
        lptr = array.get(_labelarray,_i)
        label.delete(lptr)
        array.remove(_labelarray,_i)

// *************** GET A FVG BOX TOP AND BOTTOM VALUES
_getboxelements(_boxptr,_i) =>
    _boxelement = array.get(_boxptr,_i)
    _left       = box.get_left(_boxelement)
    _top        = box.get_top(_boxelement)
    _btm        = box.get_bottom(_boxelement)
    _right      = box.get_right(_boxelement)
    [_boxelement,_left,_top,_btm,_right]
    
// ************** GET LEFT SIDE START BAR INDEX OF EXISTING BOX, for use in preventing duplicates for higher timeframes
_getboxstart(_boxptr,_i) =>
    _boxelement = array.get(_boxptr,_i)

// *************** ADD A LABEL TO AN FVG    
_addlabel(_fvglabelsarray,_bar_index,_high,_string,_style,y,_show_label) =>
    var string s = na
    if _show_label
        if show_timeonlabels
            s := _string + str.format("  {0,date,HH:mm MM/dd/yy}", time + msOffsetInput)
        else
            s := _string
        blabel = label.new(_bar_index,_high,text=s,color=color.rgb(0,0,0,100),style = _style) //_string,color=color.rgb(0,0,0,100),style = _style)
        label.set_textcolor(blabel,labelcolor)//color.rgb(label_red,label_grn,label_blu,label_trans))
        label.set_xy(blabel,_bar_index + label_shift,y)
        label.set_size(blabel,size.normal)
        array.push(_fvglabelsarray,blabel)

// added these two globally as can't use them in functions with consistency/reliability towards incursion alerts       
lasthigh    =   high[1]    
lastlow     =   low[1]

// compare existing bull fvg box values to current price action
_getbullfvgaction(_top,_bottom,_intrusion_percentage) =>
    midpt = (_top + _bottom) / 2
    threshold   = _top - (_intrusion_percentage * (_top - _bottom))
    _have_intrusion = low < threshold and lastlow > threshold    
    _lowundertop    = low < _top
    _lowunderbtm    = low < _bottom
    _lowundermid    = low < midpt
    _closeundertop  = close < _top
    _closeunderbtm  = close < _bottom
    _closeundermid  = low < midpt
    [_lowundertop,_lowunderbtm,_closeundertop,_closeunderbtm,_lowundermid,_closeundermid,_have_intrusion]//,_closecrosswithin]

// compare existing bear fvg box values to current price action   
_getbearfvgaction(_top,_bottom,_intrusion_percentage) =>
    midpt = (_top + _bottom) / 2
    threshold   = _bottom + (_intrusion_percentage * (_top - _bottom))
    _have_intrusion = high > threshold and lasthigh < threshold    
    _highovertop    = high > _top
    _highoverbtm    = high > _bottom
    _highovermid    = high > midpt
    _closeovertop   = close > _top
    _closeoverbtm   = close > _bottom
    _closeovermid   = close > midpt
    [_highovertop,_highoverbtm,_closeovertop,_closeoverbtm,_highovermid,_closeovermid,_have_intrusion]

// used for adjusting fvg label position with each new bar
_setlabelxy(_fvglabels,_i,_barindex,_top,_bottom) =>
    _lptr = array.get(_fvglabels,_i)
    label.set_xy(_lptr,_barindex + label_shift,(_top + _bottom) / 2)

// used for adjusting bull box position with each new bar
_setboxleftbull(_boxbull,_i,_barindex) =>
    _bxbull = box.get_left(_boxbull)
    box.set_left(_boxbull,last_bar_index + label_shift)

// used for adjusting bear box position with each new bar
_setboxleftbear(_boxbear,_i,_barindex) =>
    _bxbear = box.get_left(_boxbear)
    box.set_left(_boxbear,last_bar_index + label_shift)

// get higher timeframe price values
_gethighlowcloseopens(_ticker,_period,_high,_high2,_low,_low2,_close1,_open1) =>
    request.security(_ticker,_period,[_high,_high2,_low,_low2,_close1,_open1])      

// modify existing bull fvg boxes for incursion, deletion etc.
_update_bull_fvgs(_boxbull,_labelsbull,_timestring) =>
    for i = array.size(_boxbull) - 1 to 0 by 1
        [fvgboxbull,left,top,bottom,right] = _getboxelements(_boxbull,i)
        [lowundertop,lowunderbtm,closeundertop,closeunderbtm,lowundermid,closeundermid,intrusion] = _getbullfvgaction(top,bottom,intrusion_percentage)

        // INCURSION ALERT DONE HERE: if mitigation action is normal or none then do alert
        if (mitigationaction == 1 or mitigationaction == 3) and intrusion and incursion_alerts
            alert("Bull OB Wick Incursion " + str.tostring(_timestring), alert.freq_once_per_bar)

        if entrychangecolor
            if lowundertop  // low < top   
                box.set_bgcolor(fvgboxbull,entry_bull_color)

        if show_labels     
            _setlabelxy(_labelsbull,i,bar_index,top,bottom)
            _setboxleftbull(fvgboxbull,i,bar_index)
        
        // *** if mitigation is set to dynamic and body is referenced with price and price is within fvg
        if mitigationaction == 2 and UseBodyForMitigation and closeundertop 
            box.set_top(fvgboxbull,close)
            if show_labels
                _setlabelxy(_labelsbull,i,bar_index,close,bottom)                
        else
        // else if mitigation set to dynamic and body mitigation NOT enabled and btm wick goes below top of fvg band then move band top lower            
            if mitigationaction == 2 and lowundertop 
                box.set_top(fvgboxbull,low)
                if show_labels
                    _setlabelxy(_labelsbull,i,bar_index,low,bottom)
        
        // section to delete boxes when fvg filled
        if mitigationaction == 3 // if 'none' selected for mitigation
            if UseBodyForMitigation and closeunderbtm
                box.set_bgcolor(fvgboxbull,color.new(nomiticolor,nmc_trans))
            else
                if lowunderbtm 
                    box.set_bgcolor(fvgboxbull,color.new(nomiticolor,nmc_trans)) 
                    
            if (UseBodyForMitigation and closeunderbtm) or lowunderbtm
                if show_labels and show_mitigated_text
                    larray = array.get(_labelsbull,i)
                    curr_text = label.get_text(larray)
                    found = str.contains(curr_text,"Mitigated")                    
                    if not found
                        label.set_text(larray,curr_text + " Mitigated")                     
        
        if mitigationaction == 1 or mitigationaction == 2
            if UseBodyForMitigation and closeunderbtm
                _box_n_label_delete(_boxbull, i,_labelsbull, show_labels)            
            else
                if lowunderbtm 
                    _box_n_label_delete(_boxbull, i,_labelsbull, show_labels) 
                    
        if mitigationaction == 4 // if half mitigation
            if UseBodyForMitigation and closeundermid
                _box_n_label_delete(_boxbull, i,_labelsbull, show_labels)             
            else
                if lowundermid 
                    _box_n_label_delete(_boxbull, i,_labelsbull, show_labels) 

// modify existing bear fvg boxes for incursion, deletion etc.
_update_bear_fvgs(_boxbear,_labelsbear,_timestring) =>                
    for i = array.size(_boxbear) - 1 to 0 by 1
        [fvgboxbear,left,top,bottom,right] = _getboxelements(_boxbear,i)
        [highovertop,highoverbtm,closeovertop,closeoverbtm,highovermid,closeovermid,intrusion] = _getbearfvgaction(top,bottom,intrusion_percentage)         

        // INCURSION ALERT DONE HERE: if mitigation action is normal or none then do alert
        if (mitigationaction == 1 or mitigationaction == 3) and intrusion and incursion_alerts
            alert("Bear OB Wick Incursion " + str.tostring(_timestring), alert.freq_once_per_bar)

        if entrychangecolor
            if highoverbtm   
                box.set_bgcolor(fvgboxbear,entry_bear_color)//color.new(bearfvgcolor,_bear_trans - (_bear_trans / 4)))
            else
                box.set_bgcolor(fvgboxbear,bearfvgcolor)//color.new(bearfvgcolor,_bear_trans)) 

        if show_labels 
            _setlabelxy(_labelsbear,i,bar_index,top,bottom)
            _setboxleftbear(fvgboxbear,i,bar_index)             

        // *** if mitigation is set to dynamic and body is referenced with price and price is within fvg
        if mitigationaction == 2 and UseBodyForMitigation and closeoverbtm  
            box.set_bottom(fvgboxbear,close) 
            if show_labels
                _setlabelxy(_labelsbear,i,bar_index,close,bottom)                  
        else
        // else if mitigation set to dynamic and body mitigation NOT enabled and top wick goes above bottom of fvg band then move band bottom up            
            if mitigationaction == 2 and highoverbtm 
                box.set_bottom(fvgboxbear,high)
                if show_labels
                    _setlabelxy(_labelsbear,i,bar_index,top,high)  
                    
        if mitigationaction == 3 // if 'none' selected for mitigation
            if UseBodyForMitigation and closeovertop
                box.set_bgcolor(fvgboxbear,color.new(nomiticolor,nmc_trans))
            else
                if highovertop
                    box.set_bgcolor(fvgboxbear,color.new(nomiticolor,nmc_trans))  
            
            if (UseBodyForMitigation and closeovertop) or highovertop
                if show_labels and show_mitigated_text
                    larray = array.get(_labelsbear,i)
                    curr_text = label.get_text(larray)
                    found = str.contains(curr_text,"Mitigated")                    
                    if not found
                        label.set_text(larray,curr_text + " Mitigated")                    
        
        if mitigationaction == 1 or mitigationaction == 2
            if UseBodyForMitigation and closeovertop 
                _box_n_label_delete(_boxbear, i,_labelsbear, show_labels)              
            else
                if highovertop 
                    _box_n_label_delete(_boxbear, i,_labelsbear, show_labels) 

        if mitigationaction == 4 // if half mitigation
            if UseBodyForMitigation and closeovermid
                _box_n_label_delete(_boxbear, i,_labelsbear, show_labels)             
            else
                if highovermid 
                    _box_n_label_delete(_boxbear, i,_labelsbear, show_labels)                 

// check for duplicate boxes required for eliminating duplicate higher timeframe fvg boxes, input 1: pointer to box array, input 2: bar_index which is left side of box in bar_index number
_duplicate_box(_boxarray,_top,_bottom) =>
    found = false
    if array.size(_boxarray) > 0
        for i = array.size(_boxarray) - 1 to 0 by 1       
            [_barray,left,top,bottom,right] = _getboxelements(_boxarray,i) 
            if _top == top //and _bottom == bottom  //_left_bar_index == left
                found := true
                break
    found
    
// main function call here - detects and calls to create fvg boxes then calls the update fvg functions for bulls and bears fvgs for already existing fvg boxes
_handle_all(_tstring,_bullfvgs,_bearfvgs,_bulllabels,_bearlabels,_maxarraysize,_open1,_close1,_open,_close,_high1,_low1)  =>
    var bool _newBull = false
    var bool _newBear = false
    if OnlyMktHrs and inSession or not OnlyMktHrs
        _newBull := _isfvgbull(fvgmethod,_open1,_close1,_open,_close,_high1,_low1) 
        _newBear := _isfvgbear(fvgmethod,_open1,_close1,_open,_close,_high1,_low1) 
        //_fvgmethod,_open1,_close1,_open,_close,_high1,_low1 - MIGHT HAVE to take _ out before FVG method
    
    if _newBull
        if array.size(_bullfvgs) > _maxarraysize
            _box_n_label_delete(_bullfvgs, 0,_bulllabels, show_labels)   

        if not _duplicate_box(_bullfvgs,_high1,_open1)  // inputs top, bottom of box
            array.push(_bullfvgs, box.new(left=last_bar_index + 20, bottom=_open1, right=last_bar_index + 200, top=_high1, bgcolor=bullfvgcolor, border_color=box_border_bull_color, border_width = box_border_width, border_style = box_border_style, extend=extend.right))
            //if OBwick_extend
                //box.set_right(_bullfvgs, bar_index)
            if show_labels
                _addlabel(_bulllabels,bar_index,_high1,_tstring + " OB BULL",label.style_label_left,(_high1[1] + _low1) / 2, show_labels)

    if _newBear
        if array.size(_bearfvgs) > _maxarraysize
            _box_n_label_delete(_bearfvgs, 0,_bearlabels, show_labels)          
    
        if not _duplicate_box(_bearfvgs,_open1,_low1)  // inputs top, bottom of box
            array.push(_bearfvgs, box.new(left=last_bar_index + 20, top=_open1, right=last_bar_index + 200, bottom=_low1, bgcolor=bearfvgcolor, border_color=box_border_bear_color, border_width = box_border_width, border_style = box_border_style, extend=extend.right))
            //if OBwick_extend
                //box.set_right(_bearfvgs, bar_index)
            if show_labels
                _addlabel(_bearlabels,bar_index,_high1,_tstring + " OB BEAR",label.style_label_left,(_high1[1] + _low1) / 2, show_labels)

    //|       BULL FVG HANDLING AREA         | //
    // ----------------------------------------------- //
    if array.size(_bullfvgs) > 0  // this code is to handle incursion alerts before FVG's possibly being mitigated
        _update_bull_fvgs(_bullfvgs,_bulllabels,_tstring) 
    
    // ----------------------------------------------- //
    // |        BEAR FVG HANDLING AREA         | //
    // ----------------------------------------------- //
    if array.size(_bearfvgs) > 0  // this code is to handle incursion alerts before FVG's possibly being mitigated
        _update_bear_fvgs(_bearfvgs,_bearlabels,_tstring)                 

    [_newBull,_newBear]
// ***************************************************
// Array and Box definition area
// ***************************************************
var box[]   fvgboxBull      = array.new_box()
var box[]   fvgboxBear      = array.new_box()
var         fvglabelsBull   = array.new_label()
var         fvglabelsBear   = array.new_label()
var bool    newboxBull      = false
var bool    newboxBear      = false

var box[]   fvgboxBull5     = array.new_box()
var box[]   fvgboxBear5     = array.new_box()
var         fvglabelsBull5  = array.new_label()
var         fvglabelsBear5  = array.new_label()
var bool    newboxBull5     = false
var bool    newboxBear5     = false

var box[]   fvgboxBull10    = array.new_box()
var box[]   fvgboxBear10    = array.new_box()
var         fvglabelsBull10 = array.new_label()
var         fvglabelsBear10 = array.new_label()
var bool    newboxBull10    = false
var bool    newboxBear10    = false

var box[]   fvgboxBull15    = array.new_box()
var box[]   fvgboxBear15    = array.new_box()
var         fvglabelsBull15 = array.new_label()
var         fvglabelsBear15 = array.new_label()
var bool    newboxBull15    = false
var bool    newboxBear15    = false

var box[]   fvgboxBull30    = array.new_box()
var box[]   fvgboxBear30    = array.new_box()
var         fvglabelsBull30 = array.new_label()
var         fvglabelsBear30 = array.new_label()
var bool    newboxBull30    = false
var bool    newboxBear30    = false

var box[]   fvgboxBull1hr   = array.new_box()
var box[]   fvgboxBear1hr   = array.new_box()
var         fvglabelsBull1hr= array.new_label()
var         fvglabelsBear1hr= array.new_label()
var bool    newboxBull1hr   = false
var bool    newboxBear1hr   = false

var box[]   fvgboxBull4hr   = array.new_box()
var box[]   fvgboxBear4hr   = array.new_box()
var         fvglabelsBull4hr= array.new_label()
var         fvglabelsBear4hr= array.new_label()
var bool    newboxBull4hr   = false
var bool    newboxBear4hr   = false

var box[]   fvgboxBull8hr   = array.new_box()
var box[]   fvgboxBear8hr   = array.new_box()
var bool    newboxBull8hr   = false
var bool    newboxBear8hr   = false
var         fvglabelsBull8hr= array.new_label()
var         fvglabelsBear8hr= array.new_label()

var box[]   fvgboxBull12hr   = array.new_box()
var box[]   fvgboxBear12hr   = array.new_box()
var bool    newboxBull12hr   = false
var bool    newboxBear12hr   = false
var         fvglabelsBull12hr= array.new_label()
var         fvglabelsBear12hr= array.new_label()

var box[]   fvgboxBullDaily = array.new_box()
var box[]   fvgboxBearDaily = array.new_box()
var         fvglabelsBullDaily = array.new_label()
var         fvglabelsBearDaily = array.new_label()
var bool    newboxBullDaily = false
var bool    newboxBearDaily = false

var box[]   fvgboxBullWeekly = array.new_box()
var box[]   fvgboxBearWeekly = array.new_box()
var         fvglabelsBullWeekly = array.new_label()
var         fvglabelsBearWeekly = array.new_label()
var bool    newboxBullWeekly = false
var bool    newboxBearWeekly = false

var box[]   fvgboxBullMonthly = array.new_box()
var box[]   fvgboxBearMonthly = array.new_box()
var         fvglabelsBullMonthly = array.new_label()
var         fvglabelsBearMonthly = array.new_label()
var bool    newboxBullMonthly = false
var bool    newboxBearMonthly = false


///////////////////////////////////////
//|        MAIN CODE                  |
///////////////////////////////////////
if enable_current_timeframe and _not_current_timeframe_equal_enabled_tfs(current_timeframe)
    [_open1,_close1,_open,_close,_high1,_low1] = _gethighlowcloseopens(syminfo.tickerid,current_timeframe,open[1],close[1],open[0],close[0],high[1],low[1])
    [tnewboxBull,tnewboxBear] = _handle_all(currtfstring,fvgboxBull,fvgboxBear,fvglabelsBull,fvglabelsBear,curr_MaxArraySize,_open1,_close1,_open,_close,_high1,_low1)
    newboxBull := tnewboxBull
    newboxBear := tnewboxBear
if enable_5min
    [_open1,_close1,_open,_close,_high1,_low1] = _gethighlowcloseopens(syminfo.tickerid,_gettf_interval_str(0),open[1],close[1],open[0],close[0],high[1],low[1])
    [tnewboxBull5,tnewboxBear5] =_handle_all(_gettf_label_str(0),fvgboxBull5,fvgboxBear5,fvglabelsBull5,fvglabelsBear5,MaxArraySize_5min,_open1,_close1,_open,_close,_high1,_low1)
    newboxBull5 := tnewboxBull5
    newboxBear5 := tnewboxBear5
if enable_10min
    [_open1,_close1,_open,_close,_high1,_low1] = _gethighlowcloseopens(syminfo.tickerid,_gettf_interval_str(1),open[1],close[1],open[0],close[0],high[1],low[1])
    [tnewboxBull10,tnewboxBear10] = _handle_all(_gettf_label_str(1),fvgboxBull10,fvgboxBear10,fvglabelsBull10,fvglabelsBear10,MaxArraySize_10min,_open1,_close1,_open,_close,_high1,_low1)
    newboxBull10 := tnewboxBull10
    newboxBear10 := tnewboxBear10
if enable_15min
    [_open1,_close1,_open,_close,_high1,_low1] = _gethighlowcloseopens(syminfo.tickerid,_gettf_interval_str(2),open[1],close[1],open[0],close[0],high[1],low[1])
    [tnewboxBull15,tnewboxBear15] = _handle_all(_gettf_label_str(2),fvgboxBull15,fvgboxBear15,fvglabelsBull15,fvglabelsBear15,MaxArraySize_15min,_open1,_close1,_open,_close,_high1,_low1)
    newboxBull15 := tnewboxBull15
    newboxBear15 := tnewboxBear15
if enable_30min
    [_open1,_close1,_open,_close,_high1,_low1] = _gethighlowcloseopens(syminfo.tickerid,_gettf_interval_str(3),open[1],close[1],open[0],close[0],high[1],low[1])
    [tnewboxBull30,tnewboxBear30] = _handle_all(_gettf_label_str(3),fvgboxBull30,fvgboxBear30,fvglabelsBull30,fvglabelsBear30,MaxArraySize_30min,_open1,_close1,_open,_close,_high1,_low1)
    newboxBull30 := tnewboxBull30
    newboxBear30 := tnewboxBear30
if enable_1hr
    [_open1,_close1,_open,_close,_high1,_low1] = _gethighlowcloseopens(syminfo.tickerid,_gettf_interval_str(4),open[1],close[1],open[0],close[0],high[1],low[1])
    [tnewboxBull1hr,tnewboxBear1hr] = _handle_all(_gettf_label_str(4),fvgboxBull1hr,fvgboxBear1hr,fvglabelsBull1hr,fvglabelsBear1hr,MaxArraySize_1hr,_open1,_close1,_open,_close,_high1,_low1)
    newboxBull1hr := tnewboxBull1hr
    newboxBear1hr := tnewboxBear1hr
if enable_4hr
    [_open1,_close1,_open,_close,_high1,_low1] = _gethighlowcloseopens(syminfo.tickerid,_gettf_interval_str(5),open[1],close[1],open[0],close[0],high[1],low[1])
    [tnewboxBull4hr,tnewboxBear4hr] = _handle_all(_gettf_label_str(5),fvgboxBull4hr,fvgboxBear4hr,fvglabelsBull4hr,fvglabelsBear4hr,MaxArraySize_4hr,_open1,_close1,_open,_close,_high1,_low1)       
    newboxBull4hr := tnewboxBull4hr
    newboxBear4hr := tnewboxBear4hr
if enable_8hr
    [_open1,_close1,_open,_close,_high1,_low1] = _gethighlowcloseopens(syminfo.tickerid,_gettf_interval_str(6),open[1],close[1],open[0],close[0],high[1],low[1])
    [tnewboxBull8hr,tnewboxBear8hr] = _handle_all(_gettf_label_str(6),fvgboxBull8hr,fvgboxBear8hr,fvglabelsBull8hr,fvglabelsBear8hr,MaxArraySize_8hr,_open1,_close1,_open,_close,_high1,_low1)
    newboxBull8hr := tnewboxBull8hr
    newboxBear8hr := tnewboxBear8hr
if enable_12hr
    [_open1,_close1,_open,_close,_high1,_low1] = _gethighlowcloseopens(syminfo.tickerid,_gettf_interval_str(7),open[1],close[1],open[0],close[0],high[1],low[1])
    [tnewboxBull12hr,tnewboxBear12hr] = _handle_all(_gettf_label_str(7),fvgboxBull12hr,fvgboxBear12hr,fvglabelsBull12hr,fvglabelsBear12hr,MaxArraySize_12hr,_open1,_close1,_open,_close,_high1,_low1)
    newboxBull12hr := tnewboxBull12hr
    newboxBear12hr := tnewboxBear12hr
if enable_Daily
    [_open1,_close1,_open,_close,_high1,_low1] = _gethighlowcloseopens(syminfo.tickerid,_gettf_interval_str(8),open[1],close[1],open[0],close[0],high[1],low[1])
    [tnewboxBullDaily,tnewboxBearDaily] = _handle_all(_gettf_label_str(8),fvgboxBullDaily,fvgboxBearDaily,fvglabelsBullDaily,fvglabelsBearDaily,MaxArraySize_Daily,_open1,_close1,_open,_close,_high1,_low1)
    newboxBullDaily := tnewboxBullDaily
    newboxBearDaily := tnewboxBearDaily
if enable_Week
    [_open1,_close1,_open,_close,_high1,_low1] = _gethighlowcloseopens(syminfo.tickerid,_gettf_interval_str(9),open[1],close[1],open[0],close[0],high[1],low[1])
    [tnewboxBullWeekly,tnewboxBearWeekly] = _handle_all(_gettf_label_str(9),fvgboxBullWeekly,fvgboxBearWeekly,fvglabelsBullWeekly,fvglabelsBearWeekly,MaxArraySize_Wkly,_open1,_close1,_open,_close,_high1,_low1)    
    newboxBullWeekly := tnewboxBullWeekly
    newboxBearWeekly := tnewboxBearWeekly
if enable_Month
    [_open1,_close1,_open,_close,_high1,_low1] = _gethighlowcloseopens(syminfo.tickerid,_gettf_interval_str(10),open[1],close[1],open[0],close[0],high[1],low[1])
    [tnewboxBullMonthly,tnewboxBearMonthly] = _handle_all(_gettf_label_str(10),fvgboxBullMonthly,fvgboxBearMonthly,fvglabelsBullMonthly,fvglabelsBearMonthly,MaxArraySize_Mthly,_open1,_close1,_open,_close,_high1,_low1)
    newboxBullMonthly := tnewboxBullMonthly
    newboxBearMonthly := tnewboxBearMonthly
// // ------------------------------------------------------- //
// // |            STANDARD CONDITIONAL ALERT SECTION         | //
// // ------------------------------------------------------- // 

bull_fvg_creation_alert = newboxBull or newboxBull5 or newboxBull10 or newboxBull15 or newboxBull1hr or newboxBull4hr or newboxBull8hr or newboxBullDaily or newboxBullWeekly or newboxBullMonthly
alertcondition(bull_fvg_creation_alert,title='Bull OB Creation',message = 'Bull OB Creation' )
bear_fvg_creation_alert = newboxBear or newboxBear5 or newboxBear10 or newboxBear15 or newboxBear1hr or newboxBear4hr or newboxBear8hr or newboxBearDaily or newboxBearWeekly or newboxBearMonthly          
alertcondition(bear_fvg_creation_alert,title='Bear OB Creation',message = 'Bear OB Creation' )
both_fvg_creation_alert = newboxBull or newboxBull5 or newboxBull10 or newboxBull15 or newboxBull1hr or newboxBull4hr or newboxBull8hr or newboxBullDaily or newboxBullWeekly or newboxBullMonthly or newboxBear or newboxBear5 or newboxBear10 or newboxBear15 or newboxBear1hr or newboxBear4hr or newboxBear8hr or newboxBearDaily or newboxBearWeekly or newboxBearMonthly        
alertcondition(both_fvg_creation_alert,title='Bull or Bear OB Creation',message = 'Bull or Bear OB Creation' )






