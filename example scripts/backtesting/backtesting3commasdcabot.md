// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © rouxam

//@version=4
// Author: rouxam
strategy("Backtesting 3commas DCA Bot v2", overlay=true, pyramiding=99, process_orders_on_close=true, commission_type=strategy.commission.percent, commission_value=0.1, initial_capital=3009)

// Important Note : Initial Capital above is written to match the default value based on bot parameters.
//                  It is expected the user will update the initial_capital via the GUI Parameters > Properties tab
//                  A warning will displayed in case initial capital and max amount for bot usage do not match.

// ----------------------------------
// Strategy Inputs
// ----------------------------------
take_profit             = input(4.5, type=input.float,  title="Take Profit (%)", minval=0.0, step=0.1)/100
take_profit_type        = input(defval="% from total volume", title="Take Profit Type", options=["% from total volume", "% from base order"])
ttp                     = input(0.0, type=input.float,  title="Trailing Take Profit (%) [0 = Disabled]", minval=0.0, step=0.1)/100
stop_loss               = input(0.0, type=input.float,  title="Stop Loss (%) [0 = Disabled]", minval=0.0, step=0.1)/100
order_size_type         = input(defval="Fixed",         title="Order Size Type", options=["Fixed", "% of equity"])
base_order              = input(100.0, type=input.float,title="Base Order") 
safe_order              = input(140.0, type=input.float,title="Safety Order") 
max_safe_order          = input(6,                      title="Max Safety Trades Count", minval=0, maxval=99, step=1) 
price_deviation         = input(1.5, type=input.float,  title="Price deviation to open safety order (%)", minval=0.0, step=0.1)/100
safe_order_volume_scale = input(1.5, type=input.float,  title="Safety Order Volume Scale", step=0.1) 
safe_order_step_scale   = input(1.6, type=input.float,  title="Safety Order Step Scale", step=0.1) 
dsc                     = input(defval="RSI-7",         title="Deal Start Condition", options=["Start ASAP", "RSI-7", "TV presets"])
dsc_rsi_threshold       = input(defval=30,              title="[RSI-7 only] RSI Threshold", minval=1, maxval=99)
dsc_technicals          = input(defval="Strong",        title="[TV presets only] Strength", options=["Strong", "Weak"])
dsc_res                 = input("",                     title="[RSI-7 and TV presets Only] Indicator Timeframe", type=input.resolution)
bot_direction           = input(defval="Long",          title="Bot Direction", options=["Long", "Short"])
start_time              = input(defval=timestamp("15 June 2021 06:00"),   title="Start Time",     type=input.time)
end_time                = input(defval=timestamp("31 Dec 2021 20:00"),   title="End Time",       type=input.time)


// ----------------------------------
// Declarations
// ----------------------------------
var bo_level = 0.0
var last_so_level = 0.0
var so_level = 0.0
var ttp_active = false
var ttp_extremum = 0.0
var ttp_level = 0.0
var stop_level = 0.0
var take_profit_level = 0.0
var deal_counter = 0
var stop_loss_counter = 0
var stop_loss_on_bar = false
var latest_price = close
var deal_start_condition = false
var start_time_actual = start_time
var end_time_actual = start_time

// CONSTANTS
var float init_price = na
var IS_LONG = bot_direction == "Long"



// ----------------------------------
// Utilities functions
// ----------------------------------
pretty_date(t) => tostring(dayofmonth(t)) + "/" + tostring(month(t)) + "/" + tostring(year(t))

within_window() => time >= start_time and time <= end_time

base_order_size() => order_size_type == "Fixed" ? base_order : base_order/100 * strategy.equity
safe_order_size() => order_size_type == "Fixed" ? safe_order : safe_order/100 * strategy.equity

safety_order_deviation(index) => price_deviation * pow(safe_order_step_scale,  index - 1)

safety_order_price(index, last_safety_order_price) => 
    if IS_LONG
        last_safety_order_price * (1 - safety_order_deviation(index))
    else
        last_safety_order_price * (1 + safety_order_deviation(index))

