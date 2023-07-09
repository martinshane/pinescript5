//@version=5
indicator("First Candlestick and Opening Gap", overlay = true, shorttitle="First Candlestick and Opening Gap", max_boxes_count = 500, max_lines_count = 500)

var line highLine = na
var line lowLine = na
var label dateLabel = na

show = input.string('NWOG', 'Show', options = ['NWOG', 'NDOG'])
showLast = input.int(5, 'Amount', minval = 1)

ogCssBull = input(#5b9cf6, 'Gaps Colors', inline = 'ogcolor', group = 'Style')
ogCssBear = input(#ba68c8, '' , inline = 'ogcolor', group = 'Style')

type ogaps
    float[] top
    float[] btm
    int[]   loc
    box[]   boxes
    line[]  toplines
    line[]  btmlines

method set_area(ogaps id, max, min, css)=>
    id.boxes.unshift(
      box.new(bar_index, max, bar_index+1, min, na, bgcolor = color.new(css, 95), extend = extend.right, xloc = xloc.bar_time))
    
    id.toplines.unshift(
      line.new(bar_index, max, bar_index+1, max, color = color.new(css, 50), extend = extend.right, xloc = xloc.bar_time))
    id.btmlines.unshift(
      line.new(bar_index, min, bar_index+1, min, color = color.new(css, 50), extend = extend.right, xloc = xloc.bar_time))

method pop(ogaps id)=>
    if id.boxes.size() > showLast
        id.top.pop(),id.btm.pop(),id.loc.pop()
        id.boxes.pop().delete(),id.toplines.pop().delete()
        id.btmlines.pop().delete()

var ogaps_ = ogaps.new(array.new_float(0)
  , array.new_float(0)
  , array.new_int(0)
  , array.new_box(0)
  , array.new_line(0)
  , array.new_line(0))

var tf = show == 'NWOG' ? 'W' : 'D'
dtf = timeframe.change(tf)

// Calculate the First Candlestick
if (ta.change(time("D")) != 0)
    if (na(highLine))
        highLine := line.new(x1 = bar_index, y1 = high, x2 = bar_index, y2 = high, color=color.green, width = 1)
        lowLine := line.new(x1 = bar_index, y1 = low, x2 = bar_index, y2 = low, color=color.red, width = 1)
        dateLabel := label.new(x = bar_index, y = high, text = str.tostring(dayofmonth(time)), color=color.green, style = label.style_label_down)
    else
        line.delete(highLine)
        line.delete(lowLine)
        label.delete(dateLabel)
        highLine := line.new(x1 = bar_index, y1 = high, x2 = bar_index, y2 = high, color=color.rgb(255, 255, 255), width = 2)
        lowLine := line.new(x1 = bar_index, y1 = low, x2 = bar_index, y2 = low, color=color.rgb(255, 255, 255), width = 2)
        dateLabel := label.new(x = bar_index, y = high, text = str.tostring("1st Candlestick"), color=color.rgb(255, 255, 255), style = label.style_label_down)

if (not na(highLine))
    line.set_x2(highLine, bar_index)
    line.set_x2(lowLine, bar_index)
    label.set_x(dateLabel, bar_index)

//Detects opening gaps and set boxes
if dtf
    max = math.max(close[1], open)
    min = math.min(close[1], open)

    ogaps_.top.unshift(max)
    ogaps_.btm.unshift(min)
    ogaps_.loc.unshift(bar_index)
    
    css = open > close[1] ? ogCssBull : ogCssBear
    ogaps_.set_area(max, min, css)
    
    ogaps_.pop()
