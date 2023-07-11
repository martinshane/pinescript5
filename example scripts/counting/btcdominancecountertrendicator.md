// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © TheGrindToday

//@version=4
study("BTC Dominance",overlay = false)

btc_close = nz(security("binance:BTCUSDT", timeframe.period, close))
btc_open = nz(security("binance:BTCUSDT", timeframe.period, open))

n = 100
int count = 0

for i = 0 to n-1
    if(btc_close[n-i] > btc_open[n-i] and close[n-i] > open[n-i] or btc_close[n-i] <= btc_open[n-i] and close[n-i] <= open[n-i])
        count := count +1

bool higher = true
if (btc_close[0] > btc_open[0] and close[0] < open[0])
    higher := false
if (btc_close <= btc_open and close[0] >= open[0])
    higher := true

backgroundColour = color.white

if (higher)
    backgroundColour := color.lime
if (not higher)
    backgroundColour := color.red

if(btc_close[0] > btc_open[0] and close[0] > open[0] or btc_close[0] <= btc_open[0] and close[0] <= open[0])
    backgroundColour := color.white

bgcolor(color=backgroundColour, transp=85)

data = btc_close >= btc_open
plotshape(data, color=color.lime, style=shape.arrowup)
plotshape(not data, color=color.red, style=shape.arrowdown)
plot(count)