safety_order_qty(index) => safe_order_size() * pow(safe_order_volume_scale, index - 1)

max_amount_for_bot_usage() =>
    var total_qty = 0.0
    var last_order_qty = 0.0
    total_qty := base_order_size()
    last_order_qty := safe_order_size()
    if max_safe_order > 0
        for index = 1 to max_safe_order
            total_qty := total_qty + safety_order_qty(index)
    total_qty // returned value

max_deviation() =>
    var total_deviation = 0.0
    total_deviation := 0.0
    for index = 1 to max_safe_order
        total_deviation := total_deviation + safety_order_deviation(index)

currency_format() =>
    if syminfo.currency == "USDT" or syminfo.currency == "USD" or syminfo.currency == "TUSD" or syminfo.currency == "BUSD" or syminfo.currency == "USDC" or syminfo.currency == "EUR" or syminfo.currency == "AUD"
        "#.##"
    else if syminfo.currency == "BTC"
        "#.########"
    else
        // TODO (rouxam) list more options
        "#.####"



// ***********************************
// Deal Start Condition Strategies
// ***********************************

// RSI-7 
// ***********************************

rsi_signal() =>
    // Regular strat would be crossover but not for 3C DCA Bot
    rsi7 = rsi(close, 7)
    if IS_LONG
        [rsi7 < dsc_rsi_threshold, close]
    else
        [rsi7 > dsc_rsi_threshold, close]

// TV presets
// ***********************************
// This whole section is from the TradingView "Technical Ratings" code.
// Adding the Technical Ratings "indicator" as input to this "strategy" is not sufficient for our purpose,
// Therefore the code is copy-pasted.
// ***********************************

// Awesome Oscillator
AO() => 
    sma(hl2, 5) - sma(hl2, 34)

// Stochastic RSI
StochRSI() =>
    rsi1 = rsi(close, 14)
    K = sma(stoch(rsi1, rsi1, rsi1, 14), 3)
    D = sma(K, 3)
    [K, D]

// Ultimate Oscillator
tl() => close[1] < low ? close[1]: low
uo(ShortLen, MiddlLen, LongLen) =>
    Value1 = sum(tr, ShortLen)
    Value2 = sum(tr, MiddlLen)
    Value3 = sum(tr, LongLen)
    Value4 = sum(close - tl(), ShortLen)
    Value5 = sum(close - tl(), MiddlLen)
    Value6 = sum(close - tl(), LongLen)
    float UO = na
    if Value1 != 0 and Value2 != 0 and Value3 != 0
        var0 = LongLen / ShortLen
        var1 = LongLen / MiddlLen
        Value7 = (Value4 / Value1) * (var0)
        Value8 = (Value5 / Value2) * (var1)
        Value9 = (Value6 / Value3)
        UO := (Value7 + Value8 + Value9) / (var0 + var1 + 1)
    UO

// Ichimoku Cloud
donchian(len) => avg(lowest(len), highest(len))
ichimoku_cloud() =>
    conversionLine = donchian(9)
    baseLine = donchian(26)
    leadLine1 = avg(conversionLine, baseLine)
    leadLine2 = donchian(52)
    [conversionLine, baseLine, leadLine1, leadLine2]

calcRatingMA(ma, src) => na(ma) or na(src) ? na : (ma == src ? 0 : ( ma < src ? 1 : -1 ))
calcRating(buy, sell) => buy ? 1 : ( sell ? -1 : 0 )

