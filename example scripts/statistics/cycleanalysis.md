//@version=5
indicator(
  title = "Cycles Analysis", 
  shorttitle = "", 
  overlay = true, 
  max_labels_count=500,
  max_lines_count = 500)

//VERSION HISTORY
// >=1 - first working version
// >=2 
//    - added decimals to % direction change
//    - fixed an exception when there are 0 periods
//    - changed the indicator to work real time and with replays
//    - split percent change for bull and bear markets
//    - added ability to calculate duration periods in Days or Candles
//    - added minimum period duration
var revision = 2

//#region ======Consts======
string GROUP_MAIN = "Main"
string GROUP_TWEAKS = "Tweaks"
string GROUP_CUSTOM = "Custom"
string GROUP_DURATION = "How to present durations?"
string GROUP_BENNER = "Benner"
string GROUP_VISUAL = "Other Visuals"

var int DAYS_IN_WEEK = 7
var int MS_IN_DAY = 1000*60*60*24
var int BACKGROUND_TRANSPARENCY = 90

var string DIRECTION_BULL = "Bull"
var string DIRECTION_BEAR = "Bear"
var string DIRECTION_BOTH = "Both"
var string DIRECTION_NONE = "None"
var string CREST_PEAK = "Peak"
var string CREST_TROUGH = "Trough"
var string DIRECTION_CHANGE_PERCENTAGE = "Percentage"
var string DIRECTION_CHANGE_DURATION = "Duration"
var string DIRECTION_CHANGE_BOTH = "Both"
var string TIME_DURATION_DAYS = "Days"
var string TIME_DURATION_CANDLES = "Candles"
//#endregion

//#region ======Inputs======
var int revisionInput = input.int(defval = revision, options = [revision], title = "Version", tooltip = "Ignore it", group = GROUP_MAIN)
var float changeInDirectionPercentsInput = input.float(defval = 30, title = "Change in Direction, %", minval = 0.1, maxval = 100, tooltip = 'How many % has to happen for us to decide that the market has started moving up', group=GROUP_MAIN)

var string changeInDirectionModeInput = input.string(defval = DIRECTION_CHANGE_PERCENTAGE, options = [DIRECTION_CHANGE_PERCENTAGE, DIRECTION_CHANGE_DURATION, DIRECTION_CHANGE_BOTH], title = "Direction Change Mode", tooltip = "Should we identify directions by % change or duration?", group = GROUP_TWEAKS)
var string durationUnitsInput = input.string(defval = TIME_DURATION_DAYS, options = [TIME_DURATION_DAYS, TIME_DURATION_CANDLES], title = "Duration Units", tooltip = "You might want to use Days for higher TFs, and Candles for lower", group = GROUP_TWEAKS)
var bool useSeparatePercentageForBearChangeInput = input.bool(defval = false, title = "Use Separate Value for Bear Percentage Change", tooltip = 'Just in case if you think 20% up != 20% down', group = GROUP_TWEAKS)
var float changeInDirectionPercentsBearInput = input.float(defval = 30, title = "Change in Bear Direction, %", minval = 0.1, maxval = 100, tooltip = 'How many % has to happen for us to decide that the market has started moving down', group=GROUP_TWEAKS)
var int changeInDirectionUnitsInput = input.int(defval = 182, title = "Change in Direction, time", minval = 1, maxval = 365, tooltip = 'How many days has to happen without a new top/bottom for us to decide that the market has change direction', group=GROUP_TWEAKS)
var string enforceMinimumPeriodDurationModeInput = input.string(defval = DIRECTION_NONE, options = [DIRECTION_NONE, DIRECTION_BOTH, DIRECTION_BULL, DIRECTION_BEAR], title = "Enforce Minimum Period Duration For", tooltip = "Use this if you want to remove pull backs from the analysis", group = GROUP_TWEAKS)
var int minimumPeriodDurationUnitsInput = input.int(defval = 90, title = "Minimum Period Duration, time", minval = 1, maxval = 365, tooltip = 'The minimum duration in time units for each period', group=GROUP_TWEAKS)
var string firstPeriodDirectionInput = input.string(defval = DIRECTION_BULL, options = [DIRECTION_BULL, DIRECTION_BEAR], title = "First Period Direction", tooltip = "Likely you know which direction the first custom period went", group = GROUP_TWEAKS)
var bool showStagnationInput = input.bool(defval = false, title = "Show Stagnations", group = GROUP_TWEAKS)
var int stagnationUnitsInput = input.int(defval = 182, title = "Stagnation, time", minval = 0, maxval = 1000, tooltip = 'How many days has to happen without a new top/bottom for us to decide that the market is stagnating', group=GROUP_TWEAKS)
var bool showPredictionZoneInput = input.bool(defval = true, title = "Show Predictions Zone", group = GROUP_TWEAKS)
var bool calculatePercentileInput = input.bool(defval = true, title = "Calculate Percentile", tooltip = 'Use percentiles to remove fat tails. You need to have at least 3 full periods to be able to calculate perceniles.', group = GROUP_TWEAKS)
var int percentileInput = input.int(defval = 80, title = "Percentile", minval = 1, maxval = 99, group=GROUP_TWEAKS)
var bool showAggregatesInput = input.bool(defval = true, title = "Show Aggeragates Table", group = GROUP_TWEAKS)

