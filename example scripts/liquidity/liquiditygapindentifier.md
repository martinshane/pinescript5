
// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © Zeke2209

//@version=5
indicator("LIQUIDITY GAP IDENTIFIER", overlay = true, max_boxes_count = 500)

candle_1_L = low[2]
candle_1_H = high[2]
candle_3_L = low
candle_3_H = high

candle_1_INDEX = bar_index[2]
candle_3_INDEX = bar_index


if (((candle_3_L - candle_1_H) > 0) and (candle_3_H - candle_1_L) > 0)
    box.new(candle_1_INDEX, candle_3_L, candle_3_INDEX+1, candle_1_H, bgcolor = color.new(color.silver, 60), border_color = color.green)
    
if(((candle_1_L - candle_3_H) > 0) and (candle_3_L - candle_1_L) < 0)
    box.new(candle_1_INDEX, candle_1_L, candle_3_INDEX+1, candle_3_H, bgcolor = color.new(color.silver, 60), border_color = color.red)
    
    
    
//plot(h, color = color.green) 
//plot(l, color = color.red)