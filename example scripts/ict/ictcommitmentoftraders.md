// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/Description:
// The Commitment of Traders (COT) is a valuable raw data report released weekly by the Commodity Futures Trading Commission (CFTC). This report offers insights into the current long and short positions of three key market entities:
// Commercial Traders (usually represented in red)
// Large Traders (typically depicted in green)
// Small Speculator Traders (commonly shown in blue)

// The concept of utilizing the COT data as a strategic trading tool was first introduced by Larry Williams, who emphasized the importance of monitoring Commercial Speculators – large corporate producers or consumers of commodities.

// The Inner Circle Trader (ICT) prompts us to delve deeper into this data. While we can easily determine their Net Position (also referred to as the Main Program) by subtracting Commercial Short Positions from the Commercial Long Positions, this calculation doesn't reveal their ongoing Hedge Program.

// Merely following the Main Program won't provide a trading edge. Aligning with the Hedge Program can be an invaluable weapon in your trading arsenal.

// The Commercial Speculators' Hedge Program can be unveiled by examining the highest and lowest reading of their Net Position over a chosen time period and setting a new "zero line" between these extremes. This process generates a novel "COT Graph" providing a detailed understanding of the Commercial Speculators' current market activity.

// When the Hedge Program, Seasonality, and Open Interest are cross-referenced with Institutional Orderflow, a trader can construct a very clear medium-to-long-term market narrative.

// Features:
// Access COT Data for the Commercial Speculators via Tradingview's reliable data source
// Automate calculations and display the 3-month, 6-month, 12-month, 2-year, and 3-year Hedge Program
// Define your own Custom Time Range for the Hedge Program
// Display the Main Program and all Hedge Programs in an easy-to-understand table format

// Additionally, by following the included instructions, you can augment your table with COT data from multiple markets. This extra information can help monitor correlated markets and develop a more robust market narrative.




import TradingView/LibraryCOT/2 as cot
//@version=5
indicator("ICT Commitment of Traders"
         , "COT°"
         , format=format.volume
         , max_bars_back=5000
         , max_boxes_count=10
         , max_lines_count=10)

if timeframe.in_seconds(timeframe.period) < timeframe.in_seconds("D")
    runtime.error("Go to the Daily Timeframe or higher to see COT Data!")


//#region[Global]
noColor = color.new(color.white, 100)
_month  =       month(time, "America/New_York")
_year   =        year(time, "America/New_York")
_day    =  dayofmonth(time, "America/New_York")
start3M =   timestamp(_month < 4? _year-1 : _year, _month <= 3 ? 9+_month : _month-3, _day, hour, minute, second)
start6M =   timestamp(_month < 7? _year-1 : _year, _month <= 6 ? 6+_month : _month-6, _day, hour, minute, second)
start1Y =   timestamp(_year-1                    , _month                           , _day, hour, minute, second)
start2Y =   timestamp(_year-2                    , _month                           , _day, hour, minute, second)
start3Y =   timestamp(_year-3                    , _month                           , _day, hour, minute, second)

boxLoc(string _loc) =>
    loc = switch _loc
        "Top-Left"      => position.top_left
        "Top-Center"    => position.top_center
        "Top-Right"     => position.top_right
        "Middle-Left"   => position.middle_left
        "Middle-Center" => position.middle_center
        "Middle-Right"  => position.middle_right
        "Bottom-Left"   => position.bottom_left
        "Bottom-Center" => position.bottom_center
        "Bottom-Right"  => position.bottom_right
    loc

size(string _size) =>
    size = switch _size
        "Tiny"   => size.tiny
        "Small"  => size.small
        "Normal" => size.normal
        "Large"  => size.large
        "Huge"   => size.huge
    size
//#endregion


//#region[User Inputs]
// COT Data
lookBack_Title_UI = "COT Hedge Program ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏"

lookBack   = input.string("6 Months"           , title=lookBack_Title_UI      , group="COT Data", options=['3 Months', '6 Months', '12 Months', '2 Years', '3 Years', 'Custom'], inline='cot')
optionData = input.bool(false                  , title="Include Options Data?", group="COT Data", inline='cot')
_userStart = input.time(timestamp('01-01-2100'), title='Custom COT Look Back' , group="COT Data")