var bool useCustomStartRangeInput = input.bool(defval = false, title = "Use Custom Start Date", tooltip = 'Just in case if you want to analyse only a subset of data', group = GROUP_CUSTOM)
var int rangeStartDateInput = input.time(defval = 0, title = "Range Start Date", tooltip = "Start of the analysis period", group = GROUP_CUSTOM)
var bool useCustomEndRangeInput = input.bool(defval = false, title = "Use Custom End Date", tooltip = 'Just in case if you want to analyse only a subset of data', group = GROUP_CUSTOM)
var int rangeEndDateInput = input.time(defval = 0, title = "Range End Exit", tooltip = "End of the analysis period", group = GROUP_CUSTOM)

var bool durationInYearsInput = input.bool(defval = true, title = "Years", group = GROUP_DURATION, inline = GROUP_DURATION)
var bool durationInMonthsInput = input.bool(defval = true, title = "Month", group = GROUP_DURATION, inline = GROUP_DURATION)
var bool durationInDaysInput = input.bool(defval = true, title = "Days", group = GROUP_DURATION, inline = GROUP_DURATION)
var bool durationInHoursInput = input.bool(defval = false, title = "Hours", group = GROUP_DURATION, inline = GROUP_DURATION)
var bool durationInMinutesInput = input.bool(defval = false, title = "Minutes", group = GROUP_DURATION, inline = GROUP_DURATION)
var bool durationInSecondsInput = input.bool(defval = false, title = "Seconds", group = GROUP_DURATION, inline = GROUP_DURATION)

var bool showBackgroundInput = input.bool(defval = true, title = "Show Background", group = GROUP_VISUAL)
var bool showLablesInput = input.bool(defval = true, title = "Show Lables", group = GROUP_VISUAL)
var bool showNewHighsLowsInput = input.bool(defval = false, title = "Show New Highs/Lows", group = GROUP_VISUAL)
var bool showReversalDetectionPointInput = input.bool(defval = true, title = "Show the point when the direction change was detected", group = GROUP_VISUAL)
//#endregion

//#region ======Types======
type crestType
    string crestCategory
    int crestTime
    int crestBar
    float price
    int durationMs
    float changePercentage
    line marker
    int totalStagnation = 0

type periodType
    string direction 
    float curHighPrice
    int curHighDate
    int curHighBar
    float curLowPrice
    int curLowDate
    int curLowBar
    int totalStagnation = 0

type iterationDataType
    string printText = ""
    string printYLocation = ""

    bool newHigh = false
    bool newLow = false
    bool displayLastCrest = false
    bool stagnationFinished = false
    int stagnationStart = na
    int stagnationEnd = na
    int stagnatioDurationMs = na
    float stagnationCrest = na
    float stagnationChangePercent = na

    float bennerCrest = na
    color bennerColor = na

type resultTableType
    string title
    string valueBull
    string valueBear

type calcVisualsType
    line finalLine = na
    linefill finalFill = na
    box predictionBox = na
    label predictionMean = na
    label predictionMedian = na
