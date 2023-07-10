// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © only_fibonacci

//@version=5
indicator("Signal Table v1",overlay=true)

sembol = input.symbol("","Symbol")
var table mobiletablo = table.new(position.bottom_right,2,6,border_width=4,border_color=color.gray, frame_color=color.gray, frame_width=4)
var table tablo = table.new(position.top_right,6,6,border_width=4,border_color=color.gray, frame_color=color.gray, frame_width=4)

emaGoster = input.bool(true,"Show Ema's")
fibonacciGoster=input.bool(false,"Show Fibonacci Retracement")
mobilGoster = input.bool(false,"Is mobile ?")
period=input.timeframe(defval="",title="Time Period")
uzunluk=input.int(233, minval=8, title="Fibonacci Distance")
tepe = request.security(syminfo.tickerid,period,ta.highest(uzunluk))
dip = request.security(syminfo.tickerid,period,ta.lowest(uzunluk))
tepeyeUzaklik = request.security(syminfo.tickerid,period,math.abs(ta.highestbars(uzunluk)))
dipeUzaklik = request.security(syminfo.tickerid,period,math.abs(ta.lowestbars(uzunluk)))
fib618=dipeUzaklik>tepeyeUzaklik ? tepe-((tepe-dip)*618/1000) : dip+((tepe-dip)*618/1000)
fib50=dipeUzaklik>tepeyeUzaklik ? tepe-((tepe-dip)*500/1000) : dip+((tepe-dip)*500/1000)
fib382=dipeUzaklik>tepeyeUzaklik ? tepe-((tepe-dip)*382/1000) : dip+((tepe-dip)*382/1000)
fib786=dipeUzaklik>tepeyeUzaklik ? tepe-((tepe-dip)*786/1000) : dip+((tepe-dip)*786/1000)

saydam = fibonacciGoster?true:false
///////////////////////////// fib kapat aç
plot(tepe,linewidth=1,color=color.red,style=plot.style_linebr,trackprice=saydam,transp=100,title="High",editable=false)
plot(dip,linewidth=1,color=color.green,style=plot.style_linebr,trackprice=saydam,transp=100,title="Low",editable=false)
plot(fib618,linewidth=1,color=color.black,style=plot.style_linebr,trackprice=saydam,transp=100,title="Fib 0,618",editable=false)
plot(fib382,linewidth=1,color=color.black,style=plot.style_linebr,trackprice=saydam,transp=100,title="Fib 0,382",editable=false)
plot(fib50,linewidth=1,color=color.black,style=plot.style_linebr,trackprice=saydam,transp=100,title="Fib 0,50",editable=false)
plot(fib786,linewidth=1,color=color.black,style=plot.style_linebr,trackprice=saydam,transp=100,title="Fib 0,786",editable=false)

/////////////////////////////////////
saydamEma = emaGoster?0:100
emaK = input.int(defval = 9, title="Short EMA")
emaU = input.int(defval = 28, title="Long EMA")


ema1=input.int(defval=50, title="EMA 1")
ema2=input.int(defval=100, title="EMA 2")
ema3=input.int(defval=200, title="EMA 3")
sma1 = input.int(defval=50, title="SMA 1")
sma2= input.int(defval=100, title="SMA 2")
sma3 = input.int(defval=200, title="SMA 3")

rsi = request.security(sembol,period,ta.rsi(close,14))
emaKG = request.security(sembol,period,ta.ema(close,emaK))
emaUG = request.security(sembol,period,ta.ema(close,emaU))
ema50 = request.security(sembol,period,ta.ema(close,ema1))
ema100 = request.security(sembol,period,ta.ema(close,ema2))
ema200 = request.security(sembol,period,ta.ema(close,ema3))
sma50 = request.security(sembol,period,ta.sma(close,sma1))
sma100 = request.security(sembol,period,ta.sma(close,sma2))
sma200 = request.security(sembol,period,ta.sma(close,sma3))
atr = request.security(sembol,period,ta.atr(14))
hacim = request.security(sembol,period,volume)
acilis = request.security(sembol,period,open)
kapanis = request.security(sembol,period,close)
fark = request.security(sembol,period,(ta.change(close)/close[1])*100)


