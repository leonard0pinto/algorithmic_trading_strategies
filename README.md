# Mean-Reversion and ARIMAX Algorithmic Trading Strategies on SPTL

This project evaluates two leveraged algorithmic trading strategies on the SPDR Portfolio Long Term Treasury ETF (SPTL), an ETF providing exposure to long-duration U.S. Treasury bonds. The aim is to compare a rule-based mean-reversion strategy with a forecast-driven ARIMAX strategy under a consistent backtesting framework.

The analysis uses SPTL daily price data, the Effective Federal Funds Rate (EFFR) as a proxy for the risk-free rate, and additional fixed-income/macroeconomic variables from Federal Reserve Economic Data (FRED). The sample covers January 2023 to December 2023, a period characterised by elevated interest rates and Federal Reserve policy tightening.

The full dataset is split into training, validation, and test sets. Model and parameter selection are performed using the training and validation sets, while final performance is evaluated on an unseen out-of-sample test set.

The portfolio starts with initial capital of $100,000 and uses a maximum leverage constraint of 10x. Unused capital is assumed to earn the daily risk-free rate.

## Strategies Implemented

### 1. Mean-Reversion Strategy

The mean-reversion strategy is a rule-based trading strategy. It compares the current SPTL return to a rolling moving average of past returns. If the current return is below its recent average, the strategy takes a long position, based on the idea that returns may revert upward. If the current return is above its recent average, the strategy takes a short position, based on the idea that returns may revert downward.

In the final implementation, the signal is scaled by the size of the deviation between the moving average and the current return, and clipped between -1 and 1. The optimal lookback window is selected using validation-set Sharpe Ratio.

### 2. ARIMAX Forecasting Strategy

The ARIMAX strategy is a forecast-based trading strategy. Instead of relying only on a moving-average rule, it models stationary SPTL daily returns using an ARIMAX model with exogenous fixed-income variables.

The model generates a one-step-ahead daily return forecast. This forecast is converted into a next-day price prediction. The strategy goes long when the predicted next-day price is above the current price, and short when the predicted next-day price is below the current price.

The final ARIMAX model uses selected exogenous variables including Treasury yield data, yield-curve spread information, and term-premium data.

## Performance Evaluation

Both strategies are evaluated using:

- Annualised Sharpe Ratio
- Maximum Drawdown
- Calmar Ratio
- Total Return
- Daily and cumulative PnL

The mean-reversion strategy produces more controlled downside behaviour due to its continuous, non-binary signal. The ARIMAX strategy achieves higher total gross return but exhibits larger drawdowns, reflecting the higher risk of a more aggressive forecast-driven strategy.

## API Key Requirement

This project uses data from the Federal Reserve Economic Data (FRED) API. To run the notebooks, users need to create their own FRED API key and store it as an environment variable.

```bash
export FRED_API_KEY="your_fred_api_key_here"

