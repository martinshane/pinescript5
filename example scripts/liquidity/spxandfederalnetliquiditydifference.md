// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © TaxShields
// Fed Net Liquidity indicator from © jlb05013
//@version=5
indicator("Fed Net Liquidity Indicator", overlay=false)

i_res = input.timeframe('D', "Resolution", options=['D', 'W', 'M'])

fed_bal = request.security('FRED:WALCL', i_res, close)/1e9 // millions
tga = request.security('FRED:WTREGEN', i_res, close)/1e9 // billions
rev_repo = request.security('FRED:RRPONTSYD', i_res, close)/1e9 // billions

//plot(fed_bal)
//plot(tga)
//plot(rev_repo)

net_liquidity = (fed_bal - (tga + rev_repo)) / 1.1 - 1625

Difference = close - net_liquidity
plot(Difference, color=color.red)

plot(250, style=plot.style_line)
plot(-250, style=plot.style_line)