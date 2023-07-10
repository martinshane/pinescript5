// This work is licensed under a Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0) https://creativecommons.org/licenses/by-nc-sa/4.0/
// © LuxAlgo

//@version=5
indicator("Fair Value Gap [LuxAlgo]", overlay = true, max_lines_count = 500)
//------------------------------------------------------------------------------
//Settings
//-----------------------------------------------------------------------------{
threshold_per = input.float(0, "Percentage"
  , minval = 0)
  
auto = input(false, "Auto Threshold")

discard_len = input(5, "Show Last Unfilled Gaps")

show_all = input(false, "Show All Unfilled Gaps")

tf = input.timeframe('', "Timeframe")

//Style
bull_css = input.color(color.new(#0cb51a, 50), "Bullish FVG"
  , group = 'Style')

bear_css = input.color(color.new(#ff1100, 50), "Bearish FVG"
  , group = 'Style')

show_dash = input(true, "Show Dashboard"
  , group = 'Style')

//-----------------------------------------------------------------------------}
//Global variables
//-----------------------------------------------------------------------------{
var bull_fvg_lines = array.new_line(0)
var discarded_bull_fvg_lines = array.new_line(0)

var bear_fvg_lines = array.new_line(0)
var discarded_bear_fvg_lines = array.new_line(0)

n = bar_index

change_tf = timeframe.change(tf)

get_ohlc()=> [close[1], open[1], high, low, high[2], low[2]]

[src_c1, src_o1, src_h, src_l, src_h2, src_l2] =
  request.security(syminfo.tickerid, tf, get_ohlc())

delta_per = (src_c1 - src_o1) / src_o1 * 100

threshold = auto ? ta.cum(math.abs(change_tf ? delta_per : 0)) / n * 2 
  : threshold_per

//-----------------------------------------------------------------------------}
//Detection/Display Function
//-----------------------------------------------------------------------------{
detect_display_fvg(fvg_condition
  , fvg_type
  , fvg_lines
  , discarded_fvg_lines
  , css)=>
  
    var float fvg_max = na, var float fvg_min = na
    var int filled = 0    , var int total = 0
    var int fill_x1 = 0   , var int duration = 0
    
    //Check if historical FVG's are filled
    idx = 0
    for l in fvg_lines
        line.set_x2(l, n)
        
        turn_filled_cnd = fvg_type == "bullish" 
          ? src_l <= line.get_y2(l)
          : src_h >= line.get_y2(l)
          
        if turn_filled_cnd
            array.remove(fvg_lines, idx)
            
            filled += 1
            duration += n - line.get_x1(l)
            
        idx += 1
    
    idx := 0
    for l in discarded_fvg_lines
        turn_filled_cnd = fvg_type == "bullish" 
          ? src_l <= line.get_y2(l)
          : src_h >= line.get_y2(l)
        
        if turn_filled_cnd
            array.remove(discarded_fvg_lines, idx)
            
            filled += 1
            duration += n - line.get_x1(l)
            
        idx += 1
    
    //Test for FVG
    if fvg_condition
        
        //If FVG is unfilled store line in array
        if fvg_max[1] != fvg_min[1]
        
            level = fvg_type == "bullish" ? fvg_min : fvg_max
            
            array.unshift(fvg_lines
              , line.new(n - 1, level, n, level
              , color = css
              , style = line.style_dashed))
            
            if not show_all
                if array.size(fvg_lines) > discard_len
                    pop_l = array.pop(fvg_lines)
                    array.push(discarded_fvg_lines, pop_l)
        else 
            filled += 1
            duration += n - fill_x1
        
        fvg_max := fvg_type == "bullish" ? src_l : src_l2
        fvg_min := fvg_type == "bullish" ? src_h2 : src_h
        
        total += 1
        fill_x1 := n
        
    fvg_max := fvg_type == "bullish" 
      ? math.max(math.min(fvg_max, src_l), fvg_min)
      : fvg_max
    
    fvg_min := fvg_type == "bearish" 
      ? math.min(math.max(fvg_min, src_h), fvg_max)
      : fvg_min
    
    filled_cnd = fvg_max == fvg_min and fvg_max[1] != fvg_min
    
    fill_x1 := filled_cnd
      ? n
      : fill_x1
      
    [fvg_max, fvg_min, filled, duration, total, filled_cnd]
    
//-----------------------------------------------------------------------------}
//Bullish FVG
//-----------------------------------------------------------------------------{
//Bullish FVG Condition
bullish_fvg_cnd = src_l > src_h2
  and src_c1 > src_h2 
  and delta_per > threshold
  and change_tf

[bull_fvg_max
  , bull_fvg_min
  , bull_filled
  , bull_duration
  , bull_total
  , bull_filled_cnd] = detect_display_fvg(bullish_fvg_cnd
  , "bullish"
  , bull_fvg_lines
  , discarded_bull_fvg_lines
  , bull_css)
  
bearish_fvg_cnd = src_h < src_l2 
  and src_c1 < src_l2 
  and -delta_per > threshold
  and change_tf

[bear_fvg_max
  , bear_fvg_min
  , bear_filled
  , bear_duration
  , bear_total
  , bear_filled_cnd] = detect_display_fvg(bearish_fvg_cnd
  , "bearish"
  , bear_fvg_lines
  , discarded_bear_fvg_lines
  , bear_css)
  
//-----------------------------------------------------------------------------}
//Dashboard
//-----------------------------------------------------------------------------{
var tb = table.new(position.top_right, 3, 3)

//Table labels
if barstate.isfirst and show_dash
    table.cell(tb, 0, 1, "Filled %"
      , text_color = chart.fg_color
      , text_halign = text.align_right)
    
    table.cell(tb, 0, 2, "Avg Duration"
      , text_color = chart.fg_color
      , text_halign = text.align_right)
    
    table.cell(tb, 1, 0, "Bullish"
      , text_color = chart.fg_color)
    
    table.cell(tb, 2, 0, "Bearish"
      , text_color = chart.fg_color)

if barstate.islast and show_dash
    //Filled FVG stats
    table.cell(tb, 1, 1
      , str.format("{0, number, percent}", bull_filled / bull_total)
      , text_color = chart.fg_color)
    
    table.cell(tb, 2, 1
      , str.format("{0, number, percent}", bear_filled / bear_total)
      , text_color = chart.fg_color)
    
    //FVG duration stats
    table.cell(tb, 1, 2
      , str.format("{0,number,integer}", bull_duration / bull_filled)
      , text_color = chart.fg_color)
    
    table.cell(tb, 2, 2
      , str.format("{0,number,integer}", bear_duration / bear_filled)
      , text_color = chart.fg_color)

//-----------------------------------------------------------------------------}
//Plots
//-----------------------------------------------------------------------------{
//Bullish FVG
bull_max_plot = plot(bull_fvg_max, "Bullish FVG Maximum", na)
bull_min_plot = plot(bull_fvg_min, "Bullish FVG Minimum", na)

fill(bull_max_plot, bull_min_plot, bullish_fvg_cnd ? na : bull_css, "Bullish FVG")

//Bearish FVG
bear_max_plot = plot(bear_fvg_max, "Bearish FVG Maximum", na)
bear_min_plot = plot(bear_fvg_min, "Bearish FVG Minimum", na)

fill(bear_max_plot, bear_min_plot, bearish_fvg_cnd ? na : bear_css)

//-----------------------------------------------------------------------------}
//Alerts
//-----------------------------------------------------------------------------{
alertcondition(bull_filled_cnd, "Bullish FVG", "Recent bullish FVG filled")

alertcondition(bear_filled_cnd, "Bearish FVG", "Recent bearish FVG filled")

//-----------------------------------------------------------------------------}