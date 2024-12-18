// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © mouadtagner

//@version=5
strategy("A2", overlay=true, margin_long=100, margin_short=100, process_orders_on_close = true)
showsignals = input(title="Show Buy/Sell Signals ?", defval=true)
highlighting = input(title="Highlighter On/Off ?", defval=true)
//sma1
src1=input(ohlc4,"sma1source")
leng1=input(1,"sma1length")
smatimeframe1=input.timeframe('D',"sma1 timeframe")
sma1= ta.sma(src1,leng1)
SMA1 = request.security(syminfo.tickerid, smatimeframe1, sma1)
//sma14
src14=input(ohlc4,"sma14source")
leng14=input(14,"sma14length")
smatimeframe14=input.timeframe('D',"sma14 timeframe")
sma14= ta.sma(src14,leng14)
SMA14 = request.security(syminfo.tickerid, smatimeframe14, sma14)
// Get daily high, low, and close prices
dailyHigh = request.security(syminfo.tickerid, "D", high)
dailyLow = request.security(syminfo.tickerid, "D", low)
dailyClose = request.security(syminfo.tickerid, "D", close)
// Calculate Pivot Point, R1, R2, R3, stopeR4, S1, S2, S3, stopeS4 levels with Fibonacci retracement
pivot = (dailyHigh + dailyLow + dailyClose) / 3
prange = dailyHigh - dailyLow

R1 = pivot + 0.382 * prange
R2 = pivot + 0.618 * prange
R3 = pivot + 1.000 * prange
stopeR4 = pivot + 1.000 * prange + 0.0001*40
profitR5= R2 - R3 + R2 - 0.0001 * 40

S1 = pivot - 0.382 * prange
S2 = pivot - 0.618 * prange
S3 = pivot - 1.000 * prange
stopeS4 = pivot - 1.000 * prange - 0.0001 * 40
profitS5= S2 - S3 + S2 + 0.0001 * 40

//STOPANDPROFIT
stoplosslong = (low <=stopeS4) 
stoplossshort = (high >=stopeR4) 
takeprofitlong = (high >= profitS5)
takeprofitshort = (low <=profitR5)
//djnnncnj
var float lastDayOfWeek = na
var bool canTrade = na

// Get the day of the week (0 for Sunday, 1 for Monday, and so on)
currentDayOfWeek = dayofweek(time)

// Check if it's a new trading day (new day of the week)
if (na(lastDayOfWeek) or lastDayOfWeek != currentDayOfWeek)
    canTrade := true
// Get the current time
currentHour = hour
currentMinute = minute
//CONDITIONS
longCondition = ((SMA1> SMA14) and (low <= S2) and (currentHour <10 and currentHour >= 1))
if (longCondition and canTrade)
    strategy.entry("Long", strategy.long,when=longCondition)
    canTrade := false
        //shortCondition
shortCondition = ((SMA1< SMA14) and (high >= R2) and (currentHour <10 and currentHour >= 1))
if (shortCondition and canTrade)
    strategy.entry("short", strategy.short,when=shortCondition)
    canTrade := false
// Store the current day of the week for comparison on the next bar
lastDayOfWeek := currentDayOfWeek
//exitcondition
exitlongCondition = stoplosslong or takeprofitlong or (currentHour >= 15 and currentMinute >= 30)
if (exitlongCondition)
    strategy.close("Long")
        
exitshortCondition = stoplossshort or takeprofitshort or (currentHour >= 15 and currentMinute >= 30)
if (exitshortCondition)
    strategy.close("short")

// Plotting pivot points Fibonacci levels and sma
plot(R2, title="R2", color=color.green)
plot(R3, title="R3", color=color.green)
plot(stopeR4, title="stopeR4", color=color.red)
plot(profitR5, title="profitR5", color=color.blue)

plot(S2, title="S2", color=color.green)
plot(S3, title="S3", color=color.red)
plot(stopeS4, title="stopeS4", color=color.red)
plot(profitS5, title="profitS5", color=color.blue)
plot(SMA1, title="sma1", color = color.yellow)
plot(SMA14, title = "sma14", color = color.white)
