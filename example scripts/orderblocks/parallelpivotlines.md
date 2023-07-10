// This work is licensed under a Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0) https://creativecommons.org/licenses/by-nc-sa/4.0/
// © LuxAlgo

//@version=4
study("Parallel Pivot Lines [LuxAlgo]",overlay=true,max_bars_back=5000)
//Basic
length   = input(30,group='Basic Settings'
  ,tooltip='Pivot length. Use higher values for having lines connected to more significant pivots')
lookback = input(3,minval=1,group='Basic Settings'
  ,tooltip='Number of lines connecting a pivot high/low to display')
Slope    = input(1.,minval=-1,maxval=1,step=0.1,group='Basic Settings'
  ,tooltip='Allows to multiply the linear regression slope by a number within -1 and 1')

//Style
ph_col = input(#2157f3,'Pivot High Lines Color',group='Line Colors')
pl_col = input(#ff1100,'Pivot Low Lines Color',group='Line Colors')

//──────────────────────────────────────────────────────────────────────────────
Sma(src,p) => a = cum(src), (a - a[max(p,0)])/max(p,0)
Variance(src,p) => p == 1 ? 0 : Sma(src*src,p) - pow(Sma(src,p),2)
Covariance(x,y,p) => Sma(x*y,p) - Sma(x,p)*Sma(y,p)
//──────────────────────────────────────────────────────────────────────────────
n = bar_index
ph = pivothigh(length,length)
pl = pivotlow(length,length)
//──────────────────────────────────────────────────────────────────────────────
varip ph_array = array.new_float(0)
varip pl_array = array.new_float(0)
varip ph_n_array = array.new_int(0)
varip pl_n_array = array.new_int(0)
if ph
    array.insert(ph_array,0,ph)
    array.insert(ph_n_array,0,n)
if pl
    array.insert(pl_array,0,pl)
    array.insert(pl_n_array,0,n)
//──────────────────────────────────────────────────────────────────────────────
val_ph = valuewhen(ph,n-length,lookback-1)
val_pl = valuewhen(pl,n-length,lookback-1)
val = min(val_ph,val_pl)
k = n - val > 0 ? n - val : 2
slope = Covariance(close,n,k)/Variance(n,k)*Slope
var line ph_l = na,var line pl_l = na
if barstate.islast
    for i = 0 to lookback-1
        ph_y2 = array.get(ph_array,i),ph_x1 = array.get(ph_n_array,i)-length
        pl_y2 = array.get(pl_array,i),pl_x1 = array.get(pl_n_array,i)-length
        ph_l := line.new(ph_x1,ph_y2,ph_x1+1,ph_y2+slope,extend=extend.right,color=ph_col)
        pl_l := line.new(pl_x1,pl_y2,pl_x1+1,pl_y2+slope,extend=extend.right,color=pl_col)