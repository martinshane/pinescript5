//@version=5
strategy("Simple EMA Crossover Strategy", overlay=true)

// Input for moving average periods
shortPeriod = input.int(15, "Short MA Period")
longPeriod = input.int(16, "Long MA Period")

// Calculate the moving averages
shortMA = ta.ema(close, shortPeriod)
longMA = ta.ema(close, longPeriod)

// Buy condition: short MA crosses above long MA
buyCondition = ta.crossover(shortMA, longMA)

// Sell condition: short MA crosses below long MA
sellCondition = ta.crossunder(shortMA, longMA)

// Execute the strategy orders
if buyCondition
    strategy.entry("Buy", strategy.long)

if sellCondition
    strategy.close("Buy")

// Plot the moving averages
plot(shortMA, color=color.yellow)
plot(longMA, color=color.blue)