//#endregion

//#region ======Long term vars======
var array<crestType> crests = array.new<crestType>()

var periodType period = periodType.new()

var calcVisuals = calcVisualsType.new(
  finalLine = line.new(na, close, na, close + 1, xloc.bar_index, extend.both, color.gray),
  finalFill = linefill.new(na, na, na),
  predictionBox = box.new(left = na, top = na, right = na, bottom = na, border_color = na, border_width = 1, border_style = line.style_solid, extend = extend.none, xloc = xloc.bar_time, bgcolor = na),
  predictionMean = label.new(x = na, y = na, text = "X", xloc = xloc.bar_time, yloc = yloc.price, color = color(na), style = label.style_none, textcolor = na, size = size.normal, textalign = text.align_center),
  predictionMedian = label.new(x = na, y = na, text = "O", xloc = xloc.bar_time, yloc = yloc.price, color = color(na), style = label.style_none, textcolor = na, size = size.normal, textalign = text.align_center))

var bool usePercentageChange = changeInDirectionModeInput == DIRECTION_CHANGE_BOTH or changeInDirectionModeInput == DIRECTION_CHANGE_PERCENTAGE
var bool useDurationChange = changeInDirectionModeInput == DIRECTION_CHANGE_BOTH or changeInDirectionModeInput == DIRECTION_CHANGE_DURATION

var bool enforceMinimumDurationForBull = enforceMinimumPeriodDurationModeInput == DIRECTION_CHANGE_BOTH or enforceMinimumPeriodDurationModeInput == DIRECTION_BULL
var bool enforceMinimumDurationForBear = enforceMinimumPeriodDurationModeInput == DIRECTION_CHANGE_BOTH or enforceMinimumPeriodDurationModeInput == DIRECTION_BEAR
//#endregion

//#region ======Short term vars======
iterationDataType iterationData = iterationDataType.new()
//#endregion

//#region ======Functions======
addPrintText(txt, string yPosition = yloc.price) =>
    string newText = iterationData.printText
    if (newText != "")
        newText += "\n"
    newText += str.tostring(txt)
    iterationData.printText := newText
    iterationData.printYLocation := yPosition

print(bool display = true) =>
    if (iterationData.printText != "" and display)
        lbl = label.new(bar_index, high, iterationData.printText, xloc.bar_index, iterationData.printYLocation, color(na), label.style_none, color.gray, size.normal, text.align_left)

calculateDifferenceBetween(duration) =>
    yearsBetween = (duration) / (1000*60*60*24*360)
    monthsBetween = (yearsBetween - math.floor(yearsBetween)) * 12
    daysBetween = (monthsBetween - math.floor(monthsBetween)) * 30
    hoursBetween = (daysBetween - math.floor(daysBetween)) * 24
    minutesBetween = (hoursBetween - math.floor(hoursBetween)) * 60
    secondsBetween = (minutesBetween - math.floor(minutesBetween)) * 60
    [math.floor(yearsBetween), math.floor(monthsBetween), math.floor(daysBetween), math.floor(hoursBetween), math.floor(minutesBetween), math.floor(secondsBetween)]

calculateDifferenceInUnits(startTime, endTime, startBar, endBar) =>
    float result = durationUnitsInput == TIME_DURATION_DAYS ? math.round((endTime - startTime) / MS_IN_DAY) : endBar - startBar

calculateDifferenceInPercentage(start, end) =>
    math.abs(end * 100 / start - 100)

isInTimePeriod() =>
    bool afterBeginning = (useCustomStartRangeInput == false or time >= rangeStartDateInput)
    bool beforeEnd = (useCustomEndRangeInput == false or time <= rangeEndDateInput)
    afterBeginning and beforeEnd