ta_presets_signal() =>
    //============== MA =================
    SMA10 = sma(close, 10)
    SMA20 = sma(close, 20)
    SMA30 = sma(close, 30)
    SMA50 = sma(close, 50)
    SMA100 = sma(close, 100)
    SMA200 = sma(close, 200)
    
    EMA10 = ema(close, 10)
    EMA20 = ema(close, 20)
    EMA30 = ema(close, 30)
    EMA50 = ema(close, 50)
    EMA100 = ema(close, 100)
    EMA200 = ema(close, 200)
    
    HullMA9 = hma(close, 9)
    
    // Volume Weighted Moving Average (VWMA)
    VWMA = vwma(close, 20)
    
    [IC_CLine, IC_BLine, IC_Lead1, IC_Lead2] = ichimoku_cloud()
    
    // ======= Other =============
    // Relative Strength Index, RSI
    RSI = rsi(close,14)
    
    // Stochastic
    lengthStoch = 14
    smoothKStoch = 3
    smoothDStoch = 3
    kStoch = sma(stoch(close, high, low, lengthStoch), smoothKStoch)
    dStoch = sma(kStoch, smoothDStoch)
    
    // Commodity Channel Index, CCI
    CCI = cci(close, 20)
    
    // Average Directional Index
    float adxValue = na, float adxPlus = na, float adxMinus = na
    [P, M, V] = dmi(14, 14)
    adxValue := V
    adxPlus := P
    adxMinus := M
    // Awesome Oscillator
    ao = AO()
    
    // Momentum
    Mom = mom(close, 10)
    // Moving Average Convergence/Divergence, MACD
    [macdMACD, signalMACD, _] = macd(close, 12, 26, 9)
    // Stochastic RSI
    [Stoch_RSI_K, Stoch_RSI_D] = StochRSI()
    // Williams Percent Range
    WR = wpr(14)
    
    // Bull / Bear Power
    BullPower = high - ema(close, 13)
    BearPower = low - ema(close, 13)
    // Ultimate Oscillator
    UO = uo(7,14,28)
    if not na(UO)
        UO := UO * 100
    ////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    
    PriceAvg = ema(close, 50)
    DownTrend = close < PriceAvg
    UpTrend = close > PriceAvg
    // calculate trading recommendation based on SMA/EMA
    float ratingMA = 0
    float ratingMAC = 0
    
    if not na(SMA10)
        ratingMA := ratingMA + calcRatingMA(SMA10, close)
        ratingMAC := ratingMAC + 1
    if not na(SMA20)
        ratingMA := ratingMA + calcRatingMA(SMA20, close)
        ratingMAC := ratingMAC + 1
    if not na(SMA30)
        ratingMA := ratingMA + calcRatingMA(SMA30, close)
        ratingMAC := ratingMAC + 1
    if not na(SMA50)
        ratingMA := ratingMA + calcRatingMA(SMA50, close)
        ratingMAC := ratingMAC + 1
    if not na(SMA100)
        ratingMA := ratingMA + calcRatingMA(SMA100, close)
        ratingMAC := ratingMAC + 1
    if not na(SMA200)
        ratingMA := ratingMA + calcRatingMA(SMA200, close)
        ratingMAC := ratingMAC + 1
    if not na(EMA10)
        ratingMA := ratingMA + calcRatingMA(EMA10, close)
        ratingMAC := ratingMAC + 1
    if not na(EMA20)
        ratingMA := ratingMA + calcRatingMA(EMA20, close)
        ratingMAC := ratingMAC + 1
    if not na(EMA30)
        ratingMA := ratingMA + calcRatingMA(EMA30, close)
        ratingMAC := ratingMAC + 1
    if not na(EMA50)
        ratingMA := ratingMA + calcRatingMA(EMA50, close)
        ratingMAC := ratingMAC + 1
    if not na(EMA100)
        ratingMA := ratingMA + calcRatingMA(EMA100, close)
        ratingMAC := ratingMAC + 1
    if not na(EMA200)
        ratingMA := ratingMA + calcRatingMA(EMA200, close)
        ratingMAC := ratingMAC + 1
    
    if not na(HullMA9)
        ratingHullMA9 = calcRatingMA(HullMA9, close)
        ratingMA := ratingMA + ratingHullMA9
        ratingMAC := ratingMAC + 1
    
    if not na(VWMA)
        ratingVWMA = calcRatingMA(VWMA, close)
        ratingMA := ratingMA + ratingVWMA
        ratingMAC := ratingMAC + 1
    
    float ratingIC = na
    if not (na(IC_Lead1) or na(IC_Lead2) or na(close) or na(close[1]) or na(IC_BLine) or na(IC_CLine))
        ratingIC := calcRating(
         IC_Lead1 > IC_Lead2 and close > IC_Lead1 and close < IC_BLine and close[1] < IC_CLine and close > IC_CLine,
         IC_Lead2 > IC_Lead1 and close < IC_Lead2 and close > IC_BLine and close[1] > IC_CLine and close < IC_CLine)
    if not na(ratingIC)
        ratingMA := ratingMA + ratingIC
        ratingMAC := ratingMAC + 1
    
    ratingMA := ratingMAC > 0 ? ratingMA / ratingMAC : na
    
    float ratingOther = 0
    float ratingOtherC = 0
    
    ratingRSI = RSI
    if not(na(ratingRSI) or na(ratingRSI[1]))
        ratingOtherC := ratingOtherC + 1
        ratingOther := ratingOther + calcRating(ratingRSI < 30 and ratingRSI[1] < ratingRSI, ratingRSI > 70 and ratingRSI[1] > ratingRSI)
    
    if not(na(kStoch) or na(dStoch) or na(kStoch[1]) or na(dStoch[1]))
        ratingOtherC := ratingOtherC + 1
        ratingOther := ratingOther + calcRating(kStoch < 20 and dStoch < 20 and kStoch > dStoch and kStoch[1] < dStoch[1], kStoch > 80 and dStoch > 80 and kStoch < dStoch and kStoch[1] > dStoch[1])
    
    ratingCCI = CCI
    if not(na(ratingCCI) or na(ratingCCI[1]))
        ratingOtherC := ratingOtherC + 1
        ratingOther := ratingOther + calcRating(ratingCCI < -100 and ratingCCI > ratingCCI[1], ratingCCI > 100 and ratingCCI < ratingCCI[1])
    
    if not(na(adxValue) or na(adxPlus[1]) or na(adxMinus[1]) or na(adxPlus) or na(adxMinus))
        ratingOtherC := ratingOtherC + 1
        ratingOther := ratingOther + calcRating(adxValue > 20 and adxPlus[1] < adxMinus[1] and adxPlus > adxMinus, adxValue > 20 and adxPlus[1] > adxMinus[1] and adxPlus < adxMinus)
    
    if not(na(ao) or na(ao[1]))
        ratingOtherC := ratingOtherC + 1
        ratingOther := ratingOther + calcRating(crossover(ao,0) or (ao > 0 and ao[1] > 0 and ao > ao[1] and ao[2] > ao[1]), crossunder(ao,0) or (ao < 0 and ao[1] < 0 and ao < ao[1] and ao[2] < ao[1]))
    
    if not(na(Mom) or na(Mom[1]))
        ratingOtherC := ratingOtherC + 1
        ratingOther := ratingOther + calcRating(Mom > Mom[1], Mom < Mom[1])
    
    if not(na(macdMACD) or na(signalMACD))
        ratingOtherC := ratingOtherC + 1
        ratingOther := ratingOther + calcRating(macdMACD > signalMACD, macdMACD < signalMACD)
    
    float ratingStoch_RSI = na
    if not(na(DownTrend) or na(UpTrend) or na(Stoch_RSI_K) or na(Stoch_RSI_D) or na(Stoch_RSI_K[1]) or na(Stoch_RSI_D[1]))
        ratingStoch_RSI := calcRating(
         DownTrend and Stoch_RSI_K < 20 and Stoch_RSI_D < 20 and Stoch_RSI_K > Stoch_RSI_D and Stoch_RSI_K[1] < Stoch_RSI_D[1],
         UpTrend and Stoch_RSI_K > 80 and Stoch_RSI_D > 80 and Stoch_RSI_K < Stoch_RSI_D and Stoch_RSI_K[1] > Stoch_RSI_D[1])
    if not na(ratingStoch_RSI)
        ratingOtherC := ratingOtherC + 1
        ratingOther := ratingOther + ratingStoch_RSI
    
    float ratingWR = na
    if not(na(WR) or na(WR[1]))
        ratingWR := calcRating(WR < -80 and WR > WR[1], WR > -20 and WR < WR[1])
    if not na(ratingWR)
        ratingOtherC := ratingOtherC + 1
        ratingOther := ratingOther + ratingWR
    
    float ratingBBPower = na
    if not(na(UpTrend) or na(DownTrend) or na(BearPower) or na(BearPower[1]) or na(BullPower) or na(BullPower[1]))
        ratingBBPower := calcRating(
         UpTrend and BearPower < 0 and BearPower > BearPower[1],
         DownTrend and BullPower > 0 and BullPower < BullPower[1])
    if not na(ratingBBPower)
        ratingOtherC := ratingOtherC + 1
        ratingOther := ratingOther + ratingBBPower
    
    float ratingUO = na
    if not(na(UO))
        ratingUO := calcRating(UO > 70, UO < 30)
    if not na(ratingUO)
        ratingOtherC := ratingOtherC + 1
        ratingOther := ratingOther + ratingUO
    
    ratingOther := ratingOtherC > 0 ? ratingOther / ratingOtherC : na
    
    float ratingTotal = 0
    float ratingTotalC = 0
    if not na(ratingMA)
        ratingTotal := ratingTotal + ratingMA
        ratingTotalC := ratingTotalC + 1
    if not na(ratingOther)
        ratingTotal := ratingTotal + ratingOther
        ratingTotalC := ratingTotalC + 1
    ratingTotal := ratingTotalC > 0 ? ratingTotal / ratingTotalC : na
    
    // little piece of @rouxam logic
    var bool start_condition = false
    float bound = dsc_technicals == "Strong" ? 0.5 : 0.1
    if IS_LONG
        start_condition := ratingTotal > bound
    else
        start_condition := ratingTotal < -bound
    [start_condition, close]
    