plot(emaKG,title="Short EMA", linewidth=2,color=color.green,transp=saydamEma)
plot(emaUG,title="Long EMA", linewidth=2,color=color.red,transp=saydamEma)


if barstate.islast and mobilGoster==false
    g1="Open Price"
    g2="Close Price"
    g3="Change Percent"
    g4="RSI"
    g5="ATR"
    g6="Volume"
    f1="Fib High"
    f2="Fib 0,382"
    f3="Fib 0,50"
    f4="Fib 0,618"
    f5="Fib 0,786"
    f6="Fib Low"
    fi1=str.tostring(math.round_to_mintick(tepe))
    fi2=str.tostring(math.round_to_mintick(fib382))
    fi3=str.tostring(math.round_to_mintick(fib50))
    fi4=str.tostring(math.round_to_mintick(fib618))
    fi5=str.tostring(math.round_to_mintick(fib786))
    fi6=str.tostring(math.round_to_mintick(dip))
    i1="SMA"+str.tostring(sma1)
    i2="SMA"+str.tostring(sma2)
    i3="SMA"+str.tostring(sma3)
    i4="EMA"+str.tostring(ema1)
    i5="EMA"+str.tostring(ema2)
    i6="EMA"+str.tostring(ema3)
    text7=str.tostring(math.round_to_mintick(sma50))
    text8=str.tostring(math.round_to_mintick(sma100))
    text9=str.tostring(math.round_to_mintick(sma200))
    text10=str.tostring(math.round_to_mintick(ema50))
    text11=str.tostring(math.round_to_mintick(ema100))
    text12=str.tostring(math.round_to_mintick(ema200))
    text1 = str.tostring(acilis)
    text2 = str.tostring(kapanis)
    text3 = str.tostring(math.round_to_mintick(fark)) +" %"
    text4 = str.tostring(math.round_to_mintick(rsi))
    text5=str.tostring(math.round_to_mintick(atr))
    text6=str.tostring(hacim)
    table.cell(tablo,0,0,bgcolor=color.black,text_color=color.white, text=g1)
    table.cell(tablo,1,0,bgcolor=color.black,text_color=color.white, text=g2)
    table.cell(tablo,2,0,bgcolor=color.black,text_color=color.white, text=g3)
    table.cell(tablo,3,0,bgcolor=color.black,text_color=color.white, text=g4)
    table.cell(tablo,4,0,bgcolor=color.black,text_color=color.white, text=g5)
    table.cell(tablo,5,0,bgcolor=color.black,text_color=color.white, text=g6)
    table.cell(tablo,0,1,bgcolor=color.black,text_color=color.white, text=text1)
    table.cell(tablo,1,1,bgcolor=color.black,text_color=color.white, text=text2)
    table.cell(tablo,2,1,bgcolor=fark>0?color.green:color.red,text_color=color.white, text=text3)
    table.cell(tablo,3,1,bgcolor=rsi>70?color.green:rsi<30?color.red:color.teal,text_color=color.white, text=text4)
    table.cell(tablo,4,1,bgcolor=color.black,text_color=color.white, text=text5)
    table.cell(tablo,5,1,bgcolor=color.black,text_color=color.white, text=text6)
    table.cell(tablo,0,2,bgcolor=color.gray,text_color=color.white, text=i1)
    table.cell(tablo,1,2,bgcolor=color.gray,text_color=color.white, text=i2)
    table.cell(tablo,2,2,bgcolor=color.gray,text_color=color.white, text=i3)
    table.cell(tablo,3,2,bgcolor=color.gray,text_color=color.white, text=i4)
    table.cell(tablo,4,2,bgcolor=color.gray,text_color=color.white, text=i5)
    table.cell(tablo,5,2,bgcolor=color.gray,text_color=color.white, text=i6)
    table.cell(tablo,0,3,bgcolor=sma50>close?color.red:color.green,text_color=color.white, text=text7)
    table.cell(tablo,1,3,bgcolor=sma100>close?color.red:color.green,text_color=color.white, text=text8)
    table.cell(tablo,2,3,bgcolor=sma200>close?color.red:color.green,text_color=color.white, text=text9)
    table.cell(tablo,3,3,bgcolor=ema50>close?color.red:color.green,text_color=color.white, text=text10)
    table.cell(tablo,4,3,bgcolor=ema100>close?color.red:color.green,text_color=color.white, text=text11)
    table.cell(tablo,5,3,bgcolor=ema200>close?color.red:color.green,text_color=color.white, text=text12)
    
    ////////// fib tablosu
    table.cell(tablo,0,4,bgcolor=color.gray,text_color=color.white, text=f1)
    table.cell(tablo,1,4,bgcolor=color.gray,text_color=color.white, text=f2)
    table.cell(tablo,2,4,bgcolor=color.gray,text_color=color.white, text=f3)
    table.cell(tablo,3,4,bgcolor=color.gray,text_color=color.white, text=f4)
    table.cell(tablo,4,4,bgcolor=color.gray,text_color=color.white, text=f5)
    table.cell(tablo,5,4,bgcolor=color.gray,text_color=color.white, text=f6)
    table.cell(tablo,0,5,bgcolor=tepe>close?color.red:color.green,text_color=color.white, text=fi1)
    table.cell(tablo,1,5,bgcolor=fib382>close?color.red:color.green,text_color=color.white, text=fi2)
    table.cell(tablo,2,5,bgcolor=fib50>close?color.red:color.green,text_color=color.white, text=fi3)
    table.cell(tablo,3,5,bgcolor=fib618>close?color.red:color.green,text_color=color.white, text=fi4)
    table.cell(tablo,4,5,bgcolor=fib786>close?color.red:color.green,text_color=color.white, text=fi5)
    table.cell(tablo,5,5,bgcolor=dip>close?color.red:color.green,text_color=color.white, text=fi6)


    