formatDuration(duration) =>
    [yearsBetween, monthsBetween, daysBetween, hoursBetween, minutesBetween, secondsBetween] = calculateDifferenceBetween(duration)
    array<string> result = array.new_string(0)
    if durationInYearsInput
        result.push(str.tostring(yearsBetween) + "y")
    else 
        monthsBetween := monthsBetween + 12 * yearsBetween
    if durationInMonthsInput
        result.push(str.tostring(monthsBetween) + "m")
    else 
        daysBetween := daysBetween + 30 * monthsBetween
    if durationInDaysInput
        result.push(str.tostring(daysBetween) + "d")
    else 
        hoursBetween := hoursBetween + 24 * daysBetween
    if durationInHoursInput
        result.push(str.tostring(hoursBetween) + "h")
    else 
        minutesBetween := minutesBetween + 60 * hoursBetween
    if durationInMinutesInput
        result.push(str.tostring(minutesBetween) + "min")
    else 
        secondsBetween := secondsBetween + 60 * minutesBetween
    if durationInSecondsInput
        result.push(str.tostring(secondsBetween) + "sec")
 
    array.join(result, ", ")

addResultDurationsData(resultData, bullDurations, bearDurations, titlePrefix = "") =>
    resultData.push(resultTableType.new(title = titlePrefix + "Shortest", valueBull = str.tostring(formatDuration(array.min(bullDurations))), valueBear = str.tostring(formatDuration(array.min(bearDurations)))))
    resultData.push(resultTableType.new(title = titlePrefix + "Longest", valueBull = str.tostring(formatDuration(array.max(bullDurations))), valueBear = str.tostring(formatDuration(array.max(bearDurations)))))
    resultData.push(resultTableType.new(title = titlePrefix + "Median Duraion", valueBull = str.tostring(formatDuration(array.median(bullDurations))), valueBear = str.tostring(formatDuration(array.median(bearDurations)))))
    resultData.push(resultTableType.new(title = titlePrefix + " Mean Duraiton", valueBull = str.tostring(formatDuration(array.avg(bullDurations))), valueBear = str.tostring(formatDuration(array.avg(bearDurations)))))

addResultChangesData(resultData, bullChanges, bearChanges, titlePrefix = "") =>
    resultData.push(resultTableType.new(title = titlePrefix + "Smallest Change", valueBull = str.tostring(array.min(bullChanges), format.percent), valueBear = str.tostring(array.min(bearChanges), format.percent)))
    resultData.push(resultTableType.new(title = titlePrefix + "Biggest Change", valueBull = str.tostring(array.max(bullChanges), format.percent), valueBear = str.tostring(array.max(bearChanges), format.percent)))
    resultData.push(resultTableType.new(title = titlePrefix + "Median Change", valueBull = str.tostring(array.median(bullChanges), format.percent), valueBear = str.tostring(array.median(bearChanges), format.percent)))
    resultData.push(resultTableType.new(title = titlePrefix + "Mean Change", valueBull = str.tostring(array.avg(bullChanges), format.percent), valueBear = str.tostring(array.avg(bearChanges), format.percent)))

addResultData(resultData, bullChanges, bullDurations, bearChanges, bearDurations, titlePrefix = "") =>
    addResultDurationsData(resultData, bullDurations, bearDurations, titlePrefix)
    addResultChangesData(resultData, bullChanges, bearChanges, titlePrefix)

calculatePercentileItemsToRemove(arr) =>
    int itemsToRemove = 0
    int size = array.size(arr)
    if (size >= 3)
        float halfPsize = size * (1 - percentileInput / 100) / 2
        //we have to remove at least one item from each side. That's why we need at least 3 items in the original array.
        if (halfPsize < 1)
            halfPsize := 1
        itemsToRemove := math.floor(halfPsize)
    itemsToRemove

getPeriodColor() =>
    period.direction == DIRECTION_BEAR ? color.red : color.green
//#endregion

//#region ======Input check======
if useCustomStartRangeInput and useCustomEndRangeInput and rangeStartDateInput > rangeEndDateInput
    runtime.error("Range Start Date should be before Range End Date")

//#endregion

