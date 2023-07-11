//indentify & label consecutive Lower high and higher low Pivot highs and lows; cumulative count until previous PH/PL gets swept
//Added optional 'spikey' condition with 'spikey' index: for filtering out unimpressive/rounded pivot highs & lows
//Added sweep labels; each prior successively LH or HL swept gets a label: e.g it sweeps the last in a sequence of LHs, prints 'ʌ', it then sweeps the second last of the LHs, prints double 'ʌ', etc, etc...
//March 20th 2023
// © twingall

//@version=5
indicator("Consecutive unswept lower highs / higher lows", shorttitle= "Consec LHs/HLs counter", overlay = true, max_labels_count = 500, max_bars_back = 5000)
    //user inputs
lb1 =input.int(10, 'pivots: lookback/forward', group = 'Pivots highs & lows', tooltip = "larger number for more significant pivot highs / pivot lows")
useSpikeyCond = input.bool(true, "Apply 'Spikeyness' filter", inline ='2', group = 'Pivots highs & lows')
atrMult =input.float(1.3, '|| Spikeyness index', minval = 0, step =0.1, inline ='2', group = 'Pivots highs & lows', tooltip = "For catching only the 'spiky' pivot highs/pivot lows\n\nIncreasing this number will filter out smooth/unimpressive pivot high&lows\n\nRepresents local ATR multiple for distance from local Moving average a high or low must be\n\nSetting of 1.5 seems optimal on 15m chart with pivot lookback/lookforward = 15")
mult =input.int(500, "Historical Labels to show (pairs)", minval = 1, group = 'counter labels')
showHighs = input.bool(true, "high label", inline='1', group = 'counter labels')
showLows = input.bool(true, "low label",inline='2',  group = 'counter labels')
txtColH = input.color(color.red, "| txtCol", inline ='1', group = 'counter labels')
txtColL = input.color(color.blue, "| txtCol:", inline ='2', group = 'counter labels')
labColH=input.color(color.orange, "| labCol:", inline ='1', group = 'counter labels')
labColL=input.color(color.orange, "| labCol:", inline ='2', group = 'counter labels')
inputLabStyleH = input.string("none", "| style", inline='1', group = 'counter labels', options = ["none", "down label", "triandle down"])
inputLabStyleL = input.string("none", "| style", inline='2', group = 'counter labels', options = ["none", "up label", "triandle up"])
txtSize = input.string(size.normal,"Size:", options = [size.auto, size.tiny, size.small, size.normal, size.large, size.huge], inline ='3', group = 'counter labels')
unit_1 = input.float(1.75, "Label spacer above/below (ATR multiples)",minval = 0, step = 0.5, group = 'counter labels', tooltip = "spacer above/below for pivot labels, based on multiples of long-term average true range")
atrSpacer = ta.atr(300)
showSweepsHighs= input.bool(true, "successive lower high sweep labels", group = "Sweeps Labels", inline ='1')
colSwpH = input.color(color.fuchsia, "", group = "Sweeps Labels", inline ='1', tooltip = "Each sweep or a prior successively Lower High gets a label: \n\ne.g it sweeps the last in a sequence of LHs, prints 'ʌ',\n\nIf it then sweeps the second last of the LHs, prints double 'ʌ', etc, etc...")
showSweepsLows = input.bool(true, "successive higher low sweep labels", group = "Sweeps Labels", inline ='2')
colSwpL = input.color(color.fuchsia, "", group = "Sweeps Labels", inline ='2', tooltip = "Each sweep or a prior successively Higher Low gets a label: \n\ne.g it sweeps the last in a sequence of LHs, prints 'v',\n\nIf it then sweeps the second last of the LHs, prints double 'v', etc, etc...")

unit_2 = input.float(2.75, "Label spacer above/below (ATR multiples)",minval = 0, step = 0.5, group =  "Sweeps Labels", tooltip = "spacer above/below for sweep labels, based on multiples of long-term average true range")
txtSize2 = input.string(size.normal,"Size:", options = [size.auto, size.tiny, size.small, size.normal, size.large, size.huge], inline ='3', group =  "Sweeps Labels")
string labStyleH = switch inputLabStyleH 
    "down label" => label.style_label_down
    "triandle down" =>  label.style_triangledown
    => label.style_none
