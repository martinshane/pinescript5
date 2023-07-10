// This work is licensed under a Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0) https://creativecommons.org/licenses/by-nc-sa/4.0/
// © LuxAlgo

//@version=5
indicator("Volume Footprint [LuxAlgo]",overlay=true,max_boxes_count=500,max_lines_count=500)
method  = input.string('Atr','Interval Size Method',options=['Atr','Manual'],inline='a',confirm=true)
length  = input.float(14,'',inline='a',confirm=true)
percent = input.bool(false,'As Percentage',confirm=true)

look = input.string('Candle','Display Type', options=['Candle','Regular','Gradient'],group='Style',confirm=true)
bull = input.color(#089981,'Trend Color',inline='b',confirm=true)
bear = input.color(#f23645,'',inline='b',confirm=true)

col_a = input.color(#bbd9fb,'Gradient Box Color',inline='c',confirm=true)
col_b = input.color(#0c3299,'',inline='c',confirm=true)

reg_col = input.color(#bbd9fb,'Regular Box Color',confirm=true)
//----
varip prices = ''
varip deltas = ''
varip delta = 0. 
varip prev = 0. 
//----
r = high-low
atr = ta.atr(math.round(length))
size = method == 'Atr' ? atr : length
k = math.round(r/size) + 1

split_prices = str.split(prices,',')
split_deltas = str.split(deltas,',')
//----
n = bar_index
if barstate.isconfirmed
    if array.size(split_prices) > 0
        for i = 1 to k
            top = low + i/k*r
            btm = low + (i-1)/k*r
            
            sum = 0.
            for j = 0 to array.size(split_prices)-1
                value = str.tonumber(array.get(split_prices,j))
                d = str.tonumber(array.get(split_deltas,j))
                sum := value < top and value >= btm ? sum + d : sum
            
            color bgcolor = na
            color border_color = na
            color text_color = na
            
            if look == 'Candle'
                bgcolor := color.new(close > open ? bull : bear,50)
                border_color := close > open ? bull : bear
                text_color := close > open ? bull : bear
            else if look == 'Gradient'
                bgcolor := color.from_gradient(sum,0,volume,col_a,col_b)
                border_color := bgcolor
            else
                bgcolor := reg_col
                border_color := color.gray
            
            txt = percent ? str.tostring(sum/volume*100,format.percent) : str.tostring(sum,'#.#####')
            if look != 'Candle'
                line.new(n,high,n,low,color=close > open ? bull : bear,width=3)
            box.new(n,top,n+1,btm,text = txt,text_color=color.gray,
              bgcolor=bgcolor,border_color=border_color)

array.clear(split_prices)
array.clear(split_deltas)
//----
if barstate.isnew
    delta := 0
    prev := 0
    deltas := ''
    prices := ''
else if barstate.islast
    delta := volume - prev
    prev := volume
    deltas += str.tostring(delta) + ','
    prices += str.tostring(close) + ','