//#region ======Init======
//initialisation when:
//no custom date selected and this is the first bar
//or there is custom date and (we just entered this range or this is the first date and the range start is before the first date)
if (useCustomStartRangeInput == false and bar_index == 0) or (useCustomStartRangeInput == true and (time[1] < rangeStartDateInput and time >= rangeStartDateInput) or (bar_index == 0 and rangeStartDateInput < time))
    //we need the first crest calculate the distance to the next proper crest
    string firstCrestCategory = firstPeriodDirectionInput == DIRECTION_BULL ? CREST_TROUGH : CREST_PEAK
    crestType firstCrest = crestType.new(crestCategory = CREST_TROUGH, crestTime = time, crestBar = bar_index, price = low, durationMs = 0, changePercentage = 0)
    if showBackgroundInput
        line start = line.new(bar_index, close, bar_index, close + 1, xloc.bar_index, extend.both, color.gray)
        firstCrest.marker := start
    crests.push(firstCrest)
    period.direction := firstPeriodDirectionInput
    period.curHighPrice := high
    period.curHighDate := time
    period.curHighBar := bar_index
    period.curLowPrice := low
    period.curLowDate := time
    period.curLowBar := bar_index
//#endregion

//#region ======Main logic======
if (isInTimePeriod())
    crestType prevCrest = crests.last()

    int prevHighDate = period.curHighDate
    int prevHighBar = period.curHighBar
    int prevLowDate = period.curLowDate
    int prevLowBar = period.curLowBar
    if (high > period.curHighPrice)
        period.curHighPrice := high
        period.curHighDate := time
        period.curHighBar := bar_index
        iterationData.newHigh := true
    if (low < period.curLowPrice)
        period.curLowPrice := low
        period.curLowDate := time
        period.curLowBar := bar_index
        iterationData.newLow := true

    if period.direction == DIRECTION_BULL
        if iterationData.newHigh
            iterationData.stagnationCrest := period.curLowPrice
            period.curLowPrice := low
            period.curLowDate := time
            period.curLowBar := bar_index
            //we calculate stagnations even if we don't show them, for that reason we can't use showStagnationInput here
            if stagnationUnitsInput > 0 and calculateDifferenceInUnits(prevHighDate, time, prevHighBar, bar_index) >= stagnationUnitsInput
                iterationData.stagnationFinished := true
                iterationData.stagnationStart := prevHighDate
                iterationData.stagnationEnd := period.curHighDate
                iterationData.stagnationChangePercent := calculateDifferenceInPercentage(period.curHighPrice, iterationData.stagnationCrest) 
                iterationData.stagnatioDurationMs := iterationData.stagnationEnd - iterationData.stagnationStart
                period.totalStagnation += iterationData.stagnatioDurationMs
        
        //change in direction
        //we change if we get a new low that is % lower than the current high
        //and it can't happen on the same candle when we got a new high (it's possible if we are on a higher timeframe like monthly and the whole candle is bigger than % selected)  
        //or if it's been X days without new high
        //also we have to check that we've passed the minimum period duration
        float bearChangePercentage = useSeparatePercentageForBearChangeInput ? changeInDirectionPercentsBearInput : changeInDirectionPercentsInput
        if (
          (iterationData.newHigh == false and usePercentageChange == true and low < period.curHighPrice * (1 - bearChangePercentage/100)) 
          or (useDurationChange == true and calculateDifferenceInUnits(period.curHighDate, time, period.curHighBar, bar_index) >= changeInDirectionUnitsInput)
          )
          and
          (enforceMinimumDurationForBear == false or calculateDifferenceInUnits(period.curHighDate, period.curLowDate, period.curHighBar, period.curLowBar) >= minimumPeriodDurationUnitsInput)
            //add a new crest record
            
            int duration = period.curHighDate - prevCrest.crestTime
            float distance = calculateDifferenceInPercentage(prevCrest.price, period.curHighPrice)
            crestType newCrest = crestType.new(crestCategory = CREST_PEAK, crestTime = period.curHighDate, crestBar = bar_index, price = period.curHighPrice, durationMs = duration, changePercentage = distance, totalStagnation = period.totalStagnation)
            crests.push(newCrest)

            //display the last crest
            iterationData.displayLastCrest := true

            //reset the period value
            period.direction := DIRECTION_BEAR
            period.curHighPrice := high
            period.curHighDate := time
            period.curHighBar := bar_index
            period.totalStagnation := 0
            
    else // in bear now
        if iterationData.newLow
            iterationData.stagnationCrest := period.curHighPrice
            period.curHighPrice := high
            period.curHighDate := time
            period.curHighBar := bar_index
            //we calculate stagnations even if we don't show them, for that reason we can't use showStagnationInput here
            if stagnationUnitsInput > 0 and calculateDifferenceInUnits(prevLowDate, time, prevLowBar, bar_index) >= stagnationUnitsInput
                iterationData.stagnationFinished := true
                iterationData.stagnationStart := prevLowDate
                iterationData.stagnationEnd := period.curLowDate
                iterationData.stagnationChangePercent := calculateDifferenceInPercentage(period.curLowPrice, iterationData.stagnationCrest) 
                iterationData.stagnatioDurationMs := iterationData.stagnationEnd - iterationData.stagnationStart
                period.totalStagnation += iterationData.stagnatioDurationMs
        //change in direction
        //we change if we get a new high that is % higher than the current low
        //and it can't happen on the same candle when we got a new low (it's possible if we are on a higher timeframe like monthly and the whole candle is bigger than % selected)  
        //or if it's been X days without new low
        //also we have to check that we've passed the minimum period duration
        if (
          (iterationData.newLow == false and usePercentageChange == true and high > period.curLowPrice * (1 + changeInDirectionPercentsInput/100)) 
          or 
          (useDurationChange == true and calculateDifferenceInUnits(period.curLowDate, time, period.curLowBar, bar_index) >= changeInDirectionUnitsInput)
          )
          and
          (enforceMinimumDurationForBull == false or calculateDifferenceInUnits(period.curLowDate, period.curHighDate, period.curLowBar, period.curHighBar) >= minimumPeriodDurationUnitsInput)
            //add a new crest record
            int duration = period.curLowDate - prevCrest.crestTime
            float distance = calculateDifferenceInPercentage(prevCrest.price, period.curLowPrice)
            crestType newCrest = crestType.new(crestCategory = CREST_TROUGH, crestTime = period.curLowDate, crestBar = bar_index, price = period.curLowPrice, durationMs = duration, changePercentage = distance, totalStagnation = period.totalStagnation)
            crests.push(newCrest)

            //display the last crest
            iterationData.displayLastCrest := true

            //reset the period value
            period.direction := DIRECTION_BULL
            period.curLowPrice := low
            period.curLowDate := time
            period.curLowBar := bar_index
            period.totalStagnation := 0
