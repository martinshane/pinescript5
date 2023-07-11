// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © chhagansinghmeena

//@version=5
indicator("Machine Learning : Cosine Similarity & Euclidean + Lorentzian Distance", overlay=true)
import chhagansinghmeena/BankNifty_CSM/16 as CSM

//====================================== Thanks to JDEHORTY For coding structure ==========//
//========== Here i am using similar coding structure that JDEHORTY used in his script =========//
//=============== There are some piece of code is similar to JDEHORTY script, Gratitude to him =============//
//===============================================================================================//

//============== For Customization Thanks to LUX ALGO idea taken from one of his script ===========

//This function appears to calculate the weighted average of a series (_src) using a rational quadratic weighting scheme. 
//It iterates over the elements of _src, assigning a weight (w) to each element based on its index and the provided parameters (_lookback and _relativeWeight).
// The weighted average is then calculated and returned as rationalQuad.
// Function to calculate the rational quadratic value based on input parameters
// Parameters:
//   - _src: Input series
//   - _lookback: Lookback period for calculating weights
//   - _relativeWeight: Relative weight factor
//   - startAtBar: Starting index for the loop (default: 0)
// Returns:
//   - Rational quadratic value based on the calculations
rationalQuadratic(_src, _lookback, _relativeWeight, startAtBar) =>
    _currentWeight = 0.0 // Initialize the current weight variable
    _cumulativeWeight = 0.0 // Initialize the cumulative weight variable
    _size = array.size(array.from(_src)) // Get the size of the input series
    for i = 0 to _size + startAtBar // Iterate over the input series
        y = _src[i] // Get the value at index i
        w = math.pow(1 + (math.pow(i, 2) / ((math.pow(_lookback, 2) * 2 * _relativeWeight))), -_relativeWeight) // Calculate the weight using the rational quadratic formula
        _currentWeight += y * w // Add the weighted value to the current weight
        _cumulativeWeight += w // Add the weight to the cumulative weight
    rationalQuad = _currentWeight / _cumulativeWeight // Calculate the rational quadratic value
    rationalQuad

//Linear interpolation involves finding a proportional value within a range based on the relative position of the original value. 
// Function to perform linear interpolation on a series
// Parameters:
//   - src: Input series for interpolation
//   - oldMax: Maximum value of the original range
//   - lookback: Number of bars to look back for calculating the minimum value (default: 100)
// Returns:
//   - Interpolated values based on the relative position in the range
get_Linear_interpolation(series float src, float oldMax , simple int lookback=100) =>
    minVal = ta.lowest(src,lookback)
    (src - minVal) / (oldMax - minVal) 

// Function to calculate get_linear_transformationd RSI using linear interpolation
// Parameters:
//   - src: Input series for RSI calculation
//   - n1: Lookback period for RSI calculation
//   - n2: Lookback period for EMA smoothing of RSI
// Returns:
//   - get_linear_transformationd RSI value using linear interpolation
n_rsi(series float src, simple int n1, simple int n2) =>
    get_Linear_interpolation(ta.ema(ta.rsi(src, n1), n2),100)

// The purpose of this code is to provide a function that calculates the KST line, which is a technical indicator used in financial analysis to identify potential trend reversals and generate trading signals.
// Function to calculate the KST (Know Sure Thing) line based on the input series
// Parameters:
//   - src: Input series
// Returns:
//   - KST line calculated using the input series
calc_kst(series float src) =>
    // Define the lengths for rate of change (ROC) calculations
    lengthROC1 = 10, lengthROC2 = 15, lengthROC3 = 20, lengthROC4 = 30
    // Calculate the rate of change (ROC) for each length 
    roc1 = ta.change(src, lengthROC1), roc2 = ta.change(src, lengthROC2), roc3 = ta.change(src, lengthROC3), roc4 = ta.change(src, lengthROC4)
    // Apply smoothing to the ROC values
    smoothed1 = ta.sma(roc1, 3), smoothed2 = ta.sma(roc2, 3), smoothed3 = ta.sma(roc3, 3), smoothed4 = ta.sma(roc4, 3)
    // Calculate the KST line
    kstLine = smoothed1 + 2 * smoothed2 + 3 * smoothed3 + 4 * smoothed4
    // Calculate the RSI (Relative Strength Index) of the KST line with a length of 14
    rsiKST = ta.rsi(kstLine, 14)
    rsiKST

