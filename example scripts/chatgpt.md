//@version=5
indicator("Fair Value Gap + Liquidity Swings [LuxAlgo]", overlay = true, max_lines_count = 500)

//------------------------------------------------------------------------------
//Settings
//-----------------------------------------------------------------------------{
threshold_per = input.float(0, "Percentage", minval = 0)

auto = input(false, "Auto Threshold")

discard_len = input(5, "Show Last Unfilled Gaps")

show_all = input(false, "Show All Unfilled Gaps")

tf = input.timeframe('', "Timeframe")

//Style
bull_css = input.color(color.new(#0cb51a, 50), "Bullish FVG", group = 'Style')

bear_css = input.color(color.new(#ff1100, 50), "Bearish FVG", group = 'Style')

show_dash = input(true, "Show Dashboard", group = 'Style')

//Liquidity Swings Settings
length = input(14, 'Pivot Lookback')

area = input.string('Wick Extremity', 'Swing Area', options = ['Wick Extremity', 'Full Range'])

intraPrecision = input(false, 'Intrabar Precision', inline = 'intrabar')
intrabarTf = input.timeframe('1', ''              , inline = 'intrabar')

filterOptions = input.string('Count', 'Filter Areas By', options = ['Count', 'Volume'], inline = 'filter')
filterValue   = input.float(0, ''                                            , inline = 'filter')

//Style
showTop      = input(true, 'Swing High'              , inline = 'top', group = 'Style')
topCss       = input(color.red, ''                   , inline = 'top', group = 'Style')
topAreaCss   = input(color.new(color.red, 50), 'Area', inline = 'top', group = 'Style')

showBtm      = input(true, 'Swing Low'                , inline = 'btm', group = 'Style')
btmCss       = input(color.teal, ''                   , inline = 'btm', group = 'Style')
btmAreaCss   = input(color.new(color.teal, 50), 'Area', inline = 'btm', group = 'Style')

labelSize = input.string('Tiny', 'Labels Size', options = ['Tiny', 'Small', 'Normal'], group = 'Style')

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

[src_c1, src_o1, src_h, src_l, src_h2, src_l2] = request.security(syminfo.tickerid, tf, get_ohlc())

delta_per = (src_c1 - src_o1) / src_o1 * 100

threshold = auto ? ta.cum(math.abs(change_tf ? delta_per : 0)) / n * 2 : threshold_per

//-----------------------------------------------------------------------------}
//Detection/Display Function
//-----------------------------------------------------------------------------}

def detect_display_fvg(fvg_condition, fvg_type, fvg_lines, discarded_fvg_lines, css): 

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

  idx = 0
  for l in discarded_fvg_lines
    line.set_x2(l, n)

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
  fill_x1 := filled == 0
      ? n
      : n

  [fvg_max, fvg_min, filled, duration, total, filled_cnd]

//Bullish FVG
//-----------------------------------------------------------------------------{
bull_fvg_max
, bull_fvg_min
, bull_filled
, bull_duration
, bull_total
, bull_filled_cnd] = detect_display_fvg(bullish_fvg_cnd
, "bullish"
, bull_fvg_lines
, discarded_bull_fvg_lines
, bull_css)

//Bearish FVG
//-----------------------------------------------------------------------------{
bear_fvg_max
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

  table.cell(tb, 0, 3, "Total")

//Table data
if barstate.isfirst and show_dash
  table.cell(tb, 1, 1, bull_filled / bull_total * 100
    , text_color = bull_css
    , text_halign = text.align_right)

  table.cell(tb, 1, 2, bull_duration / bull_filled
    , text_color = bull_css
    , text_halign = text.align_right)

  table.cell(tb, 1, 3, bull_total)

  table.cell(tb, 2, 1, bear_filled / bear_total * 100
    , text_color = bear_css
    , text_halign = text.align_right)

  table.cell(tb, 2, 2, bear_duration / bear_filled
    , text_color = bear_css
    , text_halign = text.align_right)

  table.cell(tb, 2, 3, bear_total)

  table.draw()

//Display FVG's
//-----------------------------------------------------------------------------{
if bull_filled > 0
  plot(bull_fvg_lines)

if bear_filled > 0
  plot(bear_fvg_lines)

//Debug
//-----------------------------------------------------------------------------{
if show_dash
  print(f"Bullish FVG: {bull_filled}/{bull_total}")
  print(f"Bearish FVG: {bear_filled}/{bear_total}")

  print(f"Bullish FVG Max: {bull_fvg_max}")
  print(f"Bullish FVG Min: {bull_fvg_min}")
  print(f"Bearish FVG Max: {bear_fvg_max}")
  print(f"Bearish FVG Min: {bear_fvg_min}")

//------------------------------------------------------------------------------}