if barstate.islast and mobilGoster==true
    g1="Open Price"
    g2="Close Price"
    g3="Change Percent"
    g4="RSI"
    g5="ATR"
    g6="Volume"
    text1 = str.tostring(acilis)
    text2 = str.tostring(kapanis)
    text3 = str.tostring(math.round_to_mintick(fark)) +" %"
    text4 = str.tostring(math.round_to_mintick(rsi))
    text5=str.tostring(math.round_to_mintick(atr))
    text6=str.tostring(hacim)    
    table.cell(mobiletablo,0,0,bgcolor=color.black,text_color=color.white, text=g1)
    table.cell(mobiletablo,0,1,bgcolor=color.black,text_color=color.white, text=g2)
    table.cell(mobiletablo,0,2,bgcolor=color.black,text_color=color.white, text=g3)
    table.cell(mobiletablo,0,3,bgcolor=color.black,text_color=color.white, text=g4)
    table.cell(mobiletablo,0,4,bgcolor=color.black,text_color=color.white, text=g5)
    table.cell(mobiletablo,0,5,bgcolor=color.black,text_color=color.white, text=g6)
    
    table.cell(mobiletablo,1,0,bgcolor=color.black,text_color=color.white, text=text1)
    table.cell(mobiletablo,1,1,bgcolor=color.black,text_color=color.white, text=text2)
    table.cell(mobiletablo,1,2,bgcolor=color.black,text_color=color.white, text=text3)
    table.cell(mobiletablo,1,3,bgcolor=rsi>70?color.green:rsi<30?color.red:color.teal,text_color=color.white, text=text4)
    table.cell(mobiletablo,1,4,bgcolor=color.black,text_color=color.white, text=text5)
    table.cell(mobiletablo,1,5,bgcolor=color.black,text_color=color.white, text=text6)