// Function to perform a linear transformation on a series based on historical minimum and maximum values
// Purpose: The purpose of this code is to perform a linear transformation on a given series (src) based on the historical minimum and maximum values within a specified lookback period.
// Parameters:
//   - src: Input series to be transformed (default: close)
//   - min: Minimum value of the new range
//   - max: Maximum value of the new range
//   - lookback: Number of bars to consider for calculating historical minimum and maximum (default: 200)
// Returns:
//   - Transformed series based on the linear transformation
get_linear_transformation(series float src = close, float min, float max, int lookback = 200) => 
    // Calculate the historical minimum and maximum values within the lookback period
    _historicMin = ta.highest(nz(src), lookback) 
    _historicMax = ta.lowest(nz(src), lookback) 
    // Perform the linear transformation calculation using the formula
    linearValue = min + (max - min) * (src - _historicMin) / math.max(_historicMax - _historicMin, 10e-10)
    // Return the transformed series
    linearValue

// new approach to calculate more realistic and accurate quarditic calculation
// Purpose: Perform a sigmoidal transformation on a series based on the provided parameters
// Parameters:
// - src: The input series to be transformed
// - lookback: The number of bars to consider for the sigmoid calculation (default: 20)
// - relativeWeight: The relative weight factor for the sigmoid calculation (default: 8)
// - startAtBar: The starting bar index for the calculation (default: 25)
// Returns: The transformed value after applying the sigmoidal transformation
sigmoid(series float src, int lookback = 20, float relativeWeight = 8, int startAtBar = 25) =>
    _currentWeight = 0.0
    _cumulativeWeight = 0.0
    _size = ta.barssince(startAtBar) + 1
    for i = _size to math.max(startAtBar, lookback + startAtBar) - 1
        y = src[i]
        w = math.pow(1 + (math.pow(i - startAtBar, 2) / (math.pow(lookback, 2) * 2 * relativeWeight)), -relativeWeight)
        _currentWeight += y * w
        _cumulativeWeight += w
    sigmoidValue = _currentWeight / _cumulativeWeight
    sigmoidValue

// Calculate MACD
// Purpose: Calculate the MACD (Moving Average Convergence Divergence) value based on the input series
// Parameters:
//   src: The input series for which to calculate the MACD (default: close)
// Returns: The MACD value
macd(series float src = close) =>
    [macdLine, signalLine, _] = ta.macd(src, 12, 26, 9)
    // Calculate the linear transformation of the MACD line
    ma = get_linear_transformation(macdLine, 14, 1)
    // Calculate the linear transformation of the signal line
    sa = get_linear_transformation(signalLine, 14, 1)
    // Average the transformed MACD and signal lines
    macd_val = math.avg(ma, sa)
    macd_val

