// Description:
// The ICT Algorithmic Macro Tracker° Indicator is a powerful tool designed to enhance your trading experience by clearly and efficiently plotting the known ICT Macro Times on your chart.
// Based on the teachings of the Inner Circle Trader, these Time windows correspond to periods when the Interbank Price Delivery Algorithm undergoes a series of checks (Macros) and is probable to move towards Liquidity.
// The indicator allows traders to visualize and analyze these crucial moments in NY Time:
// - 2:33-3:00
// - 4:03-4:30
// - 8:50-9:10
// - 9:50-10:10
// - 10:50-11:10
// - 11:50-12:10
// - 13:10-13:50
// - 15:15-15:45
// By providing a clean and clutter-free representation of ICT Macros, this indicator empowers traders to make more informed decisions, optimize and build their strategies based on Time.
// Massive shoutout to @reastruth for his ICT Macros Indicator, and for allowing to create one of my own, go check him out!
// Indicator Features:
// – Track ongoing ICT Macros to aid your Live analysis.
// - Gain valuable insights by hovering over the plotted ICT Macros to reveal tooltips with interval information.
// – Plot the ICT Macros in one of two ways:
// "On Chart": visualize ICT Macro timeframes directly on your chart, with automatic adjustments as Price moves.
// Pro Tip: toggle Projections to see exactly where Macros begin and end without difficulty.
// "New Pane": move the indicator two a New Pane to see both Live and Upcoming Macro events with ease in a dedicated section
// Pro Tip: this section can be collapsed by double-clicking on the main chart, allowing for seamless trading preparation.


//@version=5
indicator( "ICT Algorithmic Macro Tracker° (Open-Source) by toodegrees"
		 , shorttitle = "Macro Tracker°"
		 , overlay    = true)


