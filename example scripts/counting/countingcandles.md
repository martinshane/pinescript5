//@version=3
study("counting candles v2", overlay=false)

length = input(type=integer, defval=10, title="candles back")

fun_red_count(length) =>
    red_count = 0
    green_count = 0
    for i=1 to length
        if (close[i] < open[i])
            red_count := red_count + 1
        if (close[i] > open[i])
            green_count := green_count + 1
    [red_count, green_count]

[red_data, green_data] = fun_red_count(length)

plot(red_data, color=red)
plot(green_data, color=green)