// Define input parameters
historyLookBack = input.int(title='History Lookback', defval=2000, group='General Settings', tooltip='Number of historical periods to consider for analysis.')
nearest_Probable_Distance = input.int(title='Nearest Probable Distance', defval=8, group='General Settings', tooltip='The closest distance to consider when determining probable values.')
dash_loc = input.session("Top Right", "Stats Location", options=["Top Right", "Bottom Right", "Top Left", "Bottom Left", "Middle Right", "Bottom Center"], group='General Settings', inline="Stats Location", tooltip='The position of the statistics display on the chart.')
text_size = input.session('Small', "Stats Size", options=["Tiny", "Small", "Normal", "Large"], group='General Settings', inline="Stats Location", tooltip='The size of the text used for the statistics display.')
trenSelection = input.string(title='Moving Average Selection', options=['CPMA', 'FRMA', 'RationalQuad'], defval='RationalQuad', group='Moving Averages', tooltip='The type of moving average to use for trend analysis.')
cpmaLength = input.int(title="CPMA Length", defval=9, group='Moving Averages', tooltip='The length of the Centered Price Moving Average (CPMA) used for trend analysis.')
frmaLength = input.int(title="FRMA Length", defval=14, group='Moving Averages', tooltip='The length of the Fractal Adaptive Moving Average (FRMA) used for trend analysis.')
enableFilter = input.bool(title="Enable Trend Filter", defval=true, group='Filter', tooltip='Enable or disable the trend filter for signal processing, which provides greater accuracy.')
isRational = input.bool(title="Use Rational smoothing", defval=true, group='Filter', tooltip='Enable or disable the rational smoothing function for the selected moving average, used as a trend filter.')
isRationalSigma = input.bool(title="Use Sigmoid smoothing ", defval=true, group='Filter', tooltip='Enable or disable the sigmoid smoothing function, which works in conjunction with the rational smoothing function.')
methodSelection = input.string(title='Distance Calculation', options=['Lorentzian', 'Euclidean', 'Cosine similarity'], defval='Cosine similarity', group='Machine Learning : Methods', tooltip='The method used for calculating the distance similarity or dissimilarity when processing signals using machine learning techniques.')
start = input.time(title='Start Date', defval=timestamp('01 Jan 2023 03:30 +0000'), group='Backtesting', tooltip='The date and time to begin trading from during the backtesting period.')
finish = input.time(title='End Date', defval=timestamp('1 Jan 2099 15:30 +0000'), group='Backtesting', tooltip='The date and time to stop trading during the backtesting period.')

// Define variables for the position of the table and the text size
var table_position = dash_loc == 'Top Left' ? position.top_left :
  dash_loc == 'Bottom Left' ? position.bottom_left :
  dash_loc == 'Middle Right' ? position.middle_right :
  dash_loc == 'Bottom Center' ? position.bottom_center :
  dash_loc == 'Top Right' ? position.top_right : position.bottom_right
  
var table_text_size = text_size == 'Tiny' ? size.tiny :
  text_size == 'Small' ? size.small :
  text_size == 'Normal' ? size.normal : size.large

// Get CPMA(Conceptive Price Moving Average) and FRMA(Fractional Moving Average)
CPMA = CSM.CSM_CPMA(length = cpmaLength)
FRMA = CSM.frama_Calculation(close,length = frmaLength)

type FeatureArrays
    array<float> f1
    array<float> f2
    array<float> f3
    array<float> f4
    array<float> f5
    array<float> f6

type FeatureSeries
    float f1
    float f2
    float f3
    float f4
    float f5
    float f6

series_from(feature_string, _close, _high, _low, _hlc3, f_paramA, f_paramB) =>
    switch feature_string
        "RSI" => n_rsi(_close, f_paramA, f_paramB)    
        "KST" => get_Linear_interpolation(calc_kst(src = _close),100)
        "CPMA"=> get_linear_transformation(CPMA,14,1)
        "VWAP"=> get_linear_transformation(ta.vwap(_close),14,1)
        "FRAMA"=> get_linear_transformation(FRMA,14,1) 
        "MACD"=> macd(_close)     
        
featureSeries = 
 FeatureSeries.new(
   series_from("CPMA", close, high, low, hlc3, 0, 0), // f1
   series_from("RSI", close, high, low, hlc3, 14, 1), // f2
   series_from("VWAP", close, high, low, hlc3, 0, 0), // f3
   series_from("KST", close, high, low, hlc3, 0, 0),  // f4
   series_from("FRAMA", close, high, low, hlc3, 0, 0), // f5
   series_from("MACD", close, high, low, hlc3, 0, 0)  // f6
 )

var f1Array = array.new_float()
var f2Array = array.new_float()
var f3Array = array.new_float()
var f4Array = array.new_float()
var f5Array = array.new_float()
var f6Array = array.new_float()

array.push(f1Array, featureSeries.f1)
array.push(f2Array, featureSeries.f2)
array.push(f3Array, featureSeries.f3)
array.push(f4Array, featureSeries.f4)
array.push(f5Array, featureSeries.f5)
array.push(f6Array, featureSeries.f6)


featureArrays = 
 FeatureArrays.new(
  f1Array, // f1
  f2Array, // f2
  f3Array, // f3
  f4Array, // f4
  f5Array,  // f5
  f6Array  // f6
 )

