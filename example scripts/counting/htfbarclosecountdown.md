// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © SamRecio

//This indicator utilizes some 

//@version=5
indicator("HTF Bar Close Countdown", shorttitle = "⏳", overlay = true)

//Requesting and plotting (but hiding the display of) Crypto values to force a more consistent ticking.
//By using data from constantly moving sources I can force the indicator to update more often.
//If I did not do this the indicator would only update every time the price changed on your current chart, 
//for really slow moving tickers it basically just freezes the count for a couple seconds every couple seconds... which is not the point of a countdown.
plot(request.security("COINBASE:BTCUSD","",close), display = display.none, title = "BTCUSD", editable = false)
plot(request.security("COINBASE:ETHUSD","",close), display = display.none, title = "ETHUSD", editable = false)

//Timeframe Settings
tf1tog = input.bool(true, title = "", group ="HTF Bar Close Countdown", inline = "1")
tf1 = input.timeframe("15", title = "", group ="HTF Bar Close Countdown", inline = "1")
col1 = input.color(color.white, title = "Text", group ="HTF Bar Close Countdown", inline = "1")
bg1 = input.color(color.rgb(0,0,0,100), title = "Background", group ="HTF Bar Close Countdown", inline = "1")
tf2tog = input.bool(true, title = "", group ="HTF Bar Close Countdown", inline = "2")
tf2 = input.timeframe("60", title = "", group ="HTF Bar Close Countdown", inline = "2")
col2 = input.color(color.white, title = "Text", group ="HTF Bar Close Countdown", inline = "2")
bg2 = input.color(color.rgb(0,0,0,100), title = "Background", group ="HTF Bar Close Countdown", inline = "2")
tf3tog = input.bool(true, title = "", group ="HTF Bar Close Countdown", inline = "3")
tf3 = input.timeframe("240", title = "", group ="HTF Bar Close Countdown", inline = "3")
col3 = input.color(color.white, title = "Text", group ="HTF Bar Close Countdown", inline = "3")
bg3 = input.color(color.rgb(0,0,0,100), title = "Background", group ="HTF Bar Close Countdown", inline = "3")
tf4tog = input.bool(true, title = "", group ="HTF Bar Close Countdown", inline = "4")
tf4 = input.timeframe("D", title = "", group ="HTF Bar Close Countdown", inline = "4")
col4 = input.color(color.white, title = "Text", group ="HTF Bar Close Countdown", inline = "4")
bg4 = input.color(color.rgb(0,0,0,100), title = "Background", group ="HTF Bar Close Countdown", inline = "4")
tf5tog = input.bool(true, title = "", group ="HTF Bar Close Countdown", inline = "5")
tf5 = input.timeframe("W", title = "", group ="HTF Bar Close Countdown", inline = "5")
col5 = input.color(color.white, title = "Text", group ="HTF Bar Close Countdown", inline = "5")
bg5 = input.color(color.rgb(0,0,0,100), title = "Background", group ="HTF Bar Close Countdown", inline = "5")
//Table Settings
tsize = input.string("normal", title = "Table Size", options = ["auto","tiny","small", "normal", "large","huge"], group = "Countdown Table Settings", inline = "1")
flat = input.bool(false, title = "Flat Display?", group = "Countdown Table Settings", inline = "1")
typos = input.string("middle", title = "Position", options = ["top", "middle", "bottom"], group = "Countdown Table Settings", inline = "2")
txpos = input.string("right", title = "", options=["left", "center", "right"], group = "Countdown Table Settings", inline = "2")
tframe = input.color(color.white, title = "Frame Color:", group = "Countdown Table Settings", inline = "3")
tframes = input.int(0, minval = 0, title = "Width:", group = "Countdown Table Settings", inline = "3")
tborder = input.color(color.new(color.white,30), title ="Border Color:", group = "Countdown Table Settings", inline = "4")
tborders = input.int(1, minval = 0, title = "Width:", group = "Countdown Table Settings", inline = "4")

//Table Setup
var display = table.new(typos+"_"+txpos,10,5, border_color = tborder, border_width = tborders, frame_color = tframe, frame_width = tframes)

//Countdown Function
//This countdown method takes advantage of the "time_close()" function to determine where the input timeframe's candle close.
//By knowing at what time it closes, we can subtract where we are now from it to get how far we are from the input timeframe's close.
//This function converts all the data into strings so that I may easily input it as text into the table.
countdown(tf) =>
    til_next = ((time_close(tf)/1000) - math.floor(timenow/1000))
    next_day = math.floor(((til_next/60)/60)/24)
    next_hour = math.floor((til_next/60)/60) - (next_day*24)
    next_min = math.floor(til_next/60) - math.floor((til_next/60)/60)*60
    next_sec = til_next - (math.floor(til_next/60)*60)
    place0 = next_day == 0?"":str.tostring(next_day,"00") + ":"
    place1 = next_hour == 0?"": str.tostring(next_hour,"00") + ":"
    place2 = str.tostring(next_min,"00") + ":"
    place3 = str.tostring(next_sec,"00")
    place0 + place1 + place2 + place3

//Send to table function,
//This function allows me to add the stuff to the table based on the individual toggles. 
//By creating a function to add them to the table, you can see later that executing this function is a lot simpler and easier to follow than otherwise, since it is just doing the same thing 5 times.
//Theoretically this can be easily expaned to how ever many countdowns you want. I decided 5 was a good number, and if you want less, you can toggle them off!
to_table(_tog,_flat,_pos,_tf,_col,_bg) =>
    if (_flat == false) and _tog
        //Timeframe Title
        table.cell(display,0,_pos-1,text = _tf, text_halign = text.align_left, text_color = _col, bgcolor = _bg, text_size = tsize)
        //Countdown Values
        table.cell(display,1,_pos-1,text = session.islastbar?"Closed":countdown(_tf), text_halign = text.align_right, text_color = _col, bgcolor = _bg, text_size = tsize)
    if _flat and _tog
        //Timeframe Title
        table.cell(display,(_pos*2)-2,0,text = _tf, text_halign = text.align_right, text_color = _col, bgcolor = _bg, text_size = tsize)
        //Countdown Values
        table.cell(display,(_pos*2)-1,0,text = session.islastbar?"Closed":countdown(_tf), text_halign = text.align_left, text_color = _col, bgcolor = _bg, text_size = tsize)

//Function Execution
if barstate.islast
    to_table(tf1tog,flat,1,tf1,col1,bg1)
    to_table(tf2tog,flat,2,tf2,col2,bg2)
    to_table(tf3tog,flat,3,tf3,col3,bg3)
    to_table(tf4tog,flat,4,tf4,col4,bg4)
    to_table(tf5tog,flat,5,tf5,col5,bg5)
