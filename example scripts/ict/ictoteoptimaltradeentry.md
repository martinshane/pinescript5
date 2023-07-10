//Fib box for OTE on visible chart only (61.8% - 78.6%)
//Thanks to Pinecoder's VisibleChart library (PineCoders/VisibleChart/4) from which i borrowed code to write the library used here
//Fib extensions added: user input; choose if above or below the range
//User input toggle to choose to anchor the range from wick high/low or candle body high/low
//23rd Jan'23
//@twingall

//@version=5
import twingall/BasicVisibleChart/1

indicator("OTE visible chart", overlay = true)

useBodies =input.bool(false, "use Candle Bodies to anchor range")
showFibBox = input.bool(true, "show Fib Box", group = "OTE box", inline = "2")
boxColor = input.color(color.new(color.yellow, 82), "", group = "OTE box", inline = "2")
showText = input.bool(false, "show Text",group = "OTE box", inline = "2")
showHighLowLines=input.bool(true, "high/low lines", group = "fib retracements",inline = "3")
lineColor = input.color(color.rgb(217, 76, 217), "", group = "fib retracements",inline = "3")
showMidline =input.bool(true, "Midline", group = "fib retracements",inline = "3")
midlineColor = input.color(color.gray,"", group = "fib retracements",inline = "3")
show61eight=input.bool(false, "show 61.8 line", group = "fib retracements",inline = "5")
show78six=input.bool(false, "show 78.6 line", group = "fib retracements",inline = "6")
show88six=input.bool(false, "show 88.6 line", group = "fib retracements",inline = "7")

showfibExt1=input.bool(true,"Ext#1", group ="fib extensions",inline="9")
fibExt1Color=input.color(color.red,"", group ="fib extensions",inline="9")
fibExt1=input.float(1.618, "", group ="fib extensions",inline="9")
showfibExt2=input.bool(true,"Ext#2", group ="fib extensions",inline="9")
fibExt2Color=input.color(color.red,"", group ="fib extensions",inline="9")
fibExt2=input.float(2.0, "", group ="fib extensions",inline="9")
showfibExt3=input.bool(false,"Ext#3", group ="fib extensions",inline="10")
fibExt3Color=input.color(color.red,"", group ="fib extensions",inline="10")
fibExt3=input.float(1.5, "", group ="fib extensions",inline="10")

showfibExt4=input.bool(false,"Ext#4", group ="fib extensions",inline="10")
fibExt4Color=input.color(color.red,"", group ="fib extensions",inline="10")
fibExt4=input.float(2.5, "", group ="fib extensions",inline="10")
showfibExt5=input.bool(false,"Ext#5", group ="fib extensions",inline="11")
fibExt5Color=input.color(color.red,"", group ="fib extensions",inline="11")
fibExt5=input.float(3.0, "", group ="fib extensions",inline="11")
showfibExt6=input.bool(false,"Ext#6", group ="fib extensions",inline="11")
fibExt6Color=input.color(color.red,"", group ="fib extensions",inline="11")
fibExt6=input.float(3.5, "", group ="fib extensions",inline="11")

_88sixColor = input.color(color.green, "", group = "fib retracements",inline = "7")
_61eightColor = input.color(color.green, "", group = "fib retracements",inline = "5")
_78sixColor = input.color(color.green, "", group = "fib retracements",inline = "6")



colorNone = color.new(color.white,100)

showExtLabels=input.bool(false,"Show Labels (Fib extensions)", group ="fib extensions",inline="11")
flipInvertExts=input.bool(false,"flip/invert extensions", group ="fib extensions",inline="12")
showFibLabels = input.bool(false, "Show Labels (Fib retracements)", group="fib retracements", inline="8")

// Calling Library functions to get: high/low, body high / body low, timings, bull/bear
float chartHigh  = useBodies? BasicVisibleChart.highestBody():BasicVisibleChart.high()
float chartLow   =useBodies?BasicVisibleChart.lowestBody():BasicVisibleChart.low()
int   highTime   = BasicVisibleChart.highBarTime()
int   lowTime    = BasicVisibleChart.lowBarTime()
int   leftTime   = math.min(highTime, lowTime)
int   rightTime  = math.max(highTime, lowTime)
bool  isBull     = lowTime < highTime

// Function to manage fib lines. It declares fib lines and label the first time it is called, then sets their properties on subsequent calls. 
fibLine(series color fibColor, series float fibLevel, bool _showPriceLabels) => 
    float fibRatio = 1-(fibLevel / 100)
    float fibPrice = isBull ? chartLow  + ((chartHigh - chartLow) * fibRatio) : 
                              chartHigh - ((chartHigh - chartLow) * fibRatio)
    float _fibLabel = fibLevel /100
    var line  fibLine  =  line.new(na, na, na, na, xloc.bar_time, extend.right, fibColor,  line.style_dotted,  2)
    line.set_xy1(fibLine,  leftTime,  fibPrice)
    line.set_xy2(fibLine,  rightTime, fibPrice)
    var label fibLabel = label.new(na, na, "",     xloc.bar_time, yloc.price,  color(na), label.style_label_up, _showPriceLabels? fibColor:colorNone)
    label.set_xy(fibLabel, int(math.avg(leftTime, rightTime)), fibPrice)
    label.set_text(fibLabel, str.tostring(fibPrice, format.mintick))
    //label.set_text(fibLabel, str.format("{0, number, #.###} ({1})", _fibLabel , str.tostring(fibPrice, format.mintick)))

//Function for Fib Extension lines..
fibExt(series color fibColor, series float fibLevel,  bool _showExt) => 
    float fibRatio = fibLevel / 100
    float fibPrice = isBull ? chartLow  - ((chartHigh - chartLow) * fibRatio) : 
                              chartHigh + ((chartHigh - chartLow) * fibRatio)
    float fibLabel = not flipInvertExts? -(fibLevel /100) :(fibLevel /100)+1  //getting both normal and inverted ext labels to display as positive numbers
    var line  fibLine  =  line.new(na, na, na, na, xloc.bar_time, extend.right, fibColor,  line.style_dotted,  2)
    line.set_xy1(fibLine,  leftTime,  fibPrice)
    line.set_xy2(fibLine,  rightTime, fibPrice)
    var label fibExtLabel = label.new(na, na, "",     xloc.bar_time, yloc.price,  color(na), label.style_label_up, _showExt? fibColor:colorNone)
    label.set_xy(fibExtLabel, int(math.avg(leftTime, rightTime)), fibPrice)
    label.set_text(fibExtLabel, str.format("{0, number, #.###} ({1})", fibLabel , str.tostring(fibPrice, format.mintick)))

//Fib Box function; similar to the above but highlighting the area between 2 fib levels
fibBox(series color fibColor, series float fibLevel_1, series float fibLevel_2) =>
    float fibRatio_1 = 1-(fibLevel_1 / 100)
    float fibPrice_1 = isBull ? chartLow  + ((chartHigh - chartLow) * fibRatio_1) : chartHigh - ((chartHigh - chartLow) * fibRatio_1)
    float fibRatio_2 = 1-(fibLevel_2 / 100)
    float fibPrice_2 = isBull ? chartLow  + ((chartHigh - chartLow) * fibRatio_2) : chartHigh - ((chartHigh - chartLow) * fibRatio_2)
    var b = box.new(na, na, na, na, xloc=xloc.bar_time, border_style=line.style_dashed, extend = extend.right, border_color=colorNone, text_color=color.new(color.gray,70), text_halign=text.align_right)
    box.set_lefttop(b, leftTime, fibPrice_1)
    box.set_rightbottom(b, rightTime,fibPrice_2)
    box.set_bgcolor(b,  fibColor)
    box.set_text(b, text = showText? "OTE":"")
    
// Display code that only runs on the last bar but displays visuals on visible bars, wherever they might be in the dataset.
if barstate.islast
    //Declare fib lines and fib box
    fibLine(lineColor,  100,showFibLabels )
    fibLine(showMidline?midlineColor :colorNone, 50,showFibLabels )
    fibLine(lineColor,  0,showFibLabels )
    fibBox(showFibBox?boxColor:colorNone, 78.6, 61.8)
    fibLine(show88six?_88sixColor:colorNone, 88.6,showFibLabels )
    fibLine(show61eight?_61eightColor:colorNone, 61.8,showFibLabels )
    fibLine(show78six?_78sixColor:colorNone, 78.6,showFibLabels )
        //more fib lines
    //fibLine(_88sixColor, 11.4)
    //fibLine(_61eightColor, 38.2)
    //fibLine(_78sixColor, 21.4)

    //Declare Fib extensions
if barstate.islast and showfibExt1 and flipInvertExts
    fibExt(fibExt1Color, (fibExt1*100)-100, showExtLabels)
if barstate.islast and showfibExt2 and flipInvertExts
    fibExt(fibExt2Color, (fibExt2*100)-100, showExtLabels)
if barstate.islast and showfibExt3 and flipInvertExts
    fibExt(fibExt3Color, (fibExt3*100)-100, showExtLabels)
if barstate.islast and showfibExt4 and flipInvertExts
    fibExt(fibExt4Color, (fibExt4*100)-100, showExtLabels)
if barstate.islast and showfibExt5 and flipInvertExts
    fibExt(fibExt5Color, (fibExt5*100)-100, showExtLabels)
if barstate.islast and showfibExt6 and flipInvertExts
    fibExt(fibExt6Color, (fibExt6*100)-100, showExtLabels)

if barstate.islast and showfibExt1 and not flipInvertExts
    fibExt(fibExt1Color, -(fibExt1*100), showExtLabels)
if barstate.islast and showfibExt2 and not flipInvertExts
    fibExt(fibExt2Color, -(fibExt2*100), showExtLabels)
if barstate.islast and showfibExt3 and not flipInvertExts
    fibExt(fibExt3Color, -(fibExt3*100), showExtLabels)
if barstate.islast and showfibExt4 and not flipInvertExts
    fibExt(fibExt4Color, -(fibExt4*100), showExtLabels)
if barstate.islast and showfibExt5 and not flipInvertExts
    fibExt(fibExt5Color, -(fibExt5*100), showExtLabels)
if barstate.islast and showfibExt6 and not flipInvertExts
    fibExt(fibExt6Color, -(fibExt6*100), showExtLabels)