// ----------------------------------
// (Re-)Initialize
// ----------------------------------
var max_amount = max_amount_for_bot_usage()
var max_dev = max_deviation()
init_price := na(init_price) and within_window() ? open : init_price
latest_price := within_window() ? close : latest_price

// Actualize the start and end time of the backtesting window because number of bars is limited.
// NOTE: limits on number of available bars:
//      TradingView FREE account: 5000 bars available,
//      TradingView PRO/PRO+ account: 10000 bars available,
//      TradingView PREMIUM account: 20000 bars available.
start_time_actual := barstate.isfirst and time > start_time_actual ?  time : start_time_actual
end_time_actual := time > end_time_actual and time <= end_time ? time : end_time_actual


if strategy.position_size == 0.0
    ttp_extremum := 0.0
    ttp_active := false
    deal_start_condition := false

// ----------------------------------
// Open deal with Base Order on Deal Start Condition
// ----------------------------------
[dsc_rsi, bo_level_rsi] = security(syminfo.tickerid, dsc_res, rsi_signal())
[dsc_ta, bo_level_ta] = security(syminfo.tickerid, dsc_res, ta_presets_signal())



if(strategy.opentrades == 0 and within_window() and close > 0 and strategy.equity > 0.0)
    // Compute deal start condition
    if dsc == "Start ASAP"
        deal_start_condition := true
        bo_level := close
    if dsc == "RSI-7"
        deal_start_condition := dsc_rsi
        bo_level := bo_level_rsi
    if dsc == "TV presets"
        deal_start_condition := dsc_ta
        bo_level := bo_level_ta

    // Place Buy Order
    if deal_start_condition
        deal_counter := deal_counter + 1
        if IS_LONG
            strategy.entry("BO", limit=bo_level, long=strategy.long, qty=base_order_size()/bo_level)
        else
            strategy.entry("BO", limit=bo_level, long=strategy.short, qty=base_order_size()/bo_level)

        last_so_level := bo_level

        // Place Safety Orders
        if max_safe_order > 0
            for index = 1 to max_safe_order
                so_level := safety_order_price(index, last_so_level)
                so_name = "SO" + tostring(index) 
                if IS_LONG
                    strategy.entry(so_name, long=strategy.long, limit=so_level, qty=safety_order_qty(index)/so_level)
                else
                    strategy.entry(so_name, long=strategy.short, limit=so_level, qty=safety_order_qty(index)/so_level)
                last_so_level := so_level