//#region[GLOBAL]
var line[]  EXT = array.new_line()
var label[] LBL = array.new_label()
nMacros         = 8                                              // HEY! This is Step 1.5 for INSTRUCTIONS TO ADD CUSTOM MACROs - Start from the beginning at line 192 :)
oneDayMS        = 86400000
oneBarMS        = time_close-time
noColor         =color.new(#ffffff,100)
one             = ta.highest(timeframe.in_seconds("15")/timeframe.in_seconds(timeframe.period))+syminfo.mintick*10
y_btm_Line1     = one
y_top_Line1     = one+syminfo.mintick*5
//#endregion


//#region[INPUTS]
_macroC =  input.color(color.new(#000000,0),  title="Macro Color",        inline='main')
_mode   = input.string("On Chart",              title="",                   inline='main', options=["On Chart", "New Pane"]
					                                 							         , tooltip='When "New Pane" is selected, make sure you move the indicator to a New Pane as well, it is not automatic.')

_showL  =   input.bool(true,                    title="Macro Label?",       inline='sh')
_mTxt   =   input.bool(false,                   title="Show Time?",         inline='sh')
_extt   =	input.bool(false,                   title="Macro Projections?", inline='sh'
					  													  , tooltip='Extends On Chart Macro lines towards price.')

_bgln   =  input.color(color.new(#9c27b0,70), title="London Macro",       inline='bc')
_bgny   =  input.color(color.new(#4caf50,70), title="New York Macro",     inline='bc'
					  												  ,     tooltip='Visible only in "New Pane" mode')
//#endregion


//#region[FUNCTIONS]
time_isnewbarNY(string sess) =>
    t = time("1", sess, "America/New_York")
    na(t[1]) and not na(t) or t[1] < t

_controlMacroLine(line[] _lines, label[] _lbl, bool _time) =>
	if _time
		_lbl.last().set_x(math.round(math.avg(_lines.get(_lines.size()-2).get_x1(),time)))
		if high>_lines.last().get_y2()-syminfo.mintick*10
			_lines.get(_lines.size()-2).set_y2(high+(syminfo.mintick*10))
			_lines.last().set_y1(high+(syminfo.mintick*10))
			_lines.last().set_y2(high+(syminfo.mintick*10))
			LBL.last().set_y(high+(syminfo.mintick*10))
		if na or _lines.last().get_x2() == time
			_lines.last().set_x2(time+oneBarMS)

_controlMacroBox(box[] _boxes, bool _time, bool _friday) =>
	dly = _friday?oneDayMS*3:oneDayMS
	if na or _time
		_boxes.last().set_right(time+dly)

method memoryCleanLine(line[] A) =>
	if A.size()>0
		for i=0 to A.size()-1
			_line = A.get(i)
			if _line.get_x1()<time-oneDayMS
				line.delete(_line)
method memoryCleanLabel(label[] A, int len) =>
	if A.size()>0
		for i=0 to A.size()-1
			_line = A.get(i)
			if _line.get_x()<time-oneDayMS
				label.delete(_line)

macroOC(line[] LINES, bool _time, string _kzTime, bool _friday, string _type)=>
	dly  = _friday?oneDayMS*3:oneDayMS
	_col = _type=='ny'?_bgny:_bgln
	_txt = _mTxt?"MACRO"+"\n"+_kzTime:"MACRO"
	_tt  = _type=='ny'?"NY MACRO\n"+_kzTime:(_type=='ln'?"LN MACRO\n"+_kzTime:"myMACRO\n"+_kzTime)

	// Overlay False
	if not _time[1] and _time
		_vline1 = line.new(time,y_btm_Line1,time,y_top_Line1,xloc=xloc.bar_time,color=_macroC)
		LINES.push(_vline1)
		_hline = line.new(time,y_top_Line1,time+oneBarMS,y_top_Line1,xloc=xloc.bar_time,color=_macroC)
		LINES.push(_hline)
		if _extt
			EXT.push(line.new(time,high,time,_vline1.get_y2(),xloc=xloc.bar_time,color=_macroC,style=line.style_dotted))

		if _mode=="On Chart"
			LBL.push(label.new(time
							 , LINES.get(LINES.size()-2).get_y2()
							 , _showL?_txt:""
							 , tooltip=_tt
							 , xloc=xloc.bar_time
							 , style=label.style_label_down
							 , color=noColor
							 , textcolor=_macroC
							 , size=size.small))

	if _time[1] and not _time and LINES.size()>0 and LBL.size()>0
		_vline2 = line.new(time,y_btm_Line1,time,y_top_Line1,xloc=xloc.bar_time,color=_macroC)
		LBL.last().set_x(math.round(math.avg(LINES.get(LINES.size()-2).get_x1(),time)))
		if y_top_Line1 > LINES.get(LINES.size()-2).get_y2()
			LINES.get(LINES.size()-2).set_y2(y_top_Line1)
			LINES.last().set_y1(y_top_Line1)
			LINES.last().set_y2(y_top_Line1)
			LBL.last().set_y(y_top_Line1)
		else if y_top_Line1 < LINES.get(LINES.size()-2).get_y2()
			_vline2.set_y2(LINES.get(LINES.size()-2).get_y2())
		if _extt
			EXT.push(line.new(time,high,time,_vline2.get_y2(),xloc=xloc.bar_time,color=_macroC,style=line.style_dotted))

		LINES.push(_vline2)

	if LINES.size()>0 and LBL.size()>0
		_controlMacroLine(LINES, LBL, _time)
		

macroNP(box[] BOXES, bool _time, string _kzTime, bool _friday, string _type)=>
	dly  = _friday?86400000*3:86400000
	_col = _type=='ny'?_bgny:_bgln
	_tt  = _type=='ny'?"NY MACRO\n"+_kzTime:"LN MACRO\n"+_kzTime
	end  = not _time and _time[1]

	// Overlay False
	var _plotNP = false
	if not _time[1] and _time
		_macro = box.new(time+dly,2,time+dly,0,_macroC,xloc=xloc.bar_time,bgcolor=_col)
		BOXES.push(_macro)
		_plotNP := true

	if _time[1] and not _time and BOXES.size()>0
		_controlMacroBox(BOXES, true, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto')
		_plotNP := false
		macroL = label.new(math.round(math.avg(BOXES.last().get_left(),BOXES.last().get_right()))
					     , 1
						 , xloc=xloc.bar_time
						 , style=label.style_label_center
						 , color=noColor
						 , tooltip=_tt
						 , text='ⓘ\n\n\n\n\n\n'
						 , textcolor=_macroC
						 , size=size.small)

	if _time and _plotNP
		_controlMacroBox(BOXES, _time, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto')
//#endregion


//#region[LOGIC]
// LONDON
var line[] _LINES_LN_1 = array.new_line()
var box[] _BOXES_LN_1  = array.new_box()
timeLN1                = time_isnewbarNY("0233-0300:1234567")
lblLN1                 = "2:33 - 3:00"

var line[] _LINES_LN_2 = array.new_line()
var box[] _BOXES_LN_2  = array.new_box()
timeLN2                = time_isnewbarNY("0403-0430:1234567")
lblLN2                 = "4:03 - 4:30"

// NEW YORK
var line[] _LINES_NY_1 = array.new_line()
var box[] _BOXES_NY_1  = array.new_box()
timeNY1                = time_isnewbarNY("0850-0910:1234567")
lblNY1                 = "8:50 - 9:10"

var line[] _LINES_NY_2 = array.new_line()
var box[] _BOXES_NY_2  = array.new_box()
timeNY2                = time_isnewbarNY("0950-1010:1234567")
lblNY2                 = "9:50 - 10:10"

var line[] _LINES_NY_3 = array.new_line()
var box[] _BOXES_NY_3  = array.new_box()
timeNY3                = time_isnewbarNY("1050-1110:1234567")
lblNY3                 = "10:50 - 11:10"

var line[] _LINES_NY_4 = array.new_line()
var box[] _BOXES_NY_4  = array.new_box()
timeNY4                = time_isnewbarNY("1150-1210:1234567")
lblNY4                 = "11:50 - 12:10"

var line[] _LINES_NY_5 = array.new_line()
var box[] _BOXES_NY_5  = array.new_box()
timeNY5                = time_isnewbarNY("1310-1340:1234567")
lblNY5                 = "13:10 - 13:40"

var line[] _LINES_NY_6 = array.new_line()
var box[] _BOXES_NY_6  = array.new_box()
timeNY6                = time_isnewbarNY("1515-1545:1234567")
lblNY6                 = "15:15 - 15:45"

// 																		    //\//////////////////////////////////////////////////////////////////////////////////////////////////////////////\///
//                                                                          //  INSTRUCTIONS TO ADD CUSTOM MACROs (PART 1):                                                                    //
//                                                                          //  1.1) Copy these four lines of code and add them below the instruction box -> // CUSTOM MACROS;                 //
// var line[] _lines_YourMacroName = array.new_line()                       //       Make sure the text is not gray like these instructions. To do that, remove the '//' in front.             //
// var box[] _boxes_YourMacroName  = array.new_box()                        //  1.2) Replace 'YourMacroName' with whatever you want in these four lines.                                       //
// _time_YourMacroName             = time_isnewbarNY("hhmm-hhmm:1234567")   //  1.3) Add your macro time window in the format: "hhmm-hhmm:1234567"                                             //
// _lbl_YourMacroName              = "15:15 - 15:45"                        //  1.4) Add your macro time text in _lbl_YourMacroName, this will be displayed in the tooltips,                   //
// 																		    //       and in the MACRO label on chart, if toggled on.                                                           //
// 																		    //  1.5) You are nearly there! Just go to line 12, and make sure the nMacros variable                              //
// 																		    //     is equal to the overall number of MACROs in the script (Standard value is 8, if you add 2, make it 10!).    //
// 																		    //                                                                                                                 //
// 																		    ///\//////////////////////////////////////////.   PART 2 BELOW   .////////////////////////////////////////////////\//

// CUSTOM MACROS
// Your custom macros go here


//#endregion


//#region[PLOT]
if _mode=="On Chart"
	macroOC(_LINES_LN_1, timeLN1, lblLN1, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', 'ln')       //\////////////////////////////////////////////////////////////////////////////\///
	macroOC(_LINES_LN_2, timeLN2, lblLN2, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', 'ln')       //  INSTRUCTIONS TO ADD CUSTOM MACROs (PART 2):                                  //
	macroOC(_LINES_NY_1, timeNY1, lblNY1, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', 'ny')       //  2.1) Find line 221 under "CUSTOM MACROS ON CHART PLOT" title.                //
	macroOC(_LINES_NY_2, timeNY2, lblNY2, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', 'ny')       //  2.2) Copy it to the following line, remove '//' and replace 'YourMacroName'  //
	macroOC(_LINES_NY_3, timeNY3, lblNY3, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', 'ny')       //       with what you used above in step (1.2).                                 //
	macroOC(_LINES_NY_4, timeNY4, lblNY4, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', 'ny')       //  2.3) Make sure the beginning of the line is aligned with all the ones above; //
	macroOC(_LINES_NY_5, timeNY5, lblNY5, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', 'ny')       //       To do that press 'Tab' once, or add spaces until they match.            //
	macroOC(_LINES_NY_6, timeNY6, lblNY6, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', 'ny')       //                                                                               //
																													///\/////////////////////////.   CONT'D BELOW   .///////////////////////////////\//

	// CUSTOM MACROS ON CHART PLOT
	//macroOC(_lines_YourMacroName, _time_YourMacroName, _lbl_YourMacroName, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', '') 
else
	macroNP(_BOXES_LN_1, timeLN1, lblLN1, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', 'ln')       //\//////////////////////////.      \/\/\/      .//////////////////////////////\///
	macroNP(_BOXES_LN_2, timeLN2, lblLN2, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', 'ln')       //                                                                               //
	macroNP(_BOXES_NY_1, timeNY1, lblNY1, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', 'ny')       //  2.4) Find line 236 under "CUSTOM MACROS NEW PANE PLOT" title.                //
	macroNP(_BOXES_NY_2, timeNY2, lblNY2, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', 'ny')       //  2.5) Copy it to the following line, remove '//' and replace 'YourMacroName'  //
	macroNP(_BOXES_NY_3, timeNY3, lblNY3, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', 'ny')       //       with what you used above in step (1.2).                                 //
	macroNP(_BOXES_NY_4, timeNY4, lblNY4, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', 'ny')       //  2.6) Make sure the beginning of the line is aligned with all the ones above; //
	macroNP(_BOXES_NY_5, timeNY5, lblNY5, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', 'ny')       //       To do that press 'Tab' once, or add spaces until they match.            //
	macroNP(_BOXES_NY_6, timeNY6, lblNY6, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', 'ny')       //                                                                               //
																											 		///\///////////////////////.    LAST STEP BELOW   ./////////////////////////////\//
																											 

	// CUSTOM MACROS NEW PANE PLOT
	//macroNP(_boxes_YourMacroName, _time_YourMacroName, _lbl_YourMacroName, dayofweek(time)==dayofweek.friday and syminfo.type!='crypto', '')
//#endregion


//#region[MEMORY CLEAN UP]
_LINES_LN_1.memoryCleanLine()          
_LINES_LN_2.memoryCleanLine()          
_LINES_NY_1.memoryCleanLine()          
_LINES_NY_2.memoryCleanLine()                    //\//////////////////////////.      \/\/\/      .//////////////////////////////\///        
_LINES_NY_3.memoryCleanLine()                    //                                                                               //        
_LINES_NY_4.memoryCleanLine()                    //  2.4) Find line 253 under "CUSTOM MACROS LINE CLEAN UP" title.                //
_LINES_NY_5.memoryCleanLine()                    //  2.5) Copy it to the following line, remove '//' and replace 'YourMacroName'  //
_LINES_NY_6.memoryCleanLine()                    //       with what you used above in step (1.2).                                 //
												 //                                                                               //
												 ///\/////////////////////.    YOU ARE DONE! GLGT    .///////////////////////////\//

// CUSTOM MACROS LINE CLEAN UP
//_lines_YourMacroName.memoryCleanLine()


EXT.memoryCleanLine()
LBL.memoryCleanLabel(nMacros)
//#endregion


//#region[ALERTS]
alertcondition(time_isnewbarNY("0228-0229:1234567") or 
			   time_isnewbarNY("0358-0359:1234567") or 
			   time_isnewbarNY("0845-0846:1234567") or 
			   time_isnewbarNY("0945-0946:1234567") or 
			   time_isnewbarNY("1045-1046:1234567") or 
			   time_isnewbarNY("1145-1146:1234567") or 
			   time_isnewbarNY("1305-1306:1234567") or        // ADD YOUR ALERTS HERE
			   time_isnewbarNY("1510-1511:1234567") //or      
			 , "Alert 5 Minutes Before Macro"
			 , "A Macro Starts in 5 Minutes!")
//#endregion