// COT Data (style)
showCOT =  input.bool(true                    , title="Show COT Data?"          , group="COT Data (style)")
colorCS = input.color(color.new(#c82f3b, 0) , title="Comm. Spec. Net Position", group="COT Data (style)")
zeroMC  = input.color(color.new(#787B86, 0) , title="Main Zero Line"          , group="COT Data (style)")
zeroHC  = input.color(color.new(#000000, 0) , title="Hedge Zero Line"         , group="COT Data (style)")
shortHC = input.color(color.new(#880e4f, 90), title="Hedge Sell Area"         , group="COT Data (style)")
longHC  = input.color(color.new(#4caf50, 90), title="Hedge Buy Area"          , group="COT Data (style)")

// COT Table Info
showTable =   input.bool(true                   , title="Show Info Table?"       , group="COT Table Info")
showMult  =   input.bool(true                   , title="Show Multi-Market Data?", group="COT Table Info", tooltip='Follow the instructions in the code to enable this feature!')
colorBuy  =  input.color(#000000              , title="Buy Text Color"         , group="COT Table Info")
colorSell =  input.color(#000000              , title="Sell Text Color"        , group="COT Table Info")
highlight =  input.color(color.new(#b2b5be, 0), title="Highlight Color"        , group="COT Table Info")

// COT Table Info (style)
_y_Title_UI        = " ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏Location ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏"
_x_Title_UI        = " ‏ ‏ ‏ ‏"
tableSize_Title_UI = " ‏ ‏ ‏ ‏ ‏ ‏ ‏Table Size"
bgcolorTI_Title_UI = " ‏ ‏ ‏ ‏ ‏ ‏Background"
txcolorTI_Title_UI = " ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏ ‏Text"

_y        = input.string("Middle"               , title=_y_Title_UI       , group="COT Table Info (style)", options=["Top","Middle","Bottom"], inline='xy')
_x        = input.string("Right"                , title=_x_Title_UI       , group="COT Table Info (style)", options=["Left","Center","Right"], inline='xy')
tableSize =         size(input.string("Small"   , title=tableSize_Title_UI, group="COT Table Info (style)", options=['Tiny', 'Small', 'Normal', 'Large', 'Huge']))
bgcolorTI =  input.color(color.new(#d1d4dc, 0), title=bgcolorTI_Title_UI, group="COT Table Info (style)")
txcolorTI =  input.color(color.new(#000000, 0), title=txcolorTI_Title_UI, group="COT Table Info (style)")
//#endregion


//#region[Commitment Of Traders Data]
// Current Symbol
c_cftcCode = cot.convertRootToCOTCode("Auto")
c_CommSpec = request.security(cot.COTTickerid("Legacy", c_cftcCode, optionData, "Commercial Positions", "Long" , "All"), "D", close, ignore_invalid_symbol=true) - 
             request.security(cot.COTTickerid("Legacy", c_cftcCode, optionData, "Commercial Positions", "Short", "All"), "D", close, ignore_invalid_symbol=true)
label = syminfo.root != syminfo.ticker and cot.rootToCFTCCode(syminfo.root) != "" ? syminfo.root : 
         (cot.currencyToCFTCCode(syminfo.basecurrency) != "" ? syminfo.basecurrency : 
         (cot.currencyToCFTCCode(syminfo.currency) != "" ? syminfo.currency : ""))

//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\
//\\----------------------------------------------------------------------------------------------------//\\
//\\ How do I display multiple markets' COT Data?                                                       //\\
//\\----------------------------------------------------------------------------------------------------//\\
//\\ Use the Find tool (ctrl+F / cmmd+F) and replace "//| " with "" (omit the "")                       //\\
//\\ After you have done that, choose and add the appropriate function to Lines 121, 127, and 133       //\\
//\\ See box below to learn which function is best to use                                               //\\
//\\----------------------------------------------------------------------------------------------------//\\
//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\

//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\
//\\----------------------------------------------------------------------------------------------------//\\
//\\ What function do I choose?                                                                         //\\
//\\----------------------------------------------------------------------------------------------------//\\
//\\ FUTURES CONTRACT: cot.rootToCFTCCode("")     ~ I.E. rootToCFTCCode("NQ") for Nasdaq E-Mini Futures //\\
//\\ FOREX OR DXY:     cot.currencyToCFTCCode("") ~ I.E. rootToCFTCCode("EUR") for EURUSD Forex Pair    //\\
//\\                                                                                                    //\\
//\\ For DXY you must use "USD": cot.currencyToCFTCCode("USD")                                          //\\
//\\----------------------------------------------------------------------------------------------------//\\
//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\

// Symbol 1
//| s1_root     = "NQ"
//| s1_cftcCode = (s1_root) //cot.rootToCFTCCode(s1_root)
//| s1_CommSpec = request.security(cot.COTTickerid("Legacy", s1_cftcCode, optionData, "Commercial Positions", "Long" , "All"), "D", close, ignore_invalid_symbol=true) - 
//|               request.security(cot.COTTickerid("Legacy", s1_cftcCode, optionData, "Commercial Positions", "Short", "All"), "D", close, ignore_invalid_symbol=true)

// Symbol 2
//| s2_root     = "YM"
//| s2_cftcCode = (s2_root) //cot.rootToCFTCCode(s2_root)
//| s2_CommSpec = request.security(cot.COTTickerid("Legacy", s2_cftcCode, optionData, "Commercial Positions", "Long" , "All"), "D", close, ignore_invalid_symbol=true) - 
//|               request.security(cot.COTTickerid("Legacy", s2_cftcCode, optionData, "Commercial Positions", "Short", "All"), "D", close, ignore_invalid_symbol=true)

// Symbol 3 
//| s3_root     = "USD"
//| s3_cftcCode = (s3_root) //cot.currencyToCFTCCode(s3_root)
//| s3_CommSpec = request.security(cot.COTTickerid("Legacy", s3_cftcCode, optionData, "Commercial Positions", "Long" , "All"), "D", close, ignore_invalid_symbol=true) - 
//|               request.security(cot.COTTickerid("Legacy", s3_cftcCode, optionData, "Commercial Positions", "Short", "All"), "D", close, ignore_invalid_symbol=true)

if barstate.islastconfirmedhistory 
    if na(c_CommSpec)
        runtime.error("Could not find relevant COT data for: " + syminfo.root)
//|     else if (na(s1_CommSpec) or na(s2_CommSpec)or na(s3_CommSpec))
//|         runtime.error("Could not find relevant COT data for: " + (na(s1_CommSpec) ? s1_root : (na(s2_CommSpec) ? s2_root : s3_root)))
//#endregion


//#region[Functions]
ict_COT(float COT, int init, bool show=false) =>
    float _high = na
    float _low  = na
    for i=0 to bar_index
        if time[i] < init
            break
        else 
            if COT[i] > _high or na(_high)
                _high := COT[i]
            if COT[i] < _low or na(_low)
                _low := COT[i]
    
    _z = math.avg(_high,_low)
    var line z = na, var box prem = na, var box disc = na
    line.delete(z), box.delete(prem), box.delete(disc)
    if show and showCOT
        z    := line.new(init, _z   , time, _z  , xloc=xloc.bar_time, color=zeroHC)
        prem :=  box.new(init, _high, time, _z  , xloc=xloc.bar_time, border_color=noColor, bgcolor=longHC)
        disc :=  box.new(init, _z   , time, _low, xloc=xloc.bar_time, border_color=noColor, bgcolor=shortHC)
    _z

ict_COT_Market(float COT, int init, bool show) =>
    three_months  = ict_COT(COT, start3M)
    six_months    = ict_COT(COT, start6M)
    twelve_months = ict_COT(COT, start1Y)
    two_years     = ict_COT(COT, start2Y)
    three_years   = ict_COT(COT, start3Y)
    user_input    = ict_COT(COT, init, show)

    [three_months, six_months, twelve_months, two_years, three_years, user_input]

tableInit(table tbl) =>
    table.cell(tbl, 0, 0, 'Market°'     , 0, 0, txcolorTI, "center", text_size=tableSize, bgcolor=bgcolorTI                                , text_font_family=font.family_monospace)
    table.cell(tbl, 1, 0, 'Main-Program', 0, 0, txcolorTI, "center", text_size=tableSize, bgcolor=bgcolorTI                                , text_font_family=font.family_monospace)
    table.cell(tbl, 2, 0, '3-Month'     , 0, 0, txcolorTI, "center", text_size=tableSize, bgcolor=lookBack=='3 Months' ?highlight:bgcolorTI, text_font_family=font.family_monospace)
    table.cell(tbl, 3, 0, '6-Month'     , 0, 0, txcolorTI, "center", text_size=tableSize, bgcolor=lookBack=='6 Months' ?highlight:bgcolorTI, text_font_family=font.family_monospace)
    table.cell(tbl, 4, 0, '12-Month'    , 0, 0, txcolorTI, "center", text_size=tableSize, bgcolor=lookBack=='12 Months'?highlight:bgcolorTI, text_font_family=font.family_monospace)
    table.cell(tbl, 5, 0, '2-Year'      , 0, 0, txcolorTI, "center", text_size=tableSize, bgcolor=lookBack=='2 Years'  ?highlight:bgcolorTI, text_font_family=font.family_monospace)
    table.cell(tbl, 6, 0, '3-Year'      , 0, 0, txcolorTI, "center", text_size=tableSize, bgcolor=lookBack=='3 Years'  ?highlight:bgcolorTI, text_font_family=font.family_monospace)
    if lookBack=='Custom'
        table.cell(tbl, 7, 0, 'Custom-Range', 0, 0, txcolorTI, "center", text_size=tableSize, bgcolor=lookBack=='Custom'?highlight:bgcolorTI, text_font_family=font.family_monospace)

tableEntry(table tbl, string mrkt, int row, float COT, float v1, float v2, float v3, float v4, float v5, float v6, int init) =>
    table.cell(tbl, 0, row, mrkt               , 0, 0, txcolorTI                , "center", text_size=tableSize, bgcolor=bgcolorTI                                , text_font_family=font.family_monospace)
    table.cell(tbl, 1, row, COT>0?'BUY':'SELL' , 0, 0, COT>0?colorBuy:colorSell , "center", text_size=tableSize, bgcolor=bgcolorTI                                , text_font_family=font.family_monospace)
    table.cell(tbl, 2, row, COT>v1?'BUY':'SELL', 0, 0, COT>v1?colorBuy:colorSell, "center", text_size=tableSize, bgcolor=lookBack=='3 Months' ?highlight:bgcolorTI, text_font_family=font.family_monospace)
    table.cell(tbl, 3, row, COT>v2?'BUY':'SELL', 0, 0, COT>v2?colorBuy:colorSell, "center", text_size=tableSize, bgcolor=lookBack=='6 Months' ?highlight:bgcolorTI, text_font_family=font.family_monospace)
    table.cell(tbl, 4, row, COT>v3?'BUY':'SELL', 0, 0, COT>v3?colorBuy:colorSell, "center", text_size=tableSize, bgcolor=lookBack=='12 Months'?highlight:bgcolorTI, text_font_family=font.family_monospace)
    table.cell(tbl, 5, row, COT>v4?'BUY':'SELL', 0, 0, COT>v4?colorBuy:colorSell, "center", text_size=tableSize, bgcolor=lookBack=='2 Years'  ?highlight:bgcolorTI, text_font_family=font.family_monospace)
    table.cell(tbl, 6, row, COT>v5?'BUY':'SELL', 0, 0, COT>v5?colorBuy:colorSell, "center", text_size=tableSize, bgcolor=lookBack=='3 Years'  ?highlight:bgcolorTI, text_font_family=font.family_monospace)
    if lookBack=='Custom'
        txt = COT>v6?'BUY':'SELL'
        col = COT>v6?colorBuy:colorSell
        table.cell(tbl, 7, row, na(init)?txt:(init>time?"NA":txt), 0, 0, na(init)?col:(init>time?txcolorTI:col), "center", text_size=tableSize, bgcolor=lookBack=='Custom'?highlight:bgcolorTI, text_font_family=font.family_monospace)
//#endregion


//#region[Logic]
// ICT Commitment Of Traders
start = switch lookBack
    '3 Months'  => start3M
    '6 Months'  => start6M
    '12 Months' => start1Y
    '2 Years'   => start2Y
    '3 Years'   => start3Y
    'Custom'    => _userStart

[c_3M , c_6M , c_1Y , c_2Y , c_3Y , c_UI ] = ict_COT_Market(c_CommSpec , start, true)
//| [s1_3M, s1_6M, s1_1Y, s1_2Y, s1_3Y, s1_UI] = ict_COT_Market(s1_CommSpec, start, false)
//| [s2_3M, s2_6M, s2_1Y, s2_2Y, s2_3Y, s2_UI] = ict_COT_Market(s2_CommSpec, start, false)
//| [s3_3M, s3_6M, s3_1Y, s3_2Y, s3_3Y, s3_UI] = ict_COT_Market(s3_CommSpec, start, false)
//#endregion


//#region[Plot]
// Graph
if showCOT
    var zero = line.new(time, 0, time+1, 0, xloc=xloc.bar_time, extend=extend.both, color=zeroMC)

    var line _start = na
    line.delete(_start)
    _start := line.new(start, c_CommSpec-1, start, c_CommSpec+1, xloc=xloc.bar_time, extend=extend.both, color=zeroMC)

data = plot(showCOT ? c_CommSpec : na, title="Comm. Spec. Net Position", color=colorCS)

// Table
loc = boxLoc(_y+"-"+_x)
if barstate.islast and showTable
    algo_program = table.new(loc, 8, 20, frame_color=txcolorTI, frame_width=1)
    tableInit(algo_program)

    // Current Symbol 
    tableEntry(algo_program, label, 1, c_CommSpec, c_3M, c_6M, c_1Y, c_2Y, c_3Y, c_UI, start)

    // Other Markets' Data
    //| if showMult
    //|     tableEntry(algo_program, s1_root, 2, s1_CommSpec, s1_3M, s1_6M, s1_1Y, s1_2Y, s1_3Y, s1_UI, start)
    //|     tableEntry(algo_program, s2_root, 3, s2_CommSpec, s2_3M, s2_6M, s2_1Y, s2_2Y, s2_3Y, s2_UI, start)
    //|     tableEntry(algo_program, s3_root, 4, s3_CommSpec, s3_3M, s3_6M, s3_1Y, s3_2Y, s3_3Y, s3_UI, start)
//#endregion