string labStyleL = switch inputLabStyleL
    "up label" => label.style_label_up
    "triandle up" =>  label.style_triangleup
    => label.style_none

colorNone=color.new(color.white, 100)
pivHigh = ta.pivothigh(high, lb1, lb1)
pivLow = ta.pivotlow(low, lb1, lb1)
isPivHigh = na(pivHigh)? false:true
isPivLow = na(pivLow)? false:true
realTimeBar = input.bool(false, "alert on realtime bar", group= 'alerts', inline ='1', tooltip = "set as true to get early confirmation on realtime bar\n\ndefault is off since this can cause repainting; default is off which means alert only fires after PH/PL is properly confirmed by a candle close (one bar later than if toggled ON)")

alertThreshold_PH = input.int(7, "Piv High Alert Threshold: consecutive lower highs = ", minval= 2, inline = '2', group= 'alerts', tooltip = "Alerts need to be manually set:\nClick the three dots on indicator status line for adding your alert\n\nThis number applies only to the FIRST in the list of alerts\n\nWhen Consec pivot High equaling this number prints (lookback bars after its location), alert will fire\n\nMinval = 2, if you want PH = 1 just use the '__Pivot High confirmed' alert" )
alertThreshold_PL = input.int(7, "Piv Low Alert Threshold: consecutive higher lows = ", minval =2, inline = '3', group= 'alerts', tooltip = "Alerts need to be manually set:\n click the three dots on indicator status line for adding your alert\n\nThis number applies only to the SECOND in the list of alerts\n\nWhen Consec pivot Low equaling this number prints (lookback bars after its location), alert will fire\n\nMinval = 2, if you want PH = 1 just use the '__Pivot Low confirmed' alert")

var PH = float(na)
var PL = float(na)
var array<float> PHarr = array.new<float>(0)
var array<float> PLarr = array.new<float>(0)

var int countPH = 0    
var int countPL = 0    
countLabH = label(na)
countLabL = label(na)
var array<label> countLabHArr = array.new<label>(0)
var array<label> countLabLArr = array.new<label>(0)
atr = ta.atr(2*lb1)
maH = ta.sma(high, 2*lb1)
bool spikeyH = useSpikeyCond? pivHigh > maH+atrMult*atr:true

    //counters to allow clearing of arrays; fresh arrays for each pivot
var int countPHtot =0 
var int countPLtot =0

var array<int>phNumArr=array.new<int>(0)
if isPivHigh 
    if spikeyH
        countPH += 1
        countPHtot+=1
        PH:= pivHigh
        array.push(PHarr,PH)
        if array.last(PHarr) > array.last(PHarr)[1]
            countPH :=1
        array.push(phNumArr,countPH)
        countLabH:=label.new(bar_index-lb1, high[lb1]+unit_1*atrSpacer, str.tostring(countPH), style=showHighs?labStyleH:label.style_none, textcolor =showHighs?txtColH:colorNone, size = txtSize, color = showHighs?labColH:colorNone)
        array.push(countLabHArr,countLabH)
        if array.size(countLabHArr)>mult
            label.delete(array.shift(countLabHArr))

var array<int>plNumArr=array.new<int>(0)
maL = ta.sma(low, 2*lb1)
bool spikeyL = useSpikeyCond? pivLow < maL-atrMult*atr:true
if isPivLow 
    if spikeyL
        countPL += 1
        countPLtot+=1
        PL:= pivLow
        array.push(PLarr,PL)
        if array.last(PLarr) < array.last(PLarr)[1]
            countPL :=1
        array.push(plNumArr,countPL)
        countLabL:=label.new(bar_index-lb1, low[lb1]-unit_1*atrSpacer, text =str.tostring(countPL), style=showLows?labStyleL:label.style_none, textcolor =showLows?txtColL:colorNone, size = txtSize, color = showLows?labColL:colorNone)
        array.push(countLabLArr,countLabL)
        if array.size(countLabLArr)>mult
            label.delete(array.shift(countLabLArr))

