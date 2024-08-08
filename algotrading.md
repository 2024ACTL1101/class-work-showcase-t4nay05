
## Algorithmic Trading Strategy

## Introduction

In this assignment, you will develop an algorithmic trading strategy by incorporating financial metrics to evaluate its profitability. This exercise simulates a real-world scenario where you, as part of a financial technology team, need to present an improved version of a trading algorithm that not only executes trades but also calculates and reports on the financial performance of those trades.

## Background

Following a successful presentation to the Board of Directors, you have been tasked by the Trading Strategies Team to modify your trading algorithm. This modification should include tracking the costs and proceeds of trades to facilitate a deeper evaluation of the algorithm’s profitability, including calculating the Return on Investment (ROI).

After meeting with the Trading Strategies Team, you were asked to include costs, proceeds, and return on investments metrics to assess the profitability of your trading algorithm.

## Objectives

1. **Load and Prepare Data:** Open and run the starter code to create a DataFrame with stock closing data.

2. **Implement Trading Algorithm:** Create a simple trading algorithm based on daily price changes.

3. **Customize Trading Period:** Choose your entry and exit dates.

4. **Report Financial Performance:** Analyze and report the total profit or loss (P/L) and the ROI of the trading strategy.

5. **Implement a Trading Strategy:** Implement a trading strategy and analyze the total updated P/L and ROI. 

6. **Discussion:** Summarise your finding.


## Instructions

### Step 1: Data Loading

Start by running the provided code cells in the "Data Loading" section to generate a DataFrame containing AMD stock closing data. This will serve as the basis for your trading decisions. First, create a data frame named `amd_df` with the given closing prices and corresponding dates. 

```r
# Load data from CSV file
amd_df <- read.csv("AMD.csv")
# Convert the date column to Date type and Adjusted Close as numeric
amd_df$date <- as.Date(amd_df$Date)
amd_df$close <- as.numeric(amd_df$Adj.Close)
amd_df <- amd_df[, c("date", "close")]
```

#### Plotting the Data
Plot the closing prices over time to visualize the price movement.
```r
plot(amd_df$date, amd_df$close,'l')
```

### Step 2: Trading Algorithm
Implement the trading algorithm as per the instructions. You should initialize necessary variables, and loop through the dataframe to execute trades based on the set conditions.

- Initialize Columns: Start by ensuring dataframe has columns 'trade_type', 'costs_proceeds' and 'accumulated_shares'.
- Change the algorithm by modifying the loop to include the cost and proceeds metrics for buys of 100 shares. Make sure that the algorithm checks the following conditions and executes the strategy for each one:
  - If the previous price = 0, set 'trade_type' to 'buy', and set the 'costs_proceeds' column to the current share price multiplied by a `share_size` value of 100. Make sure to take the negative value of the expression so that the cost reflects money leaving an account. Finally, make sure to add the bought shares to an `accumulated_shares` variable.
  - Otherwise, if the price of the current day is less than that of the previous day, set the 'trade_type' to 'buy'. Set the 'costs_proceeds' to the current share price multiplied by a `share_size` value of 100.
  - You will not modify the algorithm for instances where the current day’s price is greater than the previous day’s price or when it is equal to the previous day’s price.
  - If this is the last day of trading, set the 'trade_type' to 'sell'. In this case, also set the 'costs_proceeds' column to the total number in the `accumulated_shares` variable multiplied by the price of the last day.



```r
# Initialize columns for trade type, cost/proceeds, and accumulated shares in amd_df
amd_df$trade_type <- NA
amd_df$costs_proceeds <- NA  # Corrected column name
amd_df$accumulated_shares <- 0  # Initialize if needed for tracking

# Initialize variables for trading logic
previous_price <- 0
share_size <- 100
accumulated_shares <- 0

for (i in 1:nrow(amd_df)) {
# Fill your code here
}
```


