// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © vnhilton

//Note: 1 LIMITATION ASSUMES THAT DAYS CLOSED AT SAME PRICE AS ITS OPEN ARE GREEN DAYS, & THAT 0% GAPS ARE GAP UPS

//@version=4
study("Gap Size Outcome Statistics [vnhilton]",shorttitle="Gap Size Outcome Statistics", overlay = true)

//Table column & row headers
var tble = table.new(position.bottom_left, 7, 3, border_width = 1)
table.cell(tble, 1, 0, "Threshold Value", bgcolor=color.blue, text_size=size.normal)
table.cell(tble, 2, 0, "Sample Size", bgcolor=color.blue, text_size=size.normal)
table.cell(tble, 3, 0, "Green Day", bgcolor=color.blue, text_size=size.normal)
table.cell(tble, 4, 0, "Red Day", bgcolor=color.blue, text_size=size.normal)
table.cell(tble, 5, 0, "Average Historical Gap", bgcolor=color.blue, text_size=size.normal)
table.cell(tble, 6, 0, "Historical Total Gap Sample Size", bgcolor=color.blue, text_size=size.normal)
table.cell(tble, 0, 0, " ", bgcolor=color.blue, text_size=size.normal)
table.cell(tble, 0, 1, ">", bgcolor=color.blue, text_size=size.normal)
table.cell(tble, 0, 2, "<", bgcolor=color.blue, text_size=size.normal)

//Gap inputs
greaterThres = input(1.00, minval=0.0, title="(GAP UP) | Input % >= 0")
lessThres = input(-1.00, minval=-100.0, maxval=-0.01, title="(GAP DOWN) | Input % < 0")

//Average gap up
avgGapUp = ((open[1] - close[2]) * 100 / close[2]) >= 0 ? 1 : 0                                            // Gap up
avgGapUSum = cum(avgGapUp ? ((open[1] - close[2]) * 100 / close[2]) : 0)                                   // Update gap up summation in percent
avgGapUCounter = cum(avgGapUp ? 1 : 0)                                                                     // Update gap up counter
avgGapUPercent = round(avgGapUSum / avgGapUCounter, 2)                                                     // Average gap up percent

//Average gap down
avgGapDown = ((open[1] - close[2]) * 100 / close[2]) < 0 ? 1 : 0                                           // Gap down
avgGapDSum = cum(avgGapDown ? ((open[1] - close[2]) * 100 / close[2]) : 0)                                 // Update gap down summation in percent
avgGapDCounter = cum(avgGapDown ? 1 : 0)                                                                   // Update gap down counter
avgGapDPercent = round(avgGapDSum / avgGapDCounter, 2)                                                     // Average gap down percent

//Average gap outputs
table.cell(tble, 5, 1, tostring(avgGapUPercent) + "%", bgcolor = color.orange, text_size = size.normal)    // Place 'Average gap up' in table
table.cell(tble, 5, 2, tostring(avgGapDPercent) + "%", bgcolor = color.orange, text_size = size.normal)    // Place 'Average gap down' in table
table.cell(tble, 6, 1, tostring(avgGapUCounter) + " Gap Ups", bgcolor = color.rgb(150, 75, 0), text_size = size.normal) // Place 'Average gap up counter' in table
table.cell(tble, 6, 2, tostring(avgGapDCounter) + " Gap Downs", bgcolor = color.rgb(150, 75, 0), text_size = size.normal) // Place 'Average gap down counter' in table

//Gap up
gapUpGreen = ((open[1] - close[2]) * 100 / close[2]) >= greaterThres and close[1] >= open[1] ? 1 : 0       // Gapped up & ended day in green
gapUpRed = ((open[1] - close[2]) * 100 / close[2]) >= greaterThres and close[1] < open[1] ? 1 : 0          // Gapped up & ended day in red
gUGCounter = cum(gapUpGreen ? 1 : 0)                                                                       // Update gap up green day cummulative counter                                                                  
gUDCounter = cum(gapUpRed ? 1 : 0)                                                                         // Update gap up red day cummulative counter

gUGPercent = round(gUGCounter * 100 / (gUGCounter + gUDCounter), 2)                                        // Gapped up & green day end percent
gUDPercent = round(gUDCounter * 100 / (gUGCounter + gUDCounter), 2)                                        // Gapped up & red day end percent
gUSamples  = gUGCounter + gUDCounter                                                                       // Total gap up events 

//Gap up table outputs
table.cell(tble, 1, 1, tostring(greaterThres), bgcolor = color.white, text_size = size.normal)             // Place threshold value in table
table.cell(tble, 3, 1, tostring(gUGPercent) + "%", bgcolor = color.green, text_size = size.normal)         // Place 'Gapped up & green day end percent' in table
table.cell(tble, 4, 1, tostring(gUDPercent) + "%", bgcolor = color.red, text_size = size.normal)           // Place 'Gapped up & red day end percent' in table
table.cell(tble, 2, 1, tostring(gUSamples), bgcolor = color.gray, text_size = size.normal)                 // Place 'Total gap up events' in table

//Gap down
gapDownRed = ((open[1] - close[2]) * 100 / close[2]) <= lessThres and close[1] < open[1] ? 1 : 0           // Gapped down & ended day in red
gapDownGreen = ((open[1] - close[2]) * 100 / close[2]) <= lessThres and close[1] >= open[1] ? 1 : 0        // Gapped down & ended day in green
gDRCounter = cum(gapDownRed ? 1 : 0)                                                                       // Update gap down red day cummulative counter    
gDGCounter = cum(gapDownGreen ? 1 : 0)                                                                     // Update gap down green day cummulative counter  

gDRPercent = round(gDRCounter * 100 / (gDRCounter + gDGCounter), 2)                                        // Gapped down & red day end percent
gDGPercent = round(gDGCounter * 100 / (gDRCounter + gDGCounter), 2)                                        // Gapped down & green day end percent
gDSamples = gDRCounter + gDGCounter                                                                        // Total gap down events 

//Gap down table outputs
table.cell(tble, 1, 2, tostring(lessThres), bgcolor = color.white, text_size = size.normal)                // Place threshold value in table
table.cell(tble, 4, 2, tostring(gDRPercent) + "%", bgcolor = color.red, text_size = size.normal)           // Place 'Gapped down & red day end percent' in table
table.cell(tble, 3, 2, tostring(gDGPercent) + "%", bgcolor = color.green, text_size = size.normal)         // Place 'Gapped down & green day end percent' in table
table.cell(tble, 2, 2, tostring(gDSamples), bgcolor = color.gray, text_size = size.normal)                 // Place 'Total gap down events' in table