// ----------------------------------
// Close Deal on SL, TP or TTP
// ----------------------------------
if abs(strategy.position_size) > 0
    take_profit_factor = IS_LONG ? (1 + take_profit) : (1 - take_profit)
    stop_loss_factor = IS_LONG ? (1 - stop_loss) : (1 + stop_loss)
    ttp_factor = IS_LONG ? (1 - ttp) : (1 + ttp)
    stop_level := bo_level * stop_loss_factor
    if take_profit_type == "% from total volume"
        take_profit_level := strategy.position_avg_price * take_profit_factor
    else
        take_profit_level := bo_level * take_profit_factor

    // Stop Loss
    stop_loss_on_bar := false
    if stop_loss > max_dev and not ttp_active
        if IS_LONG and low < stop_level
            stop_loss_counter := stop_loss_counter + 1
            strategy.exit(id="x", stop=stop_level, comment="SL")
            stop_loss_on_bar := true
        else if not IS_LONG and high > stop_level
            stop_loss_counter := stop_loss_counter + 1
            strategy.exit(id="x", stop=stop_level, comment="SL")
            stop_loss_on_bar := true
    if not stop_loss_on_bar
        if ttp == 0.0
            // Simple take profit
            strategy.exit(id="x", limit=take_profit_level, comment="TP")
        else
            // Trailing take profit
            if IS_LONG and high >= take_profit_level 
                ttp_extremum := max(high, ttp_extremum)
                ttp_active := true
            if not IS_LONG and low <= take_profit_level
                ttp_extremum := min(low, ttp_extremum)
                ttp_active := true
            if ttp_active 
                ttp_level := ttp_extremum * ttp_factor
                strategy.exit(id="x", stop=ttp_level, comment="TTP")

