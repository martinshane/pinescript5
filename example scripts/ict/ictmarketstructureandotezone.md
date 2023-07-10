// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © xflorex

//@version=4
study("ICT Market Structure and OTE Zone", shorttitle="ICT MS & OTE", overlay=true)

// Parameters
fib_lower = input(0.618, title="Fibonacci Lower Level", type=input.float)
fib_upper = input(0.786, title="Fibonacci Upper Level", type=input.float)

// Calculate Daily Highs and Lows
daily_high = security(syminfo.tickerid, "D", high[1])
daily_low = security(syminfo.tickerid, "D", low[1])

// Define OTE Zone
ote_upper = daily_high - (daily_high - daily_low) * fib_lower
ote_lower = daily_high - (daily_high - daily_low) * fib_upper

// Plot Daily Highs, Lows, and OTE Zone
line.new(x1=bar_index[1], y1=daily_high, x2=bar_index, y2=daily_high, color=color.red, width=1, extend=extend.right)
line.new(x1=bar_index[1], y1=daily_low, x2=bar_index, y2=daily_low, color=color.green, width=1, extend=extend.right)
ote_upper_plot = plot(ote_upper, color=color.gray, linewidth=2, title="OTE Upper", trackprice=false)
ote_lower_plot = plot(ote_lower, color=color.gray, linewidth=2, title="OTE Lower", trackprice=false)
fill(ote_upper_plot, ote_lower_plot, color=color.silver, transp=80)

// Notes
// The red line represents the daily high, while the green line represents the daily low.
// The gray area represents the optimal trade entry (OTE) zone based on Fibonacci retracement levels.





