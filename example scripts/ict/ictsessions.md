//@version=2
// Created By BryceWH
// Modified by @leonsholo
// Plots London Open Killzone and New York Open KZ as overlay boxes using current daily high / lows
// Modification adds the Asian Session and London Close session.
// Fills can be turned on and off. Created this indicator because i didnt like highlighting the whole chart background as seen in other ICT KZ indicators on tradingview and wanted something cleaner.
// If you want additional killzones such as london/new york close add the indicator to the chart twice.
// Adapted from Chris Moody's original indicator HLOC
study(title="ICT Killzone", shorttitle="ICT Killzone", overlay=true)
st = true
shl = input(true, title="Show High / Low")
londonkz = input(title="KillZone London Open", type=session, defval="0100-0500")
newyorkkz = input(title="KillZone NY Open", type=session, defval="0700-1000")
asiakz = input(title="KillZone Asian Range", type=session, defval="2015-0000")
londonckz = input(title="KillZone London Close", type=session, defval="1000-1200")
colourcheck = 1.0
boxheight = input(title="Box Height", type=float, defval=2.0)
fillcheck = input(true, title="Fill Middle")


ph = security(tickerid, 'D', high)
pl = security(tickerid, 'D', low)

dayrange = ph - pl
high2 = ph + (dayrange * 0.01 * boxheight)
low2 = pl - (dayrange * 0.01 * boxheight)

BarInSession(sess) => time(period, sess) != 0

lineColour = colourcheck == 1 ? #E1BC29 : colourcheck == 2 ? #3BB273 : na // box colour
lineColour2 = colourcheck == 2 ? #E1BC29 : colourcheck == 1 ? #3BB273 : na // box colour
lineColour3 = colourcheck == 2 and fillcheck ? #E1BC29 : colourcheck == 1 and fillcheck ? #3BB273 : white // box colour
lineColour4 = colourcheck == 1 and fillcheck ? #E1BC29 : colourcheck == 2 and fillcheck ? #3BB273 : white // box colour

//DAILY

v5=plot(shl and ph ? ph : na, title="Daily High", style=circles, linewidth=2, color=gray) // daily high low plots
v6=plot(shl and pl ? pl : na, title="Daily Low", style=circles, linewidth=2, color=gray) // daily high low plots

//LONDON
varhigh2=plot(st and ph and BarInSession(londonkz) ? high2 : na, title="London Killzone High", style=linebr, linewidth=2, color=na) // change color=na to color to make these lines visible/editable
varhigh=plot(st and ph and BarInSession(londonkz) ? ph : na, title="London KillzoneLow", style=linebr, linewidth=2, color=na) // change color=na to color to make these lines visible/editable
varlow=plot(st and pl and BarInSession(londonkz) ? pl : na, title="London Killzone High", style=linebr, linewidth=2, color=na) // change color=na to color to make these lines visible/editable
varlow2=plot(st and pl and BarInSession(londonkz) ? low2 : na, title="London Killzone Low", style=linebr, linewidth=2, color=na) // change color=na to color to make these lines visible/editable

fill(varhigh,varhigh2,color=lineColour, title="London High Box", transp=25) // box 1 top fill
fill(varhigh,varlow,color=lineColour4, title="London Fill Middle", transp=75) // fill between killzone boxes
fill(varlow,varlow2,color=lineColour, title="London Low Box", transp=25) // box 2 top fill

//NEW YORK
v1=plot(st and ph and BarInSession(newyorkkz) ? high2 : na, title="New York Killzone High", style=linebr, linewidth=2, color=na)
v2=plot(st and ph and BarInSession(newyorkkz) ? ph : na, title="New York Killzone Low", style=linebr, linewidth=2, color=na)
v3=plot(st and pl and BarInSession(newyorkkz) ? pl : na, title="New York Killzone High", style=linebr, linewidth=2, color=na)
v4=plot(st and pl and BarInSession(newyorkkz) ? low2 : na, title="New York Killzone Low", style=linebr, linewidth=2, color=na)

fill(v1,v2,color=lineColour2, title="New York High Box", transp=25)
fill(v2,v3,color=lineColour3, title="NewYork Fill Middle", transp=85)
fill(v3,v4,color=lineColour2, title="New York Low Box", transp=25)

//ASIA
var6=plot(st and ph and BarInSession(asiakz) ? high2 : na, title="Asia Zone High", style=linebr, linewidth=2, color=na)
var7=plot(st and ph and BarInSession(asiakz) ? ph : na, title="Asia Zone Low", style=linebr, linewidth=2, color=na)
var8=plot(st and pl and BarInSession(asiakz) ? pl : na, title="Asia Zone High", style=linebr, linewidth=2, color=na)
var9=plot(st and pl and BarInSession(asiakz) ? low2 : na, title="Asia Zone Low", style=linebr, linewidth=2, color=na)


fill(var6,var7,color=lineColour2, title="Asia High Box", transp=25)
fill(var7,var8,color=lineColour3, title="Asia Fill Middle", transp=85)
fill(var8,var9,color=lineColour2, title="Asia Low Box", transp=25)

//LONDONCLOSE
vari61=plot(st and ph and BarInSession(londonckz) ? high2 : na, title="London Close High", style=linebr, linewidth=2, color=na)
vari71=plot(st and ph and BarInSession(londonckz) ? ph : na, title="London Close Low", style=linebr, linewidth=2, color=na)
vari81=plot(st and pl and BarInSession(londonckz) ? pl : na, title="London Close High", style=linebr, linewidth=2, color=na)
vari91=plot(st and pl and BarInSession(londonckz) ? low2 : na, title="London Close Low", style=linebr, linewidth=2, color=na)

fill(vari61,vari71,color=lineColour2, title="London Close High Box", transp=25)
fill(vari71,vari81,color=lineColour3, title="London Close Fill Middle", transp=85)
fill(vari81,vari91,color=lineColour2, title="London Close Low Box", transp=25)





