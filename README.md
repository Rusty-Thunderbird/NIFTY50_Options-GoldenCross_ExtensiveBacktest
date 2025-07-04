# NIFTY 50 Options Backtesting & Analytics Framework

This repository provides a **robust, research-grade backtesting framework** for systematic options trading strategies on the **NIFTY 50 index**. It supports both **buy-side (long options)** and **sell-side (credit spreads)** strategies, with features like **adaptive volatility regime filtering**, **realistic execution modeling**, and **comprehensive analytics**.

---

## Features

### Chunked Backtesting
- Efficiently processes large datasets in time-based chunks.
- Reduces memory consumption and runtime delays.

---

### Buy-Side Strategy (Long Options)
- **Signal Source**: 15-minute directional signals (Call/Put).
- **Entry**: Long ATM options (Calls and Puts).
- **Exits**:
  - ATR-based trailing stop-loss.
  - Partial take-profits.
  - Dynamic take-profits based on price action.
- **Execution Logic**:
  - Adaptive regime filters using VIX/IVR.
  - Liquidity-aware modeling.
  - Includes slippage, brokerage, partial fills, and anomalies.

---

### Sell-Side Strategy (Credit Spreads)
- **Strategy Types**: Bull Put Spread & Bear Call Spread.
- **Entry Conditions**:
  - Trend filters.
  - Volatility regime confirmation.
  - Price breakout confirmation.
- **Execution & Risk**:
  - Dynamic risk-based sizing.
  - Margin-aware execution and constraints.

---

## Reporting & Visualization

- Sharpe, Sortino, Calmar Ratios
- CAGR, Max Drawdown
- Win/Loss statistics, Avg PnL
- Equity Curves, Drawdown Charts
- Trade-level PnL distributions
- PnL Heatmaps by Month & Trade Side
- Monte Carlo Simulation for risk modeling

---

## Data Requirements

| Type          | Description                                                         |
|---------------|---------------------------------------------------------------------|
| Spot Data     | Minute-level OHLC for NIFTY 50 index                                |
| Options Data  | Minute-level options chain with strike, type, expiry, OI, volume, IV|
| VIX Data      | Minute-level or resampled India VIX data                            |
| Signals       | CSV with 15-min interval signals (`timestamp`, `direction`)         |

> ⚠ All paths must be configured as per your local environment.

---

## Quick Start

### 1. Clone Repository & Install Dependencies

```bash
git clone <repo-url>
cd <repo-directory>
pip install -r requirements.txt
python main_backtest.py

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```
# Load trade logs
```
buy_log_df = pd.read_csv("D:/Bits Hyderabad_Campus Diaries/PS-1/AlgoBulls/buy_trade_log.csv", parse_dates=['exit_date'])
sell_log_df = pd.read_csv("D:/Bits Hyderabad_Campus Diaries/PS-1/AlgoBulls/sell_trade_log.csv", parse_dates=['exit_date'])
```
# Label trades
```
buy_log_df['side'] = 'Buy'
sell_log_df['side'] = 'Sell'
```
# Combine logs
```
combined_df = pd.concat([buy_log_df, sell_log_df], ignore_index=True)
combined_df['month'] = pd.to_datetime(combined_df['exit_date']).dt.to_period('M')
```
# Create pivot table
```
heatmap_data = combined_df.pivot_table(
    index='month', columns='side', values='exit_pnl', aggfunc='sum'
).fillna(0)
```
# Plot heatmap
```
plt.figure(figsize=(12, 7))
sns.heatmap(
    heatmap_data, annot=True, fmt='.0f', cmap='RdYlGn',
    linewidths=0.5, linecolor='gray', annot_kws={"size": 12}
)
plt.title('Cumulative PnL Heatmap by Month and Side', fontsize=18)
plt.xlabel('Side', fontsize=14)
plt.ylabel('Month', fontsize=14)
plt.tight_layout()
plt.show()
```
## Configuration


Strategy logic, execution controls, and risk parameters are configurable at the top of the main script.

Update file paths and filters as needed to match your local data and environment.

---

## Performance Metrics


- Sharpe Ratio  
- Sortino Ratio  
- Calmar Ratio  
- Max Drawdown  
- CAGR (Compounded Annual Growth Rate)  
- Best and Worst Trade  
- Win/Loss Percentage  
- Monte Carlo simulation for drawdown and return modeling

## Combined Strategy Performance Report

- **Initial Capital**: ₹1,000,000.00  
- **Final Portfolio**: ₹3,700,761.03  
- **Total Return**: 270.08%  
- **Max Drawdown**: ₹124,049.77  

- **Sharpe Ratio**: 3.11  
- **Sortino Ratio**: 6.62  
- **Calmar Ratio**: 10.01  

- **Total Trades**: 976  
- **Winning Trades**: 673  
- **Losing Trades**: 303  
- **Average PnL**: ₹2,767.17  
- **Best Trade**: ₹146,082.03  
- **Worst Trade**: -₹18,000.00  

- **Trade Period**: 2023-06-02 to 2025-01-15



---

## Intended Audience

- Quantitative and systematic traders  
- Financial engineering researchers  
- Students and developers exploring Indian options strategies

---

## Notes

- Modular code structure makes it easy to extend or customize strategies.  
- Realistic assumptions for execution, including slippage, brokerage, and fill modeling.  
- Chunked processing is recommended for working with large datasets efficiently.

---

## License

This project is released under the MIT License.

---

## Support

For questions, issues, or contributions, please open an issue or submit a pull request.
