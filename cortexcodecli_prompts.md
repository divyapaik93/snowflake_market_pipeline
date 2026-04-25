# Cortex Code Analytics
## Problem Statement for Analytics
<img width="552" height="723" alt="Screenshot 2026-04-25 at 7 14 55 AM" src="https://github.com/user-attachments/assets/7feed007-bc57-4ded-accf-a19623387355" />


## Cortex Code Analytics — Prompts & Results
### 1. Problem: Calculation of Stock risk, Short vs long-term price trend and checking if the trend is Bullish or Bearish.
#### Prompt: “On MARKET_DATA.PUBLIC.STOCK_DAILY, write a query that computes 20-day rolling annualised volatility, SMA-10, SMA-20, and a bullish/bearish momentum signal per ticker.”
#### Cortex output:
<img width="512" height="128" alt="unnamed" src="https://github.com/user-attachments/assets/eb106826-2e67-4082-a56b-7a014145cee8" />

### 2. Problem: Justification of risk taken
#### Prompt 2: "On MARKET_DATA.PUBLIC.STOCK_DAILY, compute the Sharpe ratio and max drawdown per ticker across the full date range"
#### Cortex output:
<img width="512" height="202" alt="unnamed-2" src="https://github.com/user-attachments/assets/bdb20abe-5355-486c-89c1-2a4caa30098c" />

### 3. Problem: Computation of fair price and profitable trade.
#### Prompt 3: "On MARKET_DATA.PUBLIC.TICK_DATA, compute average spread in basis pointsand true VWAP (price × size weighted) per ticker per day"
#### Cortex output:
<img width="512" height="226" alt="unnamed-3" src="https://github.com/user-attachments/assets/5f450427-93e9-4952-b206-71ec731ce5b2" />

### 4. Problem: Riskiest tickers and the types of risk.
#### Prompt 4: "On MARKET_DATA.PUBLIC.ALERTS, show alert count, average value, and maxvalue grouped by ticker, type, and severity"
#### Cortex output:
<img width="512" height="284" alt="unnamed-4" src="https://github.com/user-attachments/assets/90bcce2a-97f7-4dd0-975b-3744b4e10aac" />
<img width="512" height="123" alt="unnamed-5" src="https://github.com/user-attachments/assets/e0e9367e-6a7d-40e4-a1e5-b3979e819b35" />

### 5. Problem: Pipeline health check.
#### Prompt 5: "On MARKET_DATA.PUBLIC.INGESTION_LOG, show rows per second throughput and rejection rate per batch run"
#### Cortex output:
<img width="512" height="112" alt="unnamed-6" src="https://github.com/user-attachments/assets/8ace1319-90c4-4aa7-b0eb-5c2e0f44bfde" />

