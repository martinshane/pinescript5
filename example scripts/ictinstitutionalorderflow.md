// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © fadizeidan
//
//
// Displacement logic Credit: Visualizing Displacement [TFO]
// https://www.tradingview.com/script/n6djFgQH-Visualizing-Displacement-TFO/
// FTO creates other great indicators that I personally use.

//@version=5
indicator("ICT Institutional Order Flow (fadi)", overlay=true, max_bars_back = 5000, max_lines_count = 500, max_labels_count = 500, max_boxes_count = 500)

//+------------------------------------------------------------------------------------------------------------+//
//+--- Types                                                                                                ---+//
//+------------------------------------------------------------------------------------------------------------+//
type Settings 
    string  liquidity_open_style
    string  liquidity_claimed_style

    bool    ST_show
    string  ST_text_size
    color   ST_color_bull
    color   ST_color_bear
    color   ST_label_color
    bool    ST_liquidity_show
    color   ST_liquidity_open_color
    int     ST_liquidity_size
    color   ST_liquidity_claimed_color
    
    bool    IT_show
    string  IT_text_size
    color   IT_color_bull
    color   IT_color_bear
    color   IT_label_color
    bool    IT_liquidity_show
    color   IT_liquidity_open_color
    int     IT_liquidity_size
    color   IT_liquidity_claimed_color
    
    bool    LT_show
    string  LT_text_size
    color   LT_color_bull
    color   LT_color_bear
    color   LT_label_color
    bool    LT_liquidity_show
    color   LT_liquidity_open_color
    int     LT_liquidity_size
    color   LT_liquidity_claimed_color

    int     max_labels

    bool    liquidity_open_show
    int     max_lines
    int     extend

    bool    displacement_show
    bool    displacement_fvg
    int     displacement_length
    int     displacement_factor
    color   displacement_bull
    color   displacement_bear

type Imbalance_Settings
    bool    show
    color   color_bull
    color   color_bear
    bool    CE_show
    string  CE_style
    color   CE_color
    string  fvg_type
    bool    mitigated_show
    string  mitigated_type
    color   mitigated_color_bull
    color   mitigated_color_bear
    int     max_count

type Pivot
    int     time                    = 0
    float   price                   = 0
    int     time_last               = 0
    bool    claimed                 = false
    bool    isHigh                  = na
    string  lbl_text                = ""
    label   lbl                     = na
    line    ln                      = na

type MarketStructure
    string          name
    Pivot[]         ST
    Pivot[]         IT
    Pivot[]         LT
    Pivot[]         STH
    Pivot[]         ITH
    Pivot[]         STL
    Pivot[]         ITL
    Settings        settings

type Imbalance
    int     open_time
    int     close_time
    float   open
    float   middle
    float   close
    bool    mitigated
    int     mitigated_time
    line    line_middle
    box     box

type ImbalanceStructure
    Imbalance[]         imbalance
    Imbalance_Settings  settings

type Helper
    string name         = "Helper"

//+------------------------------------------------------------------------------------------------------------+//
//+--- Settings                                                                                             ---+//
//+------------------------------------------------------------------------------------------------------------+//
int MAX_BUFFER                      = 500 // internal maximum node count limit for performance

ICT_Group                           = "ICT Market Structure"

settings_liquidity                  = "Liquidity"
settings_liquidity_style            = "Liquidity Style"
settings_displacement               = "Displacement"
FVG_Group                           = "Fair Value Gap"
Imb_Group                           = "Volume Imbalance"
Gap_Group                           = "Open Gaps"

inline_st                           = "ST"
inline_st_liquidity                 = "ST liquidity"

inline_it                           = "IT"
inline_it_liquidity                 = "IT liquidity"
inline_lt                           = "LT"
inline_lt_liquidity                 = "LT liquidity"

inline_liquidity_open               = "open liquidity"
inline_liquidity_claimed            = "claimed liquidity"
inline_displacement                 = "displacement"

Settings Trend_Settings             = Settings.new()
Imbalance_Settings FVG_Settings     = Imbalance_Settings.new()
Imbalance_Settings VI_Settings      = Imbalance_Settings.new()
Imbalance_Settings Gap_Settings     = Imbalance_Settings.new()

