//@version=5
indicator('Green/Red Bar Count and Longest Sequence', overlay=true)
initialPosition = input.int(title="Initial Position", defval = 1)
numberOfCandles = input.int(title="Number of Candles", defval = 120) + initialPosition
var tbl = table.new(position.top_right, 3, 3)

prev = 'n'

gcount = 0
gmax = 0
gpartial = 0

rcount = 0
rmax = 0
rpartial = 0

for i = initialPosition to numberOfCandles by 1
    if open[i] < close[i]
        gcount += 1
        if prev == 'g'
            gpartial += 1
        else
            gpartial := 1
        prev := 'g'
        gmax := gpartial > gmax ? gpartial : gmax
    else if open[i] > close[i]
        rcount += 1
        if prev == 'r'
            rpartial += 1
        else
            rpartial := 1
        prev := 'r'
        rmax := rpartial > rmax ? rpartial : rmax
    else
        if prev == 'r'
            rpartial += 1
            rmax := rpartial > rmax ? rpartial : rmax
        if prev == 'g'
            gpartial += 1
            rmax := rpartial > rmax ? rpartial : rmax

table.cell(tbl, 0, 0, '', bgcolor=#808080, width=7, height=4)
table.cell(tbl, 1, 0, 'Green', bgcolor=#aaaaaa, width=7, height=4)
table.cell(tbl, 2, 0, 'RED', bgcolor=#aaaaaa, width=7, height=4)

table.cell(tbl, 0, 1, 'Count', bgcolor=#aaaaaa, width=7, height=4)
table.cell(tbl, 1, 1, str.tostring(gcount), bgcolor=color.green, width=7, height=4)
table.cell(tbl, 2, 1, str.tostring(rcount), bgcolor=color.red, width=7, height=4)

table.cell(tbl, 0, 2, 'Longest Sequence', bgcolor=#aaaaaa, width=7, height=4)
table.cell(tbl, 1, 2, str.tostring(gmax), bgcolor=color.green, width=7, height=4)
table.cell(tbl, 2, 2, str.tostring(rmax), bgcolor=color.red, width=7, height=4)