// Calculate RQK value for historical data
rqkValue = isRationalSigma ? sigmoid(close) : rationalQuadratic(close, 8, 0.5, 25)

// Purpose: Calculate the Euclidean distance between a given feature series and the corresponding feature arrays at index 'i'.
// Parameters:
// i: The index at which to calculate the distance.
// featureSeries: An instance of the FeatureSeries structure containing feature values.
// featureArrays: An instance of the FeatureArrays structure containing arrays of feature values.
// Returns: The Euclidean distance between the feature series and feature arrays.
get_euclidean_distance(int i, FeatureSeries featureSeries, FeatureArrays featureArrays) =>
    distance = 0.0
    distance += math.pow(featureSeries.f1 - array.get(featureArrays.f1, i), 2)
    distance += math.pow(featureSeries.f2 - array.get(featureArrays.f2, i), 2)
    distance += math.pow(featureSeries.f4 - array.get(featureArrays.f4, i), 2)
    distance += math.pow(featureSeries.f5 - array.get(featureArrays.f5, i), 2)
    distance += math.pow(featureSeries.f6 - array.get(featureArrays.f6, i), 2)
    
    if str.tonumber(timeframe.period) <= 20
        distance += math.pow(featureSeries.f3 - array.get(featureArrays.f3, i), 2)

    math.sqrt(distance)

// Purpose: Calculate the Lorentzian distance between a given feature series and the corresponding feature arrays at index 'i'.
// Lorentzian distance is a measure of dissimilarity between two vectors, considering the absolute difference between their corresponding elements.
// It is calculated as the sum of the logarithm of one plus the absolute difference for each feature.
// Use For: Comparing the dissimilarity between feature series and feature arrays in the context of trading signals or other applications.
// Parameters:
// i: The index at which to calculate the distance.
// featureSeries: An instance of the FeatureSeries structure containing feature values.
// featureArrays: An instance of the FeatureArrays structure containing arrays of feature values.
// Returns: The Lorentzian distance between the feature series and feature arrays.
// A higher value indicates higher dissimilarity, while a lower value indicates higher similarity.
get_lorentzian_distance(int i, FeatureSeries featureSeries, FeatureArrays featureArrays) =>
    distance = 0.0
    distance += math.log(1+math.abs(featureSeries.f1 - array.get(featureArrays.f1, i)))
    distance += math.log(1+math.abs(featureSeries.f2 - array.get(featureArrays.f2, i)))
    distance += math.log(1+math.abs(featureSeries.f4 - array.get(featureArrays.f4, i)))
    distance += math.log(1+math.abs(featureSeries.f5 - array.get(featureArrays.f5, i)))
    distance += math.log(1+math.abs(featureSeries.f6 - array.get(featureArrays.f6, i)))
    
    if str.tonumber(timeframe.period) <= 20
        distance += math.log(1+math.abs(featureSeries.f3 - array.get(featureArrays.f3, i)))

    math.sqrt(distance)