_countPH = realTimeBar?countPH:countPH[1]
_countPL = realTimeBar?countPL:countPL[1]
_isPivHigh = realTimeBar? isPivHigh and spikeyH: isPivHigh[1] and spikeyH[1]
_isPivLow  = realTimeBar? isPivLow and spikeyL: isPivLow[1] and spikeyL[1]

alertcondition(_countPH == alertThreshold_PH, title='__Alert: Piv High Threshold', message='Piv High Threshold Hit')
alertcondition(_countPL == alertThreshold_PL, title='__Alert: Piv Low Threshold', message='Piv Low Threshold Hit')


alertcondition(_isPivHigh, title='__Pivot High confirmed', message="Pivot High confirmed!")
alertcondition(_countPH == 2, title='_2nd pivot HIGH (successively lower)', message="2nd successively lower pivot high has printed")
alertcondition(_countPH == 3, title='_3rd pivot HIGH (successively lower)', message="3rd successively lower pivot high has printed ")
alertcondition(_countPH == 4, title='_4th pivot HIGH (successively lower)', message="4th successively lower pivot high has printed ")
alertcondition(_countPH == 5, title='_5th pivot HIGH (successively lower)', message="5th successively lower pivot high has printed ")
alertcondition(_countPH == 6, title='_6th pivot HIGH (successively lower)', message="6th successively lower pivot high has printed ")
alertcondition(_countPH == 7, title='_7th pivot HIGH (successively lower)', message="7th successively lower pivot high has printed ")
alertcondition(_countPH == 8, title='_8th pivot HIGH (successively lower)', message="8th successively lower pivot high has printed ")
alertcondition(_countPH == 9, title='_9th pivot HIGH (successively lower)', message="9th successively lower pivot high has printed ")

alertcondition(_isPivLow, title='__Pivot Low confirmed', message="Pivot Low confirmed!")
alertcondition(_countPL== 2, title='2nd pivot LOW (successively higher)', message="2nd successively higher pivot low has printed")
alertcondition(_countPL == 3, title='3rd pivot LOW (successively higher)', message="3rd successively higher pivot low has printed ")
alertcondition(_countPL == 4, title='4th pivot LOW (successively higher)', message="4th successively higher pivot low has printed ")
alertcondition(_countPL == 5, title='5th pivot LOW (successively higher)', message="5th successively higher pivot low has printed ")
alertcondition(_countPL == 6, title='6th pivot LOW (successively higher)', message="6th successively higher pivot low has printed ")
alertcondition(_countPL == 7, title='7th pivot LOW (successively higher)', message="7th successively higher pivot low has printed ")
alertcondition(_countPL == 8, title='8th pivot LOW (successively higher)', message="8th successively higher pivot low has printed ")
alertcondition(_countPL == 9, title='9th pivot LOW (successively higher)', message="9th successively higher pivot low has printed ")

var label labRtL1 =na, var label labRtL2 =na, var label labRtL3 =na, var label labRtL4 =na, var label labRtL5 =na, var label labRtL6 =na, var label labRtL7 =na, var label labRtL8 =na, var label labRtL9 =na, var label labRtL10 =na
var array<label> labRtLArr1 = array.new<label>(0), var array<label> labRtLArr2 = array.new<label>(0), var array<label> labRtLArr3 = array.new<label>(0)
var array<label> labRtLArr4 = array.new<label>(0), var array<label> labRtLArr5 = array.new<label>(0), var array<label> labRtLArr6 = array.new<label>(0)
var array<label> labRtLArr7 = array.new<label>(0), var array<label> labRtLArr8 = array.new<label>(0), var array<label> labRtLArr9 = array.new<label>(0), var array<label> labRtLArr10 = array.new<label>(0)

var label labRtH1 =na, var label labRtH2 =na, var label labRtH3 =na, var label labRtH4 =na, var label labRtH5 =na, var label labRtH6 =na, var label labRtH7 =na, var label labRtH8 =na, var label labRtH9 =na, var label labRtH10 =na
var array<label> labRtHArr1 = array.new<label>(0), var array<label> labRtHArr2 = array.new<label>(0), var array<label> labRtHArr3 = array.new<label>(0)
var array<label> labRtHArr4 = array.new<label>(0), var array<label> labRtHArr5 = array.new<label>(0), var array<label> labRtHArr6 = array.new<label>(0)
var array<label> labRtHArr7 = array.new<label>(0), var array<label> labRtHArr8 = array.new<label>(0), var array<label> labRtHArr9 = array.new<label>(0), var array<label> labRtHArr10 = array.new<label>(0)


