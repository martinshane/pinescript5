// pivot script i take from tradingview inbuild script 
// to code my idea help  by @fikira  

//@version=4
study("count pivot high/low", shorttitle="count HL", overlay=true, max_labels_count=500)

t = time('D')
gr="LENGTH LEFT / RIGHT"

leftLenH = input(title="Pivot High", type=input.integer, defval=10, minval=1, inline="Pivot High",group=gr) 
rightLenH = input(title="/", type=input.integer, defval=10, minval=1, inline="Pivot High",group=gr) 

 
leftLenL = input(title="Pivot Low", type=input.integer, defval=10, minval=1, inline="Pivot Low", group=gr) 
rightLenL = input(title="/", type=input.integer, defval=10, minval=1, inline="Pivot Low",group=gr) 

 
ph = pivothigh(leftLenH, rightLenH) 
pl = pivotlow(leftLenL, rightLenL) 
 
vwph = valuewhen(ph, ph, 0) 
vwpl = valuewhen(pl, pl, 0) 
 
counterph = 0 
counterph := change(vwph) ? nz(counterph[1]) + 1 : counterph[1] 
plot(counterph) 
counterpl=0 
counterpl := change(vwpl) ? nz(counterpl[1]) + 1  : counterpl[1] 
plot(counterpl) 
 
if ph 
    label.new(bar_index[rightLenH], ph, tostring(counterph), style=label.style_label_down, color=color.red, textcolor=color.white) 

if pl
    label.new(bar_index[leftLenL], pl, tostring(counterpl), style=label.style_label_up, color=color.green, textcolor=color.white) 