//#endregion

//#region ======Visuals======
//show a new high was found
plotshape(showNewHighsLowsInput ? iterationData.newHigh : na, location = location.abovebar, style=shape.triangleup, color = color.green)

//show a new low was found
plotshape(showNewHighsLowsInput ? iterationData.newLow : na, location = location.belowbar, style=shape.triangledown, color = color.red)

if (iterationData.displayLastCrest)

    crestType lastCrest = crests.last()
    string yLoc = yloc.belowbar
    string lblStyle = label.style_label_up
    color lineColor = color.red
    if lastCrest.crestCategory == CREST_PEAK
        yLoc := yloc.abovebar
        lblStyle := label.style_label_down
        lineColor := color.green
    
    if showLablesInput
        string fromCrest = "n/a"
        //we need at least 3 crests to figure out the distance to the previous crest of the same direction
        if (array.size(crests) > 2)
            crestType lastSameDirectionCrest = crests.get(array.size(crests)-3)
            fromCrest := formatDuration(lastCrest.crestTime - lastSameDirectionCrest.crestTime)
        string crestTxt = str.format("Price: {0}\nChange: {1}\nDuration: {2}\nFrom Crest: {3}\nTotal Stagnation: {4}", 
          str.tostring(lastCrest.price, format.mintick), 
          str.tostring(lastCrest.changePercentage, format.percent), 
          formatDuration(lastCrest.durationMs),
          fromCrest,
          formatDuration(lastCrest.totalStagnation))
        label.new(lastCrest.crestTime, 0, crestTxt, xloc=xloc.bar_time, yloc=yLoc, color=color.new(lineColor, 30), style=lblStyle)

    //paint the area with appropriate color
    crestType lastButOneCrest = crests.get(array.size(crests)-2)
    if showBackgroundInput
        lastCrest.marker := line.new(lastCrest.crestTime, close, lastCrest.crestTime, close + 1, xloc.bar_time, extend.both, lineColor)
        linefill.new(lastCrest.marker, lastButOneCrest.marker, color.new(lineColor, BACKGROUND_TRANSPARENCY))

    