//// fixing issue where sweep labels over-printed on sequence of HHs 
//// also condition for retaining last array element before array.clear to make 'higher stacked' labels print more effectively
var int _lastLnum = 0
var int _2ndLastLnum = 0
sizePlNumArr = array.size(plNumArr)
if sizePlNumArr >3
    _lastLnum := array.last(plNumArr)
    _2ndLastLnum:= array.get(plNumArr,sizePlNumArr-2)
var float keepLstLo =0.0
if array.size(PLarr)>0
    keepLstLo:=array.last(PLarr)
if countPL<countPL[1] or _lastLnum==_2ndLastLnum
    array.clear(PLarr)
if countPL[1]<countPL[2] or _lastLnum[1]==_2ndLastLnum[1]
    array.push(PLarr,keepLstLo)
sizeLarr=array.size(PLarr)

var int _lastHnum = 0
var int _2ndLastHnum = 0
sizePhNumArr = array.size(phNumArr)
if sizePhNumArr >3
    _lastHnum := array.last(phNumArr)
    _2ndLastHnum:= array.get(phNumArr,sizePhNumArr-2)
var float keepLstHi =0.0
if array.size(PHarr)>0
    keepLstHi:=array.last(PHarr)
if countPH<countPH[1] or _lastHnum==_2ndLastHnum
    array.clear(PHarr)
if countPH[1]<countPH[2] or _lastHnum[1]==_2ndLastHnum[1]
    array.push(PHarr,keepLstHi)
sizeHarr=array.size(PHarr)

    //down sweeps:
var float PLprev1 = na, var float PLprev2 = na, var float PLprev3 = na, var float PLprev4 = na, var float PLprev5 = na, var float PLprev6 = na, var float PLprev7 = na, var float PLprev8 = na, var float PLprev9 = na, var float PLprev10 = na
if sizeLarr ==0 and sizeLarr[1] !=0
    PLprev1:=na, PLprev2:=na,PLprev3:=na,PLprev4:=na, PLprev5:=na, PLprev6:=na, PLprev7:=na, PLprev8:=na, PLprev9:=na, PLprev10:=na
if sizeLarr ==1 and sizeLarr[1] ==0
    PLprev1:=array.get(PLarr, 0),PLprev2:=na,PLprev3:=na,PLprev4:=na, PLprev5:=na, PLprev6:=na, PLprev7:=na, PLprev8:=na, PLprev9:=na, PLprev10:=na
if sizeLarr ==2 and sizeLarr[1] ==1
    PLprev1:=array.get(PLarr, 1),PLprev2:=array.get(PLarr, 0),PLprev3:=na,PLprev4:=na, PLprev5:=na, PLprev6:=na, PLprev7:=na, PLprev8:=na, PLprev9:=na, PLprev10:=na
if sizeLarr ==3 and sizeLarr[1] ==2
    PLprev1:=array.get(PLarr, 2),PLprev2:=array.get(PLarr, 1),PLprev3:=array.get(PLarr, 0),PLprev4:=na, PLprev5:=na, PLprev6:=na, PLprev7:=na, PLprev8:=na, PLprev9:=na, PLprev10:=na
if sizeLarr ==4 and sizeLarr[1] ==3
    PLprev1:=array.get(PLarr, 3),PLprev2:=array.get(PLarr, 2),PLprev3:=array.get(PLarr, 1),PLprev4:=array.get(PLarr, 0), PLprev5:=na, PLprev6:=na, PLprev7:=na, PLprev8:=na, PLprev9:=na, PLprev10:=na