// Purpose: Calculate the cosine similarity between a given feature series and the corresponding feature arrays at index 'i'.
// Cosine similarity is a measure of similarity between two non-zero vectors of an inner product space.
// It measures the cosine of the angle between the vectors, indicating their directional similarity.
// The closer the cosine similarity value is to 1, the more similar the vectors are.
// Use For: Comparing the similarity between feature series and feature arrays in the context of trading signals or other applications.
// Parameters:
// i: The index at which to calculate the similarity.
// featureSeries: An instance of the FeatureSeries structure containing feature values.
// featureArrays: An instance of the FeatureArrays structure containing arrays of feature values.
// Returns: The cosine similarity between the feature series and feature arrays.
// A value close to 1 indicates high similarity, while a value close to 0 indicates low similarity or dissimilarity.
get_cosine_similarity(i, featureSeries, featureArrays) =>
    dotProduct = 0.0
    magnitudeSeries = 0.0
    magnitudeArray = 0.0

    dotProduct += featureSeries.f1 * array.get(featureArrays.f1, i)
    dotProduct += featureSeries.f2 * array.get(featureArrays.f2, i)
    dotProduct += featureSeries.f4 * array.get(featureArrays.f4, i)
    dotProduct += featureSeries.f5 * array.get(featureArrays.f5, i)
    dotProduct += featureSeries.f6 * array.get(featureArrays.f6, i)

    magnitudeSeries +=  math.pow(featureSeries.f1, 2)
    magnitudeSeries +=  math.pow(featureSeries.f2, 2)
    magnitudeSeries +=  math.pow(featureSeries.f4, 2)
    magnitudeSeries +=  math.pow(featureSeries.f5, 2)
    magnitudeSeries +=  math.pow(featureSeries.f6, 2)

    magnitudeArray += math.pow(array.get(featureArrays.f1, i), 2)
    magnitudeArray += math.pow(array.get(featureArrays.f2, i), 2)
    magnitudeArray += math.pow(array.get(featureArrays.f4, i), 2)
    magnitudeArray += math.pow(array.get(featureArrays.f5, i), 2)
    magnitudeArray += math.pow(array.get(featureArrays.f6, i), 2)

    if str.tonumber(timeframe.period) <= 20
        dotProduct += featureSeries.f3 * array.get(featureArrays.f3, i)
        magnitudeSeries +=  math.pow(featureSeries.f3, 2)
        magnitudeArray += math.pow(array.get(featureArrays.f3, i), 2)

    magnitudeSeries := math.sqrt(magnitudeSeries)
    magnitudeArray := math.sqrt(magnitudeArray)

    if magnitudeSeries == 0.0 or magnitudeArray == 0.0
        0.0
    else
        dotProduct / (magnitudeSeries * magnitudeArray)

// Purpose: Perform machine learning logic to calculate distances and predictions based on the selected method.
// Use For: Analyzing and predicting signals in trading or other applications using machine learning techniques.
maxBarsBackIndex = last_bar_index >= historyLookBack ? last_bar_index - historyLookBack : 0

src = close
y_train_series = src[4] < src[0] ? -1 : src[4] > src[0] ? 1 : 0
var y_train_array = array.new_int(0)

// Variables used for ML Logic
var predictions = array.new_float(0)
var prediction = 0.
var signal = 0
var distances = array.new_float(0)

array.push(y_train_array, y_train_series)

lastDistance = -1.0
size = math.min(historyLookBack-1, array.size(y_train_array)-1)
sizeLoop = math.min(historyLookBack-1, size)

// Purpose: Get the machine learning distance based on the selected method.
// Parameters:
//   i: The index at which to calculate the distance.
// Returns: The distance value based on the selected method (Lorentzian, Euclidean, or Cosine).
get_ML_Distance(i) =>
    switch
        methodSelection == 'Lorentzian' => get_lorentzian_distance(i, featureSeries, featureArrays)
        methodSelection == 'Euclidean' => get_euclidean_distance(i, featureSeries, featureArrays)
        methodSelection == 'Cosine similarity' => get_cosine_similarity(i, featureSeries, featureArrays)

// Purpose: Perform machine learning logic to calculate distances and predictions based on the selected method.
// Use For: Analyzing and predicting signals in trading or other applications using machine learning techniques.
// Parameters: None
// Returns: None
if bar_index >= maxBarsBackIndex
    for i = 0 to sizeLoop
        d = get_ML_Distance(i)
        // Compare the distance with the last recorded distance and check if the index is divisible by 4.
        if d >= lastDistance and i % 4
            lastDistance := d
            // Store the distance and corresponding prediction in arrays.
            array.push(distances, d)
            array.push(predictions, math.round(array.get(y_train_array, i)))
            // Check if the number of predictions exceeds the desired count.
            if array.size(predictions) > nearest_Probable_Distance
                // Update the last recorded distance to account for the shifting of arrays.
                lastDistance := array.get(distances, math.round(nearest_Probable_Distance * 3 / 4))
                // Remove the oldest distance and prediction from the arrays.
                array.shift(distances)
                array.shift(predictions)

    // Calculate the final prediction by summing up the predictions.
    prediction := array.sum(predictions)

