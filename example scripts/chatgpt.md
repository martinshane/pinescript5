//@version=5
indicator("My Script", overlay=true)

// Define the range of possible values for each setting
var int neighborsCountRange = 1 to 390
var int maxBarsBack = 2500 // fixed value
var int featureCountRange = 2 to 4
var int RSIParameterARange = 1 to 100
var int RSIParameterBRange = 1 to 100
var int WTParameterARange = 1 to 100
var int WTParameterBRange = 1 to 100
var int CCIParameterARange = 1 to 100
var int CCIParameterBRange = 1 to 100
var int ADXParameterARange = 1 to 100
var int ADXParameterBRange = 1 to 100
var float regimeThresholdRange = -99.99 to 100
var int adxThresholdRange = 1 to 100
var int emaPeriodRange = 1 to 200
var int smaPeriodRange = 1 to 200
var int lookBackWindowRange = 1 to 200
var int relativeWeightingRange = 1 to 50
var int regressionLevelRange = 1 to 100
var int enhanceKernelSmoothingLagRange = 1 to 25
var bool showDefaultExitsRange = [false, true]
var bool useDynamicExitsRange = [false, true]
var bool useVolatilityFilterRange = [false, true]
var bool useRegimeFilterRange = [false, true]
var bool useAdxFilterRange = [false, true]
var bool useEmaFilterRange = [false, true]
var bool useSmaFilterRange = [false, true]
var bool tradeWithKernelRange = [false, true]
var bool showKernelEstimateRange = [false, true]

// Initialize the best settings and best profit
var int bestNeighborsCount = na
var int bestFeatureCount = na
var int bestRSIParameterA = na
var int bestRSIParameterB = na
var int bestWTParameterA = na
var int bestWTParameterB = na
var int bestCCIParameterA = na
var int bestCCIParameterB = na
var int bestADXParameterA = na
var int bestADXParameterB = na
var float bestRegimeThreshold = na
var int bestAdxThreshold = na
var int bestEmaPeriod = na
var int bestSmaPeriod = na
var int bestLookBackWindow = na
var int bestRelativeWeighting = na
var int bestRegressionLevel = na
var int bestEnhanceKernelSmoothingLag = na
var bool bestShowDefaultExits = na
var bool bestUseDynamicExits = na
var bool bestUseVolatilityFilter = na
var bool bestUseRegimeFilter = na
var bool bestUseAdxFilter = na
var bool bestUseEmaFilter = na
var bool bestUseSmaFilter = na
var bool bestTradeWithKernel = na
var bool bestShowKernelEstimate = na
var float bestProfit = na

// Cycle through all possible combinations of settings
for neighborsCount = neighborsCountRange
    for featureCount = featureCountRange
        for RSIParameterA = RSIParameterARange
            for RSIParameterB = RSIParameterBRange
                for WTParameterA = WTParameterARange
                    for WTParameterB = WTParameterBRange
                        for CCIParameterA = CCIParameterARange
                            for CCIParameterB = CCIParameterBRange
                                for ADXParameterA = ADXParameterARange
                                    for ADXParameterB = ADXParameterBRange
                                        for regimeThreshold = regimeThresholdRange
                                            for adxThreshold = adxThresholdRange
                                                for emaPeriod = emaPeriodRange
                                                    for smaPeriod = smaPeriodRange
                                                        for lookBackWindow = lookBackWindowRange
                                                            for relativeWeighting = relativeWeightingRange
                                                                for regressionLevel = regressionLevelRange
                                                                    for enhanceKernelSmoothingLag = enhanceKernelSmoothingLagRange
                                                                        for showDefaultExits in showDefaultExitsRange
                                                                            for useDynamicExits in useDynamicExitsRange
                                                                                for useVolatilityFilter in useVolatilityFilterRange
                                                                                    for useRegimeFilter in useRegimeFilterRange
                                                                                        for useAdxFilter in useAdxFilterRange
                                                                                            for useEmaFilter in useEmaFilterRange
                                                                                                for useSmaFilter in useSmaFilterRange
                                                                                                    for tradeWithKernel in tradeWithKernelRange