// Cleanup
if (crossunder(strategy.opentrades, 0.5))
    strategy.close_all()
    strategy.cancel_all()

// ----------------------------------
// Results
// ----------------------------------
profit = max(strategy.equity, 0.0) - strategy.initial_capital
profit_pct = profit / strategy.initial_capital

add_line(the_table, the_row, the_label, the_value, is_warning) =>
    // if providing a value, the row is shown as: the_label | the_value
    // else: the_label is considered to be a title
    is_even     = (the_row % 2) == 1
    is_title    = na(the_value) ? true : false
    text        = is_title ? "" : tostring(the_value)
    bg_color    = is_title ? color.black : is_warning ? color.red : is_even ? color.silver : color.white
    text_color  = is_title ? color.white : color.black
    left_cell_align = is_title ? text.align_right : text.align_left
    table.cell(the_table, 0, the_row, the_label, bgcolor=bg_color, text_color=text_color,  text_size=size.auto, text_halign=left_cell_align)
    table.cell(the_table, 1, the_row, text,      bgcolor=bg_color, text_color=text_color,  text_size=size.auto, text_halign=text.align_right) 

var string warnings_text = ""
var warnings = array.new_string(0, "")

if (max_amount / strategy.initial_capital > 1.005 or max_amount / strategy.initial_capital < 0.995)
    if order_size_type == "Fixed"
        array.push(warnings, "Strategy Initial Capital (currently " + tostring(strategy.initial_capital, currency_format()) + " " + syminfo.currency
                           + ") must match Max Amount For Bot Usage (" + tostring(max_amount, currency_format()) + " " + syminfo.currency + ")")
    else
        array.push(warnings, "Please adjust Base Order and Safe Order percentage to reach 100% of usage of capital. Currently using " + tostring(max_amount / strategy.initial_capital, "#.##%"))

if (max_dev >= 1.0)
    array.push(warnings, "Max Price Deviation (" + tostring(max_dev, "#.##%") + ") cannot be >= 100.0%")
if (stop_loss > 0.0 and stop_loss <= max_dev)
    array.push(warnings, "Stop Loss (" + tostring(stop_loss, "#.##%") + ") should be greater than Max Price Deviation (" + tostring(max_dev, "#.##%") + ")")