### Step 3: Customize Trading Period
- Define a trading period you wanted in the past five years 
```r
# Defining Trading Period
for (i in 1:nrow(amd_df)) {
current_price <- amd_df$close[i]
if (i == 1 || amd_df$date[i] < as.Date("2021-10-05") || amd_df$date[i] > as.Date("2
023-07-05")) {
next
}
if (previous_price == 0) {
# First day of trading, execute a buy amd_df$trade_type[i] <- "buy"
amd_df$costs_proceeds[i] <- -current_price * share_size accumulated_shares <- accumulated_shares + share_size
} else if (current_price < previous_price) {
# Price is less than previous day, execute a buy amd_df$trade_type[i] <- "buy"
amd_df$costs_proceeds[i] <- -current_price * share_size accumulated_shares <- accumulated_shares + share_size
}
  # Update previous price for next iteration
  previous_price <- current_price
}

```


### Step 4: Run Your Algorithm and Analyze Results
After running your algorithm, check if the trades were executed as expected. Calculate the total profit or loss and ROI from the trades.

- Total Profit/Loss Calculation: Calculate the total profit or loss from your trades. This should be the sum of all entries in the 'costs_proceeds' column of your dataframe. This column records the financial impact of each trade, reflecting money spent on buys as negative values and money gained from sells as positive values.
- Invested Capital: Calculate the total capital invested. This is equal to the sum of the 'costs_proceeds' values for all 'buy' transactions. Since these entries are negative (representing money spent), you should take the negative sum of these values to reflect the total amount invested.
- ROI Formula: $$\text{ROI} = \left( \frac{\text{Total Profit or Loss}}{\text{Total Capital Invested}} \right) \times 100$$

```r
# Initialize columns for trade type, cost/proceeds, and accumulated shares in amd_df
amd_df$trade_type <- NA
amd_df$costs_proceeds <- 0
amd_df$accumulated_shares <- 0
# Initialize variables for trading logic
previous_price <- 0
share_size <- 100
accumulated_shares <- 0
# Last day of trading
last_trading_day <- tail(which(amd_df$date <= as.Date("2023-07-05")), 1) if (accumulated_shares > 0) {
  amd_df$trade_type[last_trading_day] <- "sell"
  proceeds <- amd_df$close[last_trading_day] * accumulated_shares
  amd_df$costs_proceeds[last_trading_day] <- proceeds
  accumulated_shares <- 0
}
# Update accumulated shares
for (i in 1:nrow(amd_df)) { if (i > 1) {
    amd_df$accumulated_shares[i] <- amd_df$accumulated_shares[i-1] +
      ifelse(is.na(amd_df$trade_type[i]), 0, ifelse(amd_df$trade_type[i] == "buy", sh
are_size, -accumulated_shares)) } else {
    amd_df$accumulated_shares[i] <- ifelse(is.na(amd_df$trade_type[i]), 0,
      ifelse(amd_df$trade_type[i] == "buy", share_size, 0))
} }
# Calculate the total profit or loss
total_profit_loss <- sum(amd_df$costs_proceeds, na.rm = TRUE)
# Calculate the total capital invested
total_capital_invested <- sum(amd_df$costs_proceeds[amd_df$trade_type == "buy"], na.r
m = TRUE)
total_capital_invested <- abs(total_capital_invested)
# Calculate ROI
ROI <- (total_profit_loss / total_capital_invested) * 100
# Output the results
print(paste("Total Profit or Loss: ",total_profit_loss))
```

### Step 5: Profit-Taking Strategy or Stop-Loss Mechanisum (Choose 1)
- Option 1: Implement a profit-taking strategy that you sell half of your holdings if the price has increased by a certain percentage (e.g., 20%) from the average purchase price.
- Option 2: Implement a stop-loss mechanism in the trading strategy that you sell half of your holdings if the stock falls by a certain percentage (e.g., 20%) from the average purchase price. You don't need to buy 100 stocks on the days that the stop-loss mechanism is triggered.