Trend_Settings.ST_show              := input.bool(false, "Short      ", group=ICT_Group, inline=inline_st)
Trend_Settings.ST_text_size         := input.string(size.tiny, "", [size.tiny, size.small, size.normal, size.large, size.huge], group=ICT_Group, inline=inline_st)
Trend_Settings.ST_color_bull        := input.color(color.black, "", group=ICT_Group, inline=inline_st)
Trend_Settings.ST_color_bear        := input.color(color.black, "", group=ICT_Group, inline=inline_st)
Trend_Settings.ST_label_color       := input.color(#ffffff00, "",group=ICT_Group, inline=inline_st)

Trend_Settings.IT_show              := input.bool(true, "Intermediate", group=ICT_Group, inline=inline_it)
Trend_Settings.IT_text_size         := input.string(size.small, "", [size.tiny, size.small, size.normal, size.large, size.huge], group=ICT_Group, inline=inline_it)
Trend_Settings.IT_color_bull        := input.color(color.purple, "", group=ICT_Group, inline=inline_it)
Trend_Settings.IT_color_bear        := input.color(color.purple, "", group=ICT_Group, inline=inline_it)
Trend_Settings.IT_label_color       := input.color(#ffffff00, "",group=ICT_Group, inline=inline_it)
Trend_Settings.LT_show              := input.bool(true, "Long       ", group=ICT_Group, inline=inline_lt)
Trend_Settings.LT_text_size         := input.string(size.normal, "", [size.tiny, size.small, size.normal, size.large, size.huge], group=ICT_Group, inline=inline_lt)
Trend_Settings.LT_color_bull        := input.color(color.blue, "", group=ICT_Group, inline=inline_lt)
Trend_Settings.LT_color_bear        := input.color(color.blue, "", group=ICT_Group, inline=inline_lt)
Trend_Settings.LT_label_color       := input.color(#ffffff00, "", group=ICT_Group, inline=inline_lt)

Trend_Settings.max_labels        := input.int(50, "Maximum number of labels", group=ICT_Group)

Trend_Settings.ST_liquidity_show            := input.bool(false, "Short      ", group = settings_liquidity, inline=inline_st_liquidity)
Trend_Settings.ST_liquidity_open_color      := input.color(color.new(color.black, 50), '', group=settings_liquidity, inline=inline_st_liquidity)
Trend_Settings.ST_liquidity_claimed_color   := input.color(color.new(color.black, 50), '', group=settings_liquidity, inline=inline_st_liquidity)
Trend_Settings.ST_liquidity_size            := input.int(1, "", options = [1,2,3,4], group=settings_liquidity, inline=inline_st_liquidity)

Trend_Settings.IT_liquidity_show            := input.bool(true, "Intermediate", group = settings_liquidity, inline=inline_it_liquidity)
Trend_Settings.IT_liquidity_open_color      := input.color(color.purple, '', group=settings_liquidity, inline=inline_it_liquidity)
Trend_Settings.IT_liquidity_claimed_color   := input.color(color.purple, '', group=settings_liquidity, inline=inline_it_liquidity)
Trend_Settings.IT_liquidity_size            := input.int(1, "", options = [1,2,3,4], group=settings_liquidity, inline=inline_it_liquidity)

Trend_Settings.LT_liquidity_show            := input.bool(true, "Long       ", group = settings_liquidity, inline=inline_lt_liquidity)
Trend_Settings.LT_liquidity_open_color      := input.color(color.blue, '', group=settings_liquidity, inline=inline_lt_liquidity)
Trend_Settings.LT_liquidity_claimed_color   := input.color(color.blue, '', group=settings_liquidity, inline=inline_lt_liquidity)
Trend_Settings.LT_liquidity_size            := input.int(2, "", options = [1,2,3,4], group=settings_liquidity, inline=inline_lt_liquidity)

Trend_Settings.liquidity_open_style         := input.string('⎯⎯⎯', 'Open          ', options = ['⎯⎯⎯', '----', '····'], group=settings_liquidity_style)
Trend_Settings.liquidity_claimed_style      := input.string('····', 'Claimed        ', options = ['⎯⎯⎯', '----', '····'], group=settings_liquidity_style)
Trend_Settings.extend                       := input.int(10, title="Extend", minval = 1, group=settings_liquidity_style)
Trend_Settings.max_lines                    := input.int(50, "Maximum number of lines", minval=1, maxval=250, group=settings_liquidity_style)

Trend_Settings.displacement_show    := input.bool(true, "Highlight    ", group=settings_displacement, inline=inline_displacement)
Trend_Settings.displacement_bull    := input.color(color.blue, "", group=settings_displacement, inline=inline_displacement)
Trend_Settings.displacement_bear    := input.color(color.orange, "", group=settings_displacement, inline=inline_displacement)

Trend_Settings.displacement_fvg     := input.bool(true, "Require FVG", group=settings_displacement)
Trend_Settings.displacement_length  := input.int(100, minval = 1, title = "Use last X bars to calculate", group=settings_displacement)
Trend_Settings.displacement_factor  := input.int(2, options = [1,2,3,4], title = "Displacement Strength", group=settings_displacement)

FVG_Settings.show                   := input.bool(true, "Show FVG    ", group=FVG_Group, inline="1")
FVG_Settings.color_bull             := input.color(color.new(color.green,90), "", group=FVG_Group, inline="1")
FVG_Settings.color_bear             := input.color(color.new(color.blue,90), "", group=FVG_Group, inline="1")
FVG_Settings.fvg_type               := input.string("Always Display", options = ["Always Display", "Same As Displacement", "Level 1", "Level 2", "Level 3", "Level 4"], group=FVG_Group, inline="1")
FVG_Settings.mitigated_show         := input.bool(true, "Show Mitigated", group=FVG_Group, inline="2")
FVG_Settings.mitigated_color_bull   := input.color(color.new(color.gray,95), "", group=FVG_Group, inline="2")
FVG_Settings.mitigated_color_bear   := input.color(color.new(color.gray,95), "", group=FVG_Group, inline="2")
FVG_Settings.mitigated_type         := input.string('Wick filled', 'when', options = ['None', 'Wick filled', 'Body filled', 'Wick filled half', 'Body filled half'], group=FVG_Group, inline="2")
FVG_Settings.CE_show                := input.bool(true, "Show C.E.    ", group=FVG_Group, inline="3")
FVG_Settings.CE_color               := input.color(color.new(color.black,60), "", group=FVG_Group, inline="3")
FVG_Settings.CE_style               := input.string('····', '     ', options = ['⎯⎯⎯', '----', '····'], group=FVG_Group, inline="3")
FVG_Settings.max_count              := input.int(20, "Maximum number of FVGs", group=FVG_Group)

VI_Settings.show                    := input.bool(true, "Show Volume Imbalance", group=Imb_Group, inline="1")
VI_Settings.color_bull              := input.color(color.new(color.red,85), "", group=Imb_Group, inline="1")
VI_Settings.color_bear              := input.color(color.new(color.red,85), "", group=Imb_Group, inline="1")
VI_Settings.mitigated_show          := input.bool(true, "Show Mitigated        ", group=Imb_Group, inline="2")
VI_Settings.mitigated_type          := 'Body filled'
VI_Settings.mitigated_color_bull    := input.color(color.new(color.red,95), "", group=Imb_Group, inline="2")
VI_Settings.mitigated_color_bear    := input.color(color.new(color.red,95), "", group=Imb_Group, inline="2")
VI_Settings.CE_show                 := false
VI_Settings.max_count               := input.int(20, "Maximum number of Imbalances", group=Imb_Group)

Gap_Settings.show                    := input.bool(true, "Show Open Gaps", group=Gap_Group, inline="1")
Gap_Settings.color_bull              := input.color(color.new(color.green,85), "", group=Gap_Group, inline="1")
Gap_Settings.color_bear              := input.color(color.new(color.green,85), "", group=Gap_Group, inline="1")
Gap_Settings.mitigated_show          := false
Gap_Settings.mitigated_type          := 'Body filled'
Gap_Settings.mitigated_color_bull    := VI_Settings.color_bull
Gap_Settings.mitigated_color_bear    := VI_Settings.color_bear
Gap_Settings.CE_show                 := false
Gap_Settings.max_count               := input.int(5, "Maximum number of Gaps", group=Gap_Group)

//+------------------------------------------------------------------------------------------------------------+//
//+--- Variables                                                                                            ---+//
//+------------------------------------------------------------------------------------------------------------+//
color color_transparent             = #ffffff00

Helper helper                       = Helper.new()

var MarketStructure Term            = MarketStructure.new("Term")
var Pivot[] ST_array                = array.new<Pivot>()
var Pivot[] IT_array                = array.new<Pivot>()
var Pivot[] LT_array                = array.new<Pivot>()
var Pivot[] STH_array               = array.new<Pivot>()
var Pivot[] ITH_array               = array.new<Pivot>()
var Pivot[] STL_array               = array.new<Pivot>()
var Pivot[] ITL_array               = array.new<Pivot>()
Term.ST                             := ST_array
Term.IT                             := IT_array
Term.LT                             := LT_array
Term.STH                            := STH_array
Term.ITH                            := ITH_array
Term.STL                            := STL_array
Term.ITL                            := ITL_array
Term.settings                       := Trend_Settings

var ImbalanceStructure FVGs         = ImbalanceStructure.new()
var Imbalance[] imbalance_array     = array.new<Imbalance>()
FVGs.imbalance                      := imbalance_array
FVGs.settings                       := FVG_Settings

var ImbalanceStructure VI           = ImbalanceStructure.new()
var Imbalance[] VI_array            = array.new<Imbalance>()
VI.imbalance                        := VI_array
VI.settings                         := VI_Settings

var ImbalanceStructure Gaps         = ImbalanceStructure.new()
var Imbalance[] Gap_array           = array.new<Imbalance>()
Gaps.imbalance                      := Gap_array
Gaps.settings                       := Gap_Settings

var label[] labels                  = array.new_label()
var line[]  lines                   = array.new_line()

body = math.abs(open - close)
std = ta.stdev(math.abs(open - close), Trend_Settings.displacement_length)

//+------------------------------------------------------------------------------------------------------------+//
//+--- Functions                                                                                            ---+//
//+------------------------------------------------------------------------------------------------------------+//
// The formatting is ugly, but Tradingview doesn't allow using barcolor inside of a function
f_highlightDisplacement() =>
    if barstate.isconfirmed
        if Trend_Settings.displacement_show
            candle_range = math.abs(open - close)
            fvg = close[1] > open[1] ? high[2] < low : low[2] > high
            Trend_Settings.displacement_fvg ? candle_range[1] > std[1] * Trend_Settings.displacement_factor and fvg : candle_range > std
        else
            false

displaced = f_highlightDisplacement()
//+------------------------------------------------------------------------------------------------------------+//
//+--- Methods                                                                                              ---+//
//+------------------------------------------------------------------------------------------------------------+//

method GetFVGDisplacementLevel(Helper helper) =>
    helper.name := "FVG Displacement Level"
    out = switch FVG_Settings.fvg_type
        'Same As Displacement' =>
            Trend_Settings.displacement_factor
        'Level 1' =>
            1
        'Level 2' =>
            2
        'Level 3' =>
            3
        'Level 4' =>
            4
    out

method LineStyle(Helper helper, string style) =>
    helper.name := style

    out = switch style
        '----' => line.style_dashed
        '····' => line.style_dotted
        => line.style_solid
    
    out

// SkipEQHigh returns the first bar_index of before the equal highs
// This is used to compare pivot points, if two at equal levels
// then we need to compare it to previous high, otherwise it will
// not identify the right STH/STL
method SkipEQHigh(Helper helper, int idx) =>
    helper.name := "Skip EQ Highs"
    i = idx
    while high[i] == high[i-1]
        i := i+1
    i
// SkipEQLows returns the first bar_index of before the equal lows
method SkipEQLow(Helper helper, int idx) =>
    helper.name := "Skip EQ Lows"
    i = idx
    while low[i] == low[i-1]
        i := i+1
    i

// Same as above, but works on pivot points. it also doesn't care for
// high or low since Pivot is a single price point
method SkipEQPivot(Pivot[] p, int idx) =>
    i = idx
    while p.get(i).price == p.get(i-1).price and p.get(i).lbl_text == p.get(i-1).lbl_text and p.size() < i
        i := i+1
    i

// isHigh returns true if the Pivot point is a high Pivot (STH, ITH, LTH)
method isHigh(Helper helper, string lbl) =>
    helper.name := "isHigh"
    str.endswith(lbl, "H")

//isLow returns true if the Pivot is a low Pivot
method isLow(Helper helper, string lbl) =>
    helper.name := "isLow"
    str.endswith(lbl, "L")

//+------------------------------------------------------------------------------------------------------------+//
//+--- ICT Market Structure Methods                                                                         ---+//
//+------------------------------------------------------------------------------------------------------------+//

// getLabelSettings calculates and returns the Pivot Point settings
// based on what is configured in the properties window
method getLabelSettings(MarketStructure MS, Pivot pivot) =>
    color   lbl_color       = na
    string  lbl_text_size   = na
    color   txt_color       = na
    string  lbl_style       = na
    string  lbl             = na

    if (MS.settings.ST_show or MS.settings.IT_show or MS.settings.LT_show)
        lbl_color               := MS.settings.ST_label_color
        lbl_text_size           := MS.settings.ST_text_size
        txt_color               := str.endswith(pivot.lbl_text,"H") ? MS.settings.ST_color_bull : MS.settings.ST_color_bear
        lbl_style               := str.endswith(pivot.lbl_text,"H") ? label.style_label_down : label.style_label_up
        lbl                     := str.endswith(pivot.lbl_text,"H") ? "STH" : "STL"

        if MS.settings.IT_show 
            lbl_color           := pivot.lbl_text == "ITH" or pivot.lbl_text == "LTH" or pivot.lbl_text == "ITL" or pivot.lbl_text == "LTL" ? MS.settings.IT_label_color : lbl_color
            lbl_text_size       := pivot.lbl_text == "ITH" or pivot.lbl_text == "LTH" or pivot.lbl_text == "ITL" or pivot.lbl_text == "LTL" ? MS.settings.IT_text_size : lbl_text_size
            txt_color           := pivot.lbl_text == "ITH" or pivot.lbl_text == "LTH"  ? MS.settings.IT_color_bull : pivot.lbl_text == "ITL" or pivot.lbl_text == "LTL" ? MS.settings.IT_color_bear : txt_color
            lbl_style           := pivot.lbl_text == "ITH" or pivot.lbl_text == "LTH"  ? label.style_label_down : pivot.lbl_text == "ITI" or pivot.lbl_text == "LTI" ? label.style_label_up : lbl_style
            lbl                 := pivot.lbl_text == "ITH" or pivot.lbl_text == "LTH" ? "ITH" : pivot.lbl_text == "ITL" or pivot.lbl_text == "LTL" ? "ITL" : lbl

        if MS.settings.LT_show
            if pivot.lbl_text == "LTH" or pivot.lbl_text == "LTL"
                lbl_color       := MS.settings.LT_label_color
                lbl_text_size   := MS.settings.LT_text_size
                txt_color       := pivot.lbl_text == "LTH"  ? MS.settings.LT_color_bull : pivot.lbl_text == "LTL" ? MS.settings.LT_color_bear : txt_color
                lbl_style       := pivot.lbl_text == "LTH"  ? label.style_label_down : pivot.lbl_text == "LTI" ? label.style_label_up : lbl_style
                lbl             := pivot.lbl_text

    [lbl_color, lbl_text_size, txt_color, lbl_style, lbl]

// AddLabel displays the Pivot label with the proper settings. it is used for both adding the initial
// Pivot and renaming/adjusting it if it is converted from STH to ITH for example
method AddLabel(MarketStructure MS, Pivot pivot) =>
    if MS.settings.ST_show or (MS.settings.IT_show and (pivot.lbl_text == "ITH" or pivot.lbl_text == "ITL" or pivot.lbl_text == "LTH" or pivot.lbl_text == "LTL")) or (MS.settings.LT_show and (pivot.lbl_text == "LTH" or pivot.lbl_text == "LTL"))
        [lbl_color, lbl_text_size, txt_color, lbl_style, lbl] = MS.getLabelSettings(pivot)

        if na(pivot.lbl)
            pivot.lbl := label.new(pivot.time, pivot.price, xloc=xloc.bar_time, color=lbl_color, size=lbl_text_size, textcolor= txt_color, style = lbl_style, text=lbl)            
            labels.unshift(pivot.lbl)
            if labels.size() >= MS.settings.max_labels
                label l = labels.pop()
                label.delete(l)
        else
            label.set_style(pivot.lbl, lbl_style)
            label.set_color(pivot.lbl, lbl_color)
            label.set_size(pivot.lbl, lbl_text_size)
            label.set_textcolor(pivot.lbl, txt_color)
            label.set_text(pivot.lbl, lbl)
        
    MS

// AddLiquidity adds the liquidity lines
method AddLiquidity(MarketStructure MS, Pivot pivot) =>
    lbl = pivot.lbl_text
    color   liquidity_open_color      = str.startswith(lbl, "ST") ? MS.settings.ST_liquidity_open_color : str.startswith(lbl, "IT") ? MS.settings.IT_liquidity_open_color : str.startswith(lbl, "LT") ? MS.settings.LT_liquidity_open_color : na
    color   liquidity_claimed_color   = str.startswith(lbl, "ST") ? MS.settings.ST_liquidity_claimed_color : str.startswith(lbl, "IT") ? MS.settings.IT_liquidity_claimed_color : str.startswith(lbl, "LT") ? MS.settings.LT_liquidity_claimed_color : na
    int     liquidity_size            = str.startswith(lbl, "ST") ? MS.settings.ST_liquidity_size : str.startswith(lbl, "IT") ? MS.settings.IT_liquidity_size : str.startswith(lbl, "LT") ? MS.settings.LT_liquidity_size : na

    if MS.settings.ST_liquidity_show or (MS.settings.IT_liquidity_show and (pivot.lbl_text == "ITH" or pivot.lbl_text == "ITL" or pivot.lbl_text == "LTH" or pivot.lbl_text == "LTL")) or (MS.settings.LT_liquidity_show and (pivot.lbl_text == "LTH" or pivot.lbl_text == "LTL"))
        if na(pivot.ln)
            pivot.ln  := line.new(pivot.time, pivot.price, time+((time-time[1])*MS.settings.extend), pivot.price, xloc=xloc.bar_time, style=helper.LineStyle(MS.settings.liquidity_open_style), color=liquidity_open_color, width=liquidity_size)
            lines.unshift(pivot.ln)
            if lines.size() > MS.settings.max_lines
                line l = lines.pop()
                line.delete(l)
        else
            line.set_color(pivot.ln, pivot.claimed ? liquidity_claimed_color : liquidity_open_color)
            line.set_width(pivot.ln, liquidity_size)
            line.set_x2(pivot.ln, pivot.claimed ? pivot.time_last : time+((time-time[1])*MS.settings.extend))
            line.set_style(pivot.ln, helper.LineStyle(pivot.claimed ? MS.settings.liquidity_claimed_style : MS.settings.liquidity_open_style))
    MS

// Add method handles the addition of newly discovered ST[H/L] Pivot Point
method Add(MarketStructure MS, float p_price, int p_time, lbl) =>
    Pivot pivot     = Pivot.new()
    pivot.price     := p_price
    pivot.time      := p_time
    pivot.lbl_text  := lbl
    
    MS.ST.unshift(pivot)
    if lbl == "STH"
        MS.STH.unshift(pivot)
    else
        MS.STL.unshift(pivot)

    MS.AddLabel(pivot).AddLiquidity(pivot)

    if MS.ST.size() > MAX_BUFFER
        Pivot temp = MS.ST.pop()
        line.delete(temp.ln)
        label.delete(temp.lbl) 
    
    MS

// FindIT rechecks the Pivot points and renames the Pivot point from ST to IT
method FindIT(MarketStructure MS) =>
    if MS.STH.size() > 2
        h1 = MS.STH.first()
        h2 = MS.STH.get(1)
        h3 = MS.STH.get(MS.STH.SkipEQPivot(2))

        if h2.price > h3.price and h2.price > h1.price and h2.lbl_text == "STH"
            h2.lbl_text := "ITH"
            MS.ITH.unshift(h2)
            MS.AddLabel(h2).AddLiquidity(h2)

    if MS.STL.size() > 2
        l1 = MS.STL.first()
        l2 = MS.STL.get(1)
        l3 = MS.STL.get(MS.STL.SkipEQPivot(2))

        if l2.price < l3.price and l2.price < l1.price and l2.lbl_text == "STL"
            l2.lbl_text := "ITL"
            MS.ITL.unshift(l2)
            MS.AddLabel(l2).AddLiquidity(l2)

    MS

// FindLT rechecks the IT Pivot points and renames the Pivot point from IT to LT
method FindLT(MarketStructure MS) =>
    if MS.ITH.size() > 2
        h1 = MS.ITH.first()
        h2 = MS.ITH.get(1)
        h3 = MS.ITH.get(MS.ITH.SkipEQPivot(2))

        if h2.price > h3.price and h2.price > h1.price and h2.lbl_text == "ITH"
            h2.lbl_text := "LTH"
            MS.LT.unshift(h2)
            MS.AddLabel(h2).AddLiquidity(h2)

    if MS.ITL.size() > 2
        l1 = MS.ITL.first()
        l2 = MS.ITL.get(1)
        l3 = MS.ITL.get(MS.ITL.SkipEQPivot(2))

        if l2.price < l3.price and l2.price < l1.price and l2.lbl_text == "ITL"
            l2.lbl_text := "LTL"
            MS.LT.unshift(l2)
            MS.AddLabel(l2).AddLiquidity(l2)
    MS

// FindST checks for ST[H/L] and calls the Add method
method FindST(MarketStructure MS) =>
    h = high[1] > high[helper.SkipEQHigh(2)] and high[1] > high
    l = low[1] < low[helper.SkipEQLow(2)] and low[1] < low

    if h
        MS.Add(high[1], time[1], "STH")
    if l
        MS.Add(low[1], time[1], "STL")
    MS

// CheckClaimed checks if liquidity has been swept
method CheckClaimed(MarketStructure MS) =>
    if MS.ST.size() > 0
        for i = 0 to MS.ST.size() - 1
            pivot = MS.ST.get(i)
            if not na(pivot.ln)
                if not pivot.claimed
                    if helper.isHigh(pivot.lbl_text)
                        pivot.claimed := high > pivot.price
                    if helper.isLow(pivot.lbl_text)
                        pivot.claimed := low < pivot.price

                    if pivot.claimed
                        pivot.time_last := time
                        pivot.claimed := true
                        MS.AddLiquidity(pivot)
                    else
                        line.set_x2(pivot.ln, time+((time-time[1])*MS.settings.extend))
    MS

//+------------------------------------------------------------------------------------------------------------+//
//+--- Imbalances Methods                                                                                   ---+//
//+------------------------------------------------------------------------------------------------------------+//
// AddZone is used to display and manage imbalance related boxes
method AddZone(ImbalanceStructure IS, Imbalance imb) =>
    if IS.settings.show
        if na(imb.box)
            imb.box := box.new(imb.open_time, imb.open, time+((time-time[1])*Trend_Settings.extend), imb.close, color_transparent, 0, bgcolor = imb.open < imb.close ? IS.settings.color_bull : IS.settings.color_bear, xloc=xloc.bar_time)
            if IS.settings.CE_show
                imb.line_middle := line.new(imb.open_time, imb.middle, time+((time-time[1])*Trend_Settings.extend), imb.middle, xloc=xloc.bar_time, style=helper.LineStyle(IS.settings.CE_style), color=IS.settings.CE_color)
            
        else
            //if imb.mitigated
            box.set_right(imb.box, imb.mitigated ? imb.mitigated_time : time+((time-time[1])*Trend_Settings.extend))
            box.set_bgcolor(imb.box, imb.open < imb.close ? imb.mitigated ? IS.settings.mitigated_color_bull : IS.settings.color_bull : imb.mitigated ? IS.settings.mitigated_color_bear : IS.settings.color_bear)
            if IS.settings.CE_show
                line.set_x2(imb.line_middle, imb.mitigated ? imb.mitigated_time : time+((time-time[1])*Trend_Settings.extend))
        
        if imb.mitigated and not IS.settings.mitigated_show
            box.delete(imb.box)
            line.delete(imb.line_middle)
    IS

// AddImbalance adds a newly discovered imbalance. this applies for both FVG and Volume Imbalance
method AddImbalance(ImbalanceStructure IS, float o, float c, int o_time, int c_time) =>
    Imbalance imb = Imbalance.new()
    imb.open_time           := o_time
    imb.close_time          := c_time
    imb.open                := o
    imb.middle              := (o+c)/2
    imb.close               := c

    IS.imbalance.unshift(imb)
    IS.AddZone(imb)

    if IS.imbalance.size() > IS.settings.max_count
        temp = IS.imbalance.pop()
        box.delete(temp.box)
        line.delete(temp.line_middle)

    IS

// CheckMitigated checks if the imbalance has been mitigated based on the settings
method CheckMitigated(ImbalanceStructure IS) =>
    if IS.imbalance.size() > 0
        for i = IS.imbalance.size() - 1 to 0
            imb = IS.imbalance.get(i)

            if not imb.mitigated
                switch IS.settings.mitigated_type
                    "None" =>
                        imb.mitigated       := false
                    'Wick filled' =>
                        imb.mitigated       := imb.open <= imb.close ? low <= imb.open : high >= imb.open
                    'Body filled' =>
                        imb.mitigated       := imb.open < imb.close ? math.min(open, close) <= imb.open : math.max(open, close) >= imb.open
                    'Wick filled half' =>
                        imb.mitigated       := imb.open <= imb.close ? low <= imb.middle : high >= imb.middle
                    'Body filled half' =>
                        imb.mitigated       := imb.open <= imb.close ? math.min(open, close) <= imb.middle : math.max(open, close) >= imb.middle
                if imb.mitigated
                    imb.mitigated_time  := time
                    IS.AddZone(imb)
                else
                    box.set_right(imb.box, time+((time-time[1])*Trend_Settings.extend))
                    if IS.settings.CE_show
                        line.set_x2(imb.line_middle, time+((time-time[1])*Trend_Settings.extend))
    IS

// FindImbalance looks for imbalances and, if found, adds it to the list
method FindImbalance(ImbalanceStructure IS, string t) =>
    if barstate.isconfirmed
        Gap = low > high[1] or high < low[1]
        if t == "GAP" and Gap
            o = high < low[1] ? low[1] : high[1]
            c = high < low[1] ? high : low
            IS.AddImbalance(o, c, time[1], time)
        if t == "FVG" and IS.settings.show and (high < low[2] or low > high[2]) and not Gap
            o = high < low[2] ? low[2] : high[2]
            c = high < low[2] ? high : low
            if FVG_Settings.fvg_type == "Always Display" or (FVG_Settings.fvg_type != "Always Display" and body[1] > std[1] * helper.GetFVGDisplacementLevel())
                IS.AddImbalance(o, c, time[2], time)

        if t == "VI" and IS.settings.show and ((math.min(open, close) > math.max(open[1], close[1])) or (math.max(open, close) < math.min(open[1], close[1]))) and not Gap
            c = math.max(open, close) > math.min(open[1], close[1]) ? math.min(open, close) : math.max(open, close)
            o = math.min(open, close) > math.max(open[1], close[1]) ? math.max(open[1], close[1]) : math.min(open[1], close[1])
            IS.AddImbalance(o, c, time[1], time)
    IS

//+------------------------------------------------------------------------------------------------------------+//
//+--- Displacement                                                                                         ---+//
//+------------------------------------------------------------------------------------------------------------+//
// Credit: FTO - original content found at
// https://www.tradingview.com/script/n6djFgQH-Visualizing-Displacement-TFO/
int offset = na
color candle = open < close ? Trend_Settings.displacement_bull : Trend_Settings.displacement_bear

if Trend_Settings.displacement_fvg
    offset := -1
    candle := open[1] < close[1] ? Trend_Settings.displacement_bull : Trend_Settings.displacement_bear

barcolor(f_highlightDisplacement() ? candle : na, offset = offset)


//+------------------------------------------------------------------------------------------------------------+//
//+--- Main call to start the process                                                                       ---+//
//+------------------------------------------------------------------------------------------------------------+//
if barstate.isconfirmed or barstate.islastconfirmedhistory
    FVGs.FindImbalance("FVG")
    VI.FindImbalance("VI")
    Gaps.FindImbalance("GAP")

FVGs.CheckMitigated()
VI.CheckMitigated()
Gaps.CheckMitigated()

if barstate.isconfirmed or barstate.islastconfirmedhistory
    Term.FindST().FindIT().FindLT()
Term.CheckClaimed()