if ((timeframe.isminutes and not (tonumber(timeframe.period) < 30)) or timeframe.isdwm)
    array.push(warnings, "Backtesting may be inaccurate. Recommended to use timeframe of 15m or less")
if (ttp >= take_profit)
    array.push(warnings, "Trailing Take Profit (" + tostring(ttp, "#.##%") + ") cannot be greater than Take Profit (" + tostring(take_profit, "#.##%") + ")")

// Making the results table
var results_table  = table.new(position.top_right, 2, 20, frame_color=color.black)
curr_row = -1
None = int(na)
curr_row += 1, add_line(results_table, curr_row, "Bot setup", None, false)
if (array.size(warnings) > 0)
    for index = 1 to array.size(warnings)
        curr_row += 1
        add_line(results_table, curr_row, "Please review your strategy settings", array.pop(warnings), true)
curr_row += 1, add_line(results_table, curr_row, "Dates", pretty_date(start_time_actual) + " to " + pretty_date(end_time_actual), false)
curr_row += 1, add_line(results_table, curr_row, "Deal Start Condition", dsc, false)
curr_row += 1, add_line(results_table, curr_row, "Base and Safety Orders", "BO: " + tostring(base_order, currency_format()) + " " + (order_size_type == "Fixed" ? "" : "% ") + syminfo.currency + " / " + "SO: " + tostring(safe_order, currency_format()) + " " + (order_size_type == "Fixed" ? "" : "% ") + syminfo.currency, false)
curr_row += 1, add_line(results_table, curr_row, "Price Deviation", tostring(price_deviation, "#.##%"), false)
curr_row += 1, add_line(results_table, curr_row, "Safe Order Scale", "Volume: " + tostring(safe_order_volume_scale) + " / Scale: " + tostring(safe_order_step_scale), false)
curr_row += 1, add_line(results_table, curr_row, "Take Profit / Stop Loss", tostring(take_profit, "#.##%") + (ttp > 0.0 ?  "( Trailing: " + tostring(ttp, "#.##%") + ")" : "") + "/ " + (stop_loss > 0.0 ? tostring(stop_loss, "#.##%") : "No Stop Loss"), false)
curr_row += 1, add_line(results_table, curr_row, "Max Amount For Bot Usage", tostring(max_amount, currency_format()) + " " + syminfo.currency, false)
curr_row += 1, add_line(results_table, curr_row, "Max Coverage", tostring(max_safe_order) + " Safety Orders / " + tostring(max_dev, "#.##%"), false)
curr_row += 1, add_line(results_table, curr_row, "Summary", None, false)
curr_row += 1, add_line(results_table, curr_row, "Initial Capital", tostring(strategy.initial_capital, currency_format()) + " " + syminfo.currency, false)
curr_row += 1, add_line(results_table, curr_row, "Final Capital",   tostring(max(strategy.equity, 0.0), currency_format()) + " " + syminfo.currency, false)
curr_row += 1, add_line(results_table, curr_row, "Net Result",      tostring(profit, currency_format()) + " " + syminfo.currency + " (" +  tostring(profit_pct, "#.##%") + ")", false)
curr_row += 1, add_line(results_table, curr_row, "Total Deals",     tostring(deal_counter), false)
if stop_loss > 0.0
    curr_row += 1, add_line(results_table, curr_row, "Deals Closed on Stop Loss", tostring(stop_loss_counter), false)
if IS_LONG
    buy_hold_profit = (latest_price - init_price) / init_price * strategy.initial_capital
    buy_hold_profit_pct = buy_hold_profit / strategy.initial_capital
    ratio = profit_pct / buy_hold_profit_pct
    curr_row += 1, add_line(results_table, curr_row, "Comparison to Buy-And-Hold",                      None, false)
    curr_row += 1, add_line(results_table, curr_row, "Net Result for Buy-and-hold",                     tostring(buy_hold_profit, currency_format()) + " " + syminfo.currency + " (" +  tostring(buy_hold_profit_pct, "#.##%") + ")", false)
    curr_row += 1, add_line(results_table, curr_row, "(Bot Performance) / (Buy-and-hold Performance)",  tostring(ratio, "#.###"), false)