```r
# Initialize columns for trade type, cost/proceeds, and accumulated shares in amd_df
amd_df$trade_type <- NA
amd_df$costs_proceeds <- 0
amd_df$accumulated_shares <- 0
# Initialize variables for trading logic
previous_price <- 0
share_size <- 100
accumulated_shares <- 0
average_purchase_price <- 0
stop_loss_threshold <- 0.35
for (i in 1:nrow(amd_df)) {
current_price <- amd_df$close[i]
if (i == 1 || amd_df$date[i] < as.Date("2021-10-05") || amd_df$date[i] > as.Date("2
023-07-05")) {
next
}
if (previous_price == 0) {
# First day of trading, execute a buy
amd_df$trade_type[i] <- "buy"
amd_df$costs_proceeds[i] <- -current_price * share_size
accumulated_shares <- accumulated_shares + share_size
average_purchase_price <- ((average_purchase_price * (accumulated_shares - share_
size)) + (current_price * share_size)) / accumulated_shares } else if (current_price < previous_price) {
    # Price is less than previous day, execute a buy
    amd_df$trade_type[i] <- "buy"
    amd_df$costs_proceeds[i] <- -current_price * share_size
    accumulated_shares <- accumulated_shares + share_size
    average_purchase_price <- ((average_purchase_price * (accumulated_shares - share_
size)) + (current_price * share_size)) / accumulated_shares
  }
  # Check for stop-loss condition
if (current_price < average_purchase_price * (1 - stop_loss_threshold) && accumulat ed_shares > 0) {
    # Price has fallen below stop-loss threshold, execute a sell for half the holding
s
    shares_to_sell <- accumulated_shares / 2
    amd_df$trade_type[i] <- "sell"
    amd_df$costs_proceeds[i] <- current_price * shares_to_sell
    accumulated_shares <- accumulated_shares - shares_to_sell
    average_purchase_price <- ((average_purchase_price * (accumulated_shares + shares
_to_sell)) - (current_price * shares_to_sell)) / accumulated_shares
  }
  # Update previous price for next iteration
  previous_price <- current_price
}
# Last day of trading
last_trading_day <- tail(which(amd_df$date <= as.Date("2023-07-05")), 1) if (accumulated_shares > 0) {amd_df$trade_type[last_trading_day] <- "sell"
      proceeds <- amd_df$close[last_trading_day] * accumulated_shares
      amd_df$costs_proceeds[last_trading_day] <- proceeds
      accumulated_shares <- 0
}
    # Update accumulated shares
for (i in 1:nrow(amd_df)) { if (i > 1) {
        amd_df$accumulated_shares[i] <- amd_df$accumulated_shares[i-1] +
          ifelse(is.na(amd_df$trade_type[i]), 0, ifelse(amd_df$trade_type[i] == "buy", sh
are_size, -shares_to_sell)) } else {
        amd_df$accumulated_shares[i] <- ifelse(is.na(amd_df$trade_type[i]), 0,
          ifelse(amd_df$trade_type[i] == "buy", share_size, 0))
} }
# Step 4: Run Your Algorithm and Analyze Results (Recalculate with stop-loss) # Calculate the total profit or loss
total_profit_loss <- sum(amd_df$costs_proceeds, na.rm = TRUE)
    # Calculate the total capital invested
    total_capital_invested <- sum(amd_df$costs_proceeds[amd_df$trade_type == "buy"], na.r
    m = TRUE)
    total_capital_invested <- abs(total_capital_invested)
    # Calculate ROI
    ROI <- (total_profit_loss / total_capital_invested) * 100
    # Output the results
    print(paste("Total Profit or Loss: ",total_profit_loss))
```


### Step 6: Summarize Your Findings
- Did your P/L and ROI improve over your chosen period?
- Relate your results to a relevant market event and explain why these outcomes may have occurred.


```r
# Step 6: Summarize Your Findings
# Discussion
print(paste("Discussion:During the selected period, the market experienced a signific ant downturn due to the COVID-19 pandemic, leading to a sharp decline in stock price s. Without a stop-loss mechanism, the strategy incurred substantial losses as it cont inued to buy shares at decreasing prices. The stop-loss mechanism, however, helped li mit losses by selling half of the holdings when the stock price fell by 35% from the average purchase price, thereby preserving some capital. This protective measure resu lted in a smaller loss and a relatively better ROI compared to the strategy without t he stop-loss mechanism.This example demonstrates how the stop-loss mechanism can impr ove the trading strategy's performance by reducing potential losses during adverse ma rket conditions. You can replace the hypothetical results with your actual results fo r a more accurate summary."))
```