// Purpose: Get the trend value based on the selected trend selection method.
// Returns: The calculated trend value.
getTrend() =>
    switch
        trenSelection == 'CPMA' => isRational ? isRationalSigma ? sigmoid(CPMA) : rationalQuadratic(CPMA, 8, 0.5, 25) : CPMA
        trenSelection == 'FRMA' => isRational ? isRationalSigma ? sigmoid(FRMA) : rationalQuadratic(FRMA, 8, 0.5, 25) : FRMA
        trenSelection == 'RationalQuad' => rqkValue

trend = getTrend()

// Determine if the current price is bullish or bearish relative to the trend.
bool isBullishSmooth = close >= trend
bool isBearishSmooth = close <= trend

// Get the gradient color and plot the trend line.
[avgrationalQuad, plotColor] = CSM.getGradientColor(isFirstbar = barstate.isfirst, src = trend, length = trenSelection == 'CPMA' ? cpmaLength : trenSelection == 'FRMA' ? frmaLength : 14, isSmoothed = false)
plot(avgrationalQuad, color=plotColor, linewidth=2, title="Trend")


// Filtered Signal: The model's prediction of future price movement direction with user-defined filters applied
signal := prediction > 0 and (enableFilter ? isBullishSmooth : true) ? 1 : prediction < 0 and (enableFilter ? isBearishSmooth : true) ? -1 : nz(signal[1])
// Check if the signal type has changed
isDifferentSignalType = ta.change(signal)
// Check if there is an early signal flip
isEarlySignalFlip = ta.change(signal) and (ta.change(signal[1]) or ta.change(signal[2]) or ta.change(signal[3]))
// Check if the signal indicates a buy
isBuySignal = signal == 1
// Check if the signal indicates a sell
isSellSignal = signal == -1
// Check if a new buy signal has occurred with a different signal type
isNewBuySignal = isBuySignal and isDifferentSignalType
// Check if a new sell signal has occurred with a different signal type
isNewSellSignal = isSellSignal and isDifferentSignalType