//show a new crest was found
plotshape(showReversalDetectionPointInput ? iterationData.displayLastCrest : na, location = location.belowbar, style=shape.diamond, color = color.fuchsia, size = size.tiny)

//show stagnation
if (showStagnationInput and iterationData.stagnationFinished)
    color lineColor = color.gray
    if showBackgroundInput
        line stagnationStartLine = line.new(iterationData.stagnationStart, close, iterationData.stagnationStart, close + 1, xloc.bar_time, extend.both, lineColor)
        line stagnationEndLine = line.new(iterationData.stagnationEnd, close, iterationData.stagnationEnd, close + 1, xloc.bar_time, extend.both, lineColor)
        linefill.new(stagnationStartLine, stagnationEndLine, color.new(lineColor, 80))
    if showLablesInput
        string stagTxt = str.format("Duration: {0}\nChange: {1}", 
          formatDuration(iterationData.stagnatioDurationMs),
          str.tostring(iterationData.stagnationChangePercent, format.percent))
        int stagPosition = iterationData.stagnationEnd - math.round((iterationData.stagnationEnd - iterationData.stagnationStart)/2)
        label.new(stagPosition, 0, stagTxt, xloc=xloc.bar_time, yloc=yloc.belowbar, color=color.new(lineColor, 30), style=label.style_label_center)
//#endregion