if sizeLarr ==5 and sizeLarr[1] ==4
    PLprev1:=array.get(PLarr, 4),PLprev2:=array.get(PLarr, 3),PLprev3:=array.get(PLarr, 2),PLprev4:=array.get(PLarr, 1), PLprev5:=array.get(PLarr, 0), PLprev6:=na, PLprev7:=na, PLprev8:=na, PLprev9:=na, PLprev10:=na
if sizeLarr ==6 and sizeLarr[1] ==5
    PLprev1:=array.get(PLarr, 5),PLprev2:=array.get(PLarr, 4),PLprev3:=array.get(PLarr, 3),PLprev4:=array.get(PLarr, 2), PLprev5:=array.get(PLarr, 1), PLprev6:=array.get(PLarr, 0), PLprev7:=na, PLprev8:=na, PLprev9:=na, PLprev10:=na
if sizeLarr ==7 and sizeLarr[1] ==6
    PLprev1:=array.get(PLarr, 6),PLprev2:=array.get(PLarr, 5),PLprev3:=array.get(PLarr, 4),PLprev4:=array.get(PLarr, 3), PLprev5:=array.get(PLarr, 2), PLprev6:=array.get(PLarr, 1), PLprev7:=array.get(PLarr, 0), PLprev8:=na, PLprev9:=na, PLprev10:=na
if sizeLarr ==8 and sizeLarr[1] ==7
    PLprev1:=array.get(PLarr, 7),PLprev2:=array.get(PLarr, 6),PLprev3:=array.get(PLarr, 5),PLprev4:=array.get(PLarr, 4), PLprev5:=array.get(PLarr, 3), PLprev6:=array.get(PLarr, 2), PLprev7:=array.get(PLarr, 1), PLprev8:=array.get(PLarr, 0), PLprev9:=na, PLprev10:=na
if sizeLarr ==9 and sizeLarr[1] ==8
    PLprev1:=array.get(PLarr, 8),PLprev2:=array.get(PLarr, 7),PLprev3:=array.get(PLarr, 6),PLprev4:=array.get(PLarr, 5), PLprev5:=array.get(PLarr, 4), PLprev6:=array.get(PLarr, 3), PLprev7:=array.get(PLarr, 2), PLprev8:=array.get(PLarr, 1), PLprev9:=array.get(PLarr, 0), PLprev10:=na

    //up sweeps:
var float PHprev1 = na, var float PHprev2 = na, var float PHprev3 = na, var float PHprev4 = na, var float PHprev5 = na, var float PHprev6 = na, var float PHprev7 = na, var float PHprev8 = na, var float PHprev9 = na, var float PHprev10 = na
if sizeHarr ==0 and sizeHarr[1] !=0
    PHprev1:=na, PHprev2:=na,PHprev3:=na,PHprev4:=na, PHprev5:=na, PHprev6:=na, PHprev7:=na, PHprev8:=na, PHprev9:=na, PHprev10:=na
if sizeHarr ==1 and sizeHarr[1] ==0
    PHprev1:=array.get(PHarr,0),PHprev2:=na,PHprev3:=na,PHprev4:=na, PHprev5:=na, PHprev6:=na, PHprev7:=na, PHprev8:=na, PHprev9:=na, PHprev10:=na  
if sizeHarr ==2 and sizeHarr[1] ==1
    PHprev1:=array.get(PHarr,1),PHprev2:=array.get(PHarr, 0),PHprev3:=na,PHprev4:=na, PHprev5:=na, PHprev6:=na, PHprev7:=na, PHprev8:=na, PHprev9:=na, PHprev10:=na
if sizeHarr ==3 and sizeHarr[1] ==2
    PHprev1:=array.get(PHarr,2),PHprev2:=array.get(PHarr, 1),PHprev3:=array.get(PHarr, 0),PHprev4:=na, PHprev5:=na, PHprev6:=na, PHprev7:=na, PHprev8:=na, PHprev9:=na, PHprev10:=na
if sizeHarr ==4 and sizeHarr[1] ==3
    PHprev1:=array.get(PHarr,3),PHprev2:=array.get(PHarr, 2),PHprev3:=array.get(PHarr, 1),PHprev4:=array.get(PHarr, 0), PHprev5:=na, PHprev6:=na, PHprev7:=na, PHprev8:=na, PHprev9:=na, PHprev10:=na
if sizeHarr ==5 and sizeHarr[1] ==4
    PHprev1:=array.get(PHarr,4),PHprev2:=array.get(PHarr, 3),PHprev3:=array.get(PHarr, 2),PHprev4:=array.get(PHarr, 1), PHprev5:=array.get(PHarr, 0), PHprev6:=na, PHprev7:=na, PHprev8:=na, PHprev9:=na, PHprev10:=na
if sizeHarr ==6 and sizeHarr[1] ==5
    PHprev1:=array.get(PHarr,5),PHprev2:=array.get(PHarr, 4),PHprev3:=array.get(PHarr, 3),PHprev4:=array.get(PHarr, 2), PHprev5:=array.get(PHarr, 1), PHprev6:=array.get(PHarr, 0), PHprev7:=na, PHprev8:=na, PHprev9:=na, PHprev10:=na
if sizeHarr ==7 and sizeHarr[1] ==6
    PHprev1:=array.get(PHarr,6),PHprev2:=array.get(PHarr, 5),PHprev3:=array.get(PHarr, 4),PHprev4:=array.get(PHarr, 3), PHprev5:=array.get(PHarr, 2), PHprev6:=array.get(PHarr, 1), PHprev7:=array.get(PHarr, 0), PHprev8:=na, PHprev9:=na, PHprev10:=na
if sizeHarr ==8 and sizeHarr[1] ==7
    PHprev1:=array.get(PHarr,7),PHprev2:=array.get(PHarr, 6),PHprev3:=array.get(PHarr, 5),PHprev4:=array.get(PHarr, 4), PHprev5:=array.get(PHarr, 3), PHprev6:=array.get(PHarr, 2), PHprev7:=array.get(PHarr, 1), PHprev8:=array.get(PHarr, 0), PHprev9:=na, PHprev10:=na
if sizeHarr ==9 and sizeHarr[1] ==8
    PHprev1:=array.get(PHarr,8),PHprev2:=array.get(PHarr, 7),PHprev3:=array.get(PHarr, 6),PHprev4:=array.get(PHarr, 5), PHprev5:=array.get(PHarr, 4), PHprev6:=array.get(PHarr, 3), PHprev7:=array.get(PHarr, 2), PHprev8:=array.get(PHarr, 1), PHprev9:=array.get(PHarr, 0), PHprev10:=na

/// ~~~ FUNCTIONS ~~~
clrArrsL(arr)=>     //clear sweep label arrays after each pivot low
    if countPLtot != countPLtot[1]
        array.clear(arr)
plCond(prev)=>
    ((low < prev and not na(prev)) or (close < prev and not na(prev))) 
txtFuncL(txt)=>
    label.new(bar_index, low-unit_2*atrSpacer, text = txt, style = label.style_label_up,textcolor = colSwpL, color = colorNone, size = txtSize2)

clrArrsH(arr)=>     //clear sweep label arrays after each pivot high
    if countPHtot != countPHtot[1]
        array.clear(arr)
phCond(prev)=>
    ((high > prev and not na(prev)) or (close > prev and not na(prev))) 
txtFuncH(txt)=>
    label.new(bar_index, high+unit_2*atrSpacer, text = txt, style = label.style_label_down,textcolor = colSwpH, color = colorNone, size = txtSize2)

arrFunc(arr)=>
    if array.size(arr)>1
        label.delete(array.last(arr))

/// ~~~ FUNCTIONS end ~~~

    //consec HL sweeps; down
if plCond(PLprev1) and showSweepsLows
    labRtL1:=txtFuncL("v"), array.push(labRtLArr1,labRtL1), arrFunc(labRtLArr1)
clrArrsL(labRtLArr1)
if plCond(PLprev2) and showSweepsLows
    labRtL2:=txtFuncL("v\nv"), array.push(labRtLArr2,labRtL2), arrFunc(labRtLArr2)
clrArrsL(labRtLArr2)
if plCond(PLprev3) and showSweepsLows
    labRtL3:=txtFuncL("v\nv\nv"), array.push(labRtLArr3,labRtL3), arrFunc(labRtLArr3)
clrArrsL(labRtLArr3)
if plCond(PLprev4) and showSweepsLows
    labRtL4:=txtFuncL("v\nv\nv\nv"), array.push(labRtLArr4,labRtL4), arrFunc(labRtLArr4)
clrArrsL(labRtLArr4)
if plCond(PLprev5) and showSweepsLows
    labRtL5:=txtFuncL("v\nv\nv\nv\nv"), array.push(labRtLArr5,labRtL5), arrFunc(labRtLArr5)
clrArrsL(labRtLArr5)
if plCond(PLprev6) and showSweepsLows
    labRtL6:=txtFuncL("v\nv\nv\nv\nv\nv"), array.push(labRtLArr6,labRtL6), arrFunc(labRtLArr6)
clrArrsL(labRtLArr6)
if plCond(PLprev7) and showSweepsLows
    labRtL7:=txtFuncL("v\nv\nv\nv\nv\nv\nv"), array.push(labRtLArr7,labRtL7), arrFunc(labRtLArr7)
clrArrsL(labRtLArr7)
if plCond(PLprev8) and showSweepsLows
    labRtL8:=txtFuncL("v\nv\nv\nv\nv\nv\nv\nv"), array.push(labRtLArr8,labRtL8), arrFunc(labRtLArr8)
clrArrsL(labRtLArr8)
if plCond(PLprev9) and showSweepsLows
    labRtL9:=txtFuncL("v\nv\nv\nv\nv\nv\nv\nv\nv"), array.push(labRtLArr9,labRtL9), arrFunc(labRtLArr9)
clrArrsL(labRtLArr9)

   // consec LH sweeps; up
if phCond(PHprev1) and showSweepsHighs
    labRtH1:=txtFuncH("ʌ"), array.push(labRtHArr1,labRtH1), arrFunc(labRtHArr1) //Ʌ ʌ
clrArrsH(labRtHArr1)
if phCond(PHprev2) and showSweepsHighs
    labRtH2:=txtFuncH("ʌ\nʌ"), array.push(labRtHArr2,labRtH2), arrFunc(labRtHArr2)
clrArrsH(labRtHArr2)
if phCond(PHprev3) and showSweepsHighs
    labRtH3:=txtFuncH("ʌ\nʌ\nʌ"), array.push(labRtHArr3,labRtH3), arrFunc(labRtHArr3)
clrArrsH(labRtHArr3)
if phCond(PHprev4) and showSweepsHighs
    labRtH4:=txtFuncH("ʌ\nʌ\nʌ\nʌ"), array.push(labRtHArr4,labRtH4), arrFunc(labRtHArr4)
clrArrsH(labRtHArr4)
if phCond(PHprev5) and showSweepsHighs
    labRtH5:=txtFuncH("ʌ\nʌ\nʌ\nʌ\nʌ"), array.push(labRtHArr5,labRtH5), arrFunc(labRtHArr5)
clrArrsH(labRtHArr5)
if phCond(PHprev6) and showSweepsHighs
    labRtH6:=txtFuncH("ʌ\nʌ\nʌ\nʌ\nʌ\nʌ"), array.push(labRtHArr6,labRtH6), arrFunc(labRtHArr6)
clrArrsH(labRtHArr6)
if phCond(PHprev7) and showSweepsHighs
    labRtH7:=txtFuncH("ʌ\nʌ\nʌ\nʌ\nʌ\nʌ\nʌ"), array.push(labRtHArr7,labRtH7), arrFunc(labRtHArr7)
clrArrsH(labRtHArr7)
if phCond(PHprev8) and showSweepsHighs
    labRtH8:=txtFuncH("ʌ\nʌ\nʌ\nʌ\nʌ\nʌ\nʌ\nʌ"), array.push(labRtHArr8,labRtH8), arrFunc(labRtHArr8)
clrArrsH(labRtHArr8)
if phCond(PHprev9) and showSweepsHighs
    labRtH9:=txtFuncH("ʌ\nʌ\nʌ\nʌ\nʌ\nʌ\nʌ\nʌ\nʌ"), array.push(labRtHArr9,labRtH9), arrFunc(labRtHArr9)
clrArrsH(labRtHArr9)