// Purpose: Get the color associated with a prediction value.
// Parameters:
// prediction: The prediction value for which to retrieve the color.
// Returns: An array containing the color and index associated with the prediction value.
get_PredictionColor(prediction) =>
    arrColor = array.new_color(0)
    array.push(arrColor, #FF0000)  // 0
    array.push(arrColor, #FF1000)  // 1
    array.push(arrColor, #FF2000)  // 2
    array.push(arrColor, #FF3000)  // 3
    array.push(arrColor, #FF4000)  // 4
    array.push(arrColor, #FF5000)  // 5
    array.push(arrColor, #FF6000)  // 6
    array.push(arrColor, #FF7000)  // 7
    array.push(arrColor, #FF8000)  // 8
    array.push(arrColor, #FF9000)  // 9
    array.push(arrColor, #0AAA00)  // 10
    array.push(arrColor, #1BBB10)  // 11
    array.push(arrColor, #2CCC20)  // 12
    array.push(arrColor, #3DDD30)  // 13
    array.push(arrColor, #5EEE50)  // 14
    array.push(arrColor, #6FFF60)  // 15
    array.push(arrColor, #7ABF70)  // 16
    array.push(arrColor, #8BCF80)  // 17
    array.push(arrColor, #9CDF90)  // 18
    array.push(arrColor, #90DFF9)  // 19

    distVal = prediction >= 10 or prediction <= -10 ? isNewSellSignal ? -10 : 9 : prediction
    index = int(distVal + 10)
    predictColor = array.get(arrColor, index)
    [predictColor, index]

[predictColor, index] = get_PredictionColor(prediction)  // Retrieve the color and index based on the prediction value
plotshape(isNewBuySignal ? low : na, 'Buy', shape.labelup, location.belowbar, color=predictColor, size=size.small, offset=0)  // Plot a 'Buy' label shape with the predicted color
plotshape(isNewSellSignal ? high : na, 'Sell', shape.labeldown, location.abovebar, color=predictColor, size=size.small, offset=0)  // Plot a 'Sell' label shape with the predicted color

// Function: window
// Purpose: Check if the current time is within the specified start and finish time range.
// Returns: True if the current time is within the range, false otherwise.
window() =>
    time >= start and time <= finish ? true : false
    
longSignal = false
shortSignal = false

if window()
    longSignal := isNewBuySignal  // Set longSignal to the value of isNewBuySignal if the current time is within the window
    shortSignal := isNewSellSignal  // Set shortSignal to the value of isNewSellSignal if the current time is within the window

lastSignalWasBullish = ta.barssince(longSignal) < ta.barssince(shortSignal)  // Check if the last signal was bullish
lastSignalWasBearish = ta.barssince(shortSignal) < ta.barssince(longSignal)  // Check if the last signal was bearish
barsSinceRedEntry = ta.barssince(shortSignal)  // Count the number of bars since the last short entry signal
barsSinceRedExit = ta.barssince(isBullishSmooth)  // Count the number of bars since the last bearish exit signal
barsSinceGreenEntry = ta.barssince(longSignal)  // Count the number of bars since the last long entry signal
barsSinceGreenExit = ta.barssince(isBearishSmooth)  // Count the number of bars since the last bullish exit signal
isValidShortExit = barsSinceRedExit > barsSinceRedEntry  // Check if the current short exit signal is valid
isValidLongExit = barsSinceGreenExit > barsSinceGreenEntry  // Check if the current long exit signal is valid
endLongTradeDynamic = (isBullishSmooth and isValidLongExit[1])  // Check if the current condition indicates the end of a long trade
endShortTradeDynamic = (isBearishSmooth and isValidShortExit[1])  // Check if the current condition indicates the end of a short trade

// Function: winRate
// Purpose: Calculate the win rate percentage based on the number of winning trades and total trades.
// Parameters:
// winTrades: The number of winning trades.
// totalTrades: The total number of trades.
// Returns: The win rate percentage.
winRate(winTrades, totalTrades) => winTrades / totalTrades * 100

// Function: winRatio
// Purpose: Calculate the win-loss ratio percentage based on the number of winning trades and losing trades.
// Parameters:
// winTrades: The number of winning trades.
// lossTrades: The number of losing trades.
// Returns: The win-loss ratio percentage.
winRatio(winTrades, lossTrades) => winTrades / (winTrades + lossTrades) * 100

//====================================== Thanks to JDEHORTY For the backtesting approach==========//
//===============================================================================================//

// Function: backtest
// Purpose: Perform the backtesting calculations and return trade statistics.
// Parameters:
// high, low, open: Price data inputs for the backtest.
// startLongTrade, endLongTrade, startShortTrade, endShortTrade, isEarlySignalFlip: Signals and flags for the trades.
// maxBarsBackIndex, thisBarIndex: Bar index data for backtesting.
// src: Source data for market price.
// useWorstCase: Flag to determine if worst-case scenario should be considered for market price.
// window: Flag to indicate if the current time is within the specified window.
backtest(high, low, open, startLongTrade, endLongTrade, startShortTrade, endShortTrade, isEarlySignalFlip, maxBarsBackIndex, thisBarIndex, src, useWorstCase, window) =>
    marketPrice = useWorstCase ? src : (high + low + open + open) / 4
    var float start_long_trade = na
    var float start_short_trade = na
    var float total_long_profit = 0.0
    var float total_short_profit = 0.0
    var int wins = 0
    var int losses = 0
    var int trade_count = 0
    var int early_signal_flip_count = 0
    var bool tookProfit = false
    lot_size = 1

    if window
        trade_count := 0
        wins := 0
        losses := 0
        early_signal_flip_count := 0

        if startLongTrade
            start_short_trade := na
            early_signal_flip_count := isEarlySignalFlip ? 1 : 0
            start_long_trade := marketPrice
            trade_count := 1

        if endLongTrade
            delta = marketPrice - start_long_trade
            wins := delta > 0 ? 1 : 0
            losses := delta < 0 ? 1 : 0
            total_long_profit := delta * lot_size

        if startShortTrade
            start_long_trade := na
            start_short_trade := marketPrice
            trade_count := 1

        if endShortTrade
            early_signal_flip_count := isEarlySignalFlip ? 1 : 0
            delta = start_short_trade - marketPrice
            wins := delta > 0 ? 1 : 0
            losses := delta < 0 ? 1 : 0
            total_short_profit := delta * lot_size

    tradeStatsHeader = "📈 CSM Strategy Stats (Assumption)"
    longProfit = ta.cum(total_long_profit)
    shortProfit = ta.cum(total_short_profit)
    longShortProfit = longProfit + shortProfit
    totalEarlySignalFlips = ta.cum(early_signal_flip_count)
    totalWins = ta.cum(wins)
    totalLosses = ta.cum(losses)
    totalTrades = ta.cum(wins + losses)
    winLossRatio = totalWins / totalTrades
    winRate = totalWins / (totalWins + totalLosses)

    [totalWins, totalLosses, totalEarlySignalFlips, totalTrades, tradeStatsHeader, winLossRatio, winRate]

// Perform the backtest and store the trade statistics
[totalWins, totalLosses, totalEarlySignalFlips, totalTrades, tradeStatsHeader, winLossRatio, winRate] = backtest(high, low, open, longSignal, endLongTradeDynamic, shortSignal, endShortTradeDynamic, isEarlySignalFlip, maxBarsBackIndex, bar_index, trend, false, window())

// The following can be used to display real-time trade stats. This can be a useful mechanism for obtaining real-time feedback during Feature Engineering. This does NOT replace the need to properly backtest.
// Note: In this context, a "Stop-Loss" is defined instances where the ML Signal prematurely flips directions before an exit signal can be generated.
//[totalWins, totalLosses, totalEarlySignalFlips, totalTrades, tradeStatsHeader, winLossRatio, winRate] = ml.backtest(high, low, open, longSignal, endLongTradeDynamic, shortSignal, endShortTradeDynamic, isEarlySignalFlip, maxBarsBackIndex, bar_index, trend, false)
// Plotting the trade statistics in a table
tableData2 = table.new(table_position, columns=2, rows=7, frame_color=color.new(color.black, 100), frame_width=1, border_width=1, border_color= color.new(color.black, 100))
update_table(tbl, tradeStatsHeader, totalTrades, totalWins, totalLosses, winLossRatio, winRate, stopLosses) => 
    c_transparent = color.new(color.black, 100)
    table.cell(tbl, 0, 0, tradeStatsHeader, text_halign=text.align_center, text_color=color.teal, text_size=table_text_size)
    table.cell(tbl, 0, 1, 'Winrate', text_halign=text.align_center, bgcolor=c_transparent, text_color=color.teal, text_size=table_text_size)
    table.cell(tbl, 1, 1, str.tostring(totalWins / totalTrades, '#.#%'), text_halign=text.align_center, bgcolor=c_transparent, text_color=color.teal, text_size=table_text_size)
    table.cell(tbl, 0, 2, 'Trades', text_halign=text.align_center, bgcolor=c_transparent, text_color=color.teal, text_size=table_text_size)
    table.cell(tbl, 1, 2, str.tostring(totalTrades, '#') + ' (' + str.tostring(totalWins, '#') + '|' + str.tostring(totalLosses, '#') + ')', text_halign=text.align_center, bgcolor=c_transparent, text_color=color.teal, text_size=table_text_size)
    table.cell(tbl, 0, 5, 'WL Ratio', text_halign=text.align_center, bgcolor=c_transparent, text_color=color.teal, text_size=table_text_size)
    table.cell(tbl, 1, 5, str.tostring(totalWins / totalLosses, '0.00'), text_halign=text.align_center, bgcolor=c_transparent, text_color=color.teal, text_size=table_text_size)
    table.cell(tbl, 0, 6, 'Early Signal Flips', text_halign=text.align_center, bgcolor=c_transparent, text_color=color.teal, text_size=table_text_size)
    table.cell(tbl, 1, 6, str.tostring(totalEarlySignalFlips, '#'), text_halign=text.align_center, bgcolor=c_transparent, text_color=color.teal, text_size=table_text_size)

if barstate.islast
    update_table(tableData2, tradeStatsHeader, totalTrades, totalWins, totalLosses, winRatio(totalWins,totalTrades), winRate(totalWins,totalLosses), totalEarlySignalFlips)