//#region ======Last Close======
if ((useCustomEndRangeInput == false) or (useCustomEndRangeInput == true and time >= rangeStartDateInput and time <= rangeEndDateInput)) and barstate.islast
    if showBackgroundInput
        color lineColor = getPeriodColor()
        crestType lastCrest = crests.last()
        line.set_x1(calcVisuals.finalLine, bar_index)
        line.set_x2(calcVisuals.finalLine, bar_index)
        //becase linefill is between lastcrest and the last line, and lastcrest might change any time - we have to delete the fill before refilling
        //it would be possible to make it faster by comparing the fill's fist line with the lastcrest and deleting only if they are different, but the speed is not too important in this setup, hence opting out for simplicity 
        linefill.delete(calcVisuals.finalFill)
        calcVisuals.finalFill := linefill.new(lastCrest.marker, calcVisuals.finalLine, color.new(lineColor, BACKGROUND_TRANSPARENCY))

    //the first element is a default fake crest, so we ignore it
    array<float> bullChanges = array.new_float(0)
    array<float> bearChanges = array.new_float(0)
    array<int> bullDurations = array.new_int(0)
    array<int> bearDurations = array.new_int(0)
    if array.size(crests) > 1
        for i = 1 to array.size(crests) - 1
            crestType curCrest = array.get(crests, i)
            if (curCrest.crestCategory == CREST_PEAK)
                bullChanges.push(curCrest.changePercentage)
                bullDurations.push(curCrest.durationMs)
            else
                bearChanges.push(curCrest.changePercentage)
                bearDurations.push(curCrest.durationMs)
    bullChanges.sort(order.ascending)
    bearChanges.sort(order.ascending)
    bullDurations.sort(order.ascending)
    bearDurations.sort(order.ascending)

    array<resultTableType> resultData = array.new<resultTableType>()
    resultData.push(resultTableType.new(title = "", valueBull = "Bull", valueBear = "Bear"))
    resultData.push(resultTableType.new(title = "Periods", valueBull = str.tostring(array.size(bullChanges)), valueBear = str.tostring(array.size(bearChanges))))
    addResultData(resultData, bullChanges, bullDurations, bearChanges, bearDurations)

    array<float> bullPercentileChanges = array.new_float(0)
    array<float> bearPercentileChanges = array.new_float(0)
    array<int> bullPercentileDurations = array.new_int(0)
    array<int> bearPercentileDurations = array.new_int(0)
    //we need at least 3 items for percentile
    if calculatePercentileInput
        int percentileItemsRemoveBull = calculatePercentileItemsToRemove(bullDurations)
        int percentileItemsRemoveBear = calculatePercentileItemsToRemove(bearDurations)
        string pPrefix = "P" + str.tostring(percentileInput) + " "
        if array.size(bullChanges) > 0
            bullPercentileChanges := array.slice(bullChanges, percentileItemsRemoveBull, array.size(bullChanges) - percentileItemsRemoveBull)
        if array.size(bearChanges) > 0
            bearPercentileChanges := array.slice(bearChanges, percentileItemsRemoveBear, array.size(bearChanges) - percentileItemsRemoveBear)
        if array.size(bullDurations) > 0
            bullPercentileDurations := array.slice(bullDurations, percentileItemsRemoveBull, array.size(bullDurations) - percentileItemsRemoveBull)
        if array.size(bearDurations) > 0    
            bearPercentileDurations := array.slice(bearDurations, percentileItemsRemoveBear, array.size(bearDurations) - percentileItemsRemoveBear)
        resultData.push(resultTableType.new(title = pPrefix + "Duration Periods", valueBull = str.tostring(array.size(bullDurations) - 2 * percentileItemsRemoveBull), valueBear = str.tostring(array.size(bearDurations) - 2 * percentileItemsRemoveBear)))
        addResultData(resultData, bullPercentileChanges, bullPercentileDurations, bearPercentileChanges, bearPercentileDurations, pPrefix)
    table results = table.new(position.top_right, 3, array.size(resultData), frame_width = 1, frame_color = color.black, border_color = color.black, border_width = 1, bgcolor = color.white)

    if showAggregatesInput
        for [i, row] in resultData
            table.cell(results, 0, i, text = row.title, text_halign = text.align_right)
            table.cell(results, 1, i, text = row.valueBull, text_halign = text.align_left)
            table.cell(results, 2, i, text = row.valueBear, text_halign = text.align_left)

    if showPredictionZoneInput
        color boxColor = getPeriodColor()
        crestType lastCrest = crests.last()
        array<float> changesArr = array.new_float(0)
        array<int> durationsArr = array.new_int(0)
        int directionSign = period.direction == DIRECTION_BULL ? 1 : -1
        if period.direction == DIRECTION_BULL 
            if calculatePercentileInput == true
                durationsArr := bullPercentileDurations
                changesArr := bullPercentileChanges
            else //
                durationsArr := bullDurations
                changesArr := bullChanges
        else //bear
            if calculatePercentileInput == true
                durationsArr := bearPercentileDurations
                changesArr := bearPercentileChanges
            else //
                durationsArr := bearDurations
                changesArr := bearChanges

        int predictionLeft = lastCrest.crestTime + array.min(durationsArr)
        int predictionRight = lastCrest.crestTime + array.max(durationsArr)
        float predictionBottom = lastCrest.price * (1 + directionSign * array.min(changesArr) / 100)
        float predictionTop = lastCrest.price * (1 + directionSign * array.max(changesArr) / 100)
        int predictionMeanX = lastCrest.crestTime + array.avg(durationsArr)
        int predictionMedianX = lastCrest.crestTime + array.median(durationsArr)
        float predictionMeanY = lastCrest.price * (1 + directionSign * array.avg(changesArr) / 100)
        float predictionMedianY = lastCrest.price * (1 + directionSign * array.median(changesArr) / 100)
        box.set_lefttop(calcVisuals.predictionBox, left = predictionLeft, top = predictionTop)
        box.set_rightbottom(calcVisuals.predictionBox, right = predictionRight, bottom = predictionBottom)
        box.set_bgcolor(calcVisuals.predictionBox, color.new(boxColor, BACKGROUND_TRANSPARENCY))
        box.set_border_color(calcVisuals.predictionBox, boxColor)
        label.set_xy(calcVisuals.predictionMean, x = predictionMeanX, y = predictionMeanY)
        label.set_textcolor(calcVisuals.predictionMean, boxColor)
        label.set_xy(calcVisuals.predictionMedian, x = predictionMedianX, y = predictionMedianY)
        label.set_textcolor(calcVisuals.predictionMedian, boxColor)
//#endregion

//#region ======Iteration Close======
print()
//#endregion