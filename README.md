# snowflake_market_pipeline
## 1. Problem Statement

### Statement
Financial data teams work with two fundamentally different data streams — historical price data and live market ticks. This project aims at building an end-to-end market data platform that handles batch and real-time ingestion, automated anomaly alerting, and AI-assisted analytics entirely within Snowflake.

### Why This Problem
I wanted to explore how both batch and real-time data pipelines behave differently in a cloud data warehouse environment, specifically how Snowflake handles high-frequency tick data versus structured historical OHLCV data ingested in bulk.

Market data was a natural fit because it combines both historical prices for trend analysis and live ticks for real-time monitoring, in one domain, making it an ideal dataset for comparing ingestion patterns, latency, and data freshness.

Beyond the pipeline itself, I wanted to understand how Snowflake's Cortex Code CLI could reduce the gap between raw data and actionable insights, replacing manual SQL writing with natural language prompts to generate analytics and a live dashboard without switching tools.

### How the System Works
Raw market data enters the pipeline through two Python scripts - a batch ingestion script that processes historical OHLCV data from a CSV file every 30 minutes, and a real-time tick simulator that streams live price data every 5 minutes. Both pipelines validate, enrich, and load data into Snowflake (MARKET_DATA.PUBLIC), where it is stored across five tables covering daily prices, raw ticks, aggregated OHLCV windows, alerts, and pipeline metadata.

A scheduler orchestrates both pipelines continuously, running them in parallel on a fixed cadence. As ticks stream in, the real-time pipeline computes rolling 10-second micro-OHLCV windows and fires rule-based alerts for price jumps, wide spreads, and order book imbalances.

Cortex Code CLI sits on top of the warehouse, translating natural language prompts into analytical SQL, surfacing risk metrics like rolling volatility, Sharpe ratio, max drawdown, true VWAP, and spread analysis across all tickers.

Finally, a Streamlit dashboard consumes the live Snowflake data, presenting tick price charts, candlestick charts with configurable time intervals, an order book imbalance panel, and a real-time alerts table, with a pre-written prompt selector for guided, no-code data exploration.

## 2. Pipeline

### a. Data Generation 
Source system(s) and how raw data enters the pipeline.
• Source: Python script
• Ingestion method: Batch and Streaming
• Frequency:
- Batch - every 30 minutes
- Streaming - every 5 minutes (each run lasts 5 minutes)
• Raw landing zone: Snowflake internal stage

### b. Cloud Data Warehouse (Snowflake)
How data is stored, modeled, and transformed inside Snowflake.
• Schema structure: RAW -> STAGING -> ANALYTICS
- RAW: Ticks and daily OHLCV land directly into TICK_DATA and STOCK_DAILY.
- STAGING: STOCK_DAILY_STAGING temp table used during MERGE for idempotent batch loads.
- ANALYTICS: MICRO_OHLCV and ALERTS as derived/aggregated outputs from the realtime pipeline.
• Data model: Flat operational schema — STOCK_DAILY as the daily fact table, TICK_DATA as the tick-level fact table, ALERTS and MICRO_OHLCV as derived tables, INGESTION_LOG as a pipeline metadata table.
• Transformations:
- Batch: validation, enrichment (daily return %, intraday range, VWAP proxy), idempotent MERGE.
- Realtime: 10-second rolling window aggregation into MICRO_OHLCV, rule-based alert evaluation into ALERTS.
• Key tables:
- STOCK_DAILY
- TICK_DATA
- MICRO_OHCLV
- ALERTS
- INGESTION_LOG

### c. Dashboard
How the data is consumed and visualized.
• BI tool: Streamlit
• Key metrics: - Live tick price per ticker (line chart)
- Micro-OHLCV candlestick chart with 5 time interval aggregations (1 Min, 15 Min, 1 Hour, 4 Hours, Daily)
- Real-time alerts filtered by severity (INFO, WARNING, CRITICAL)
- Live tick price per ticker (Line chart, filtered by the company)
- Live tick price per ticker (Line chart, all companies)
• Audience: Trading desk analysts and risk managers monitoring live market activity and anomalies.
• Refresh cadence: Automatically reflects fresh data with each ingestion cycle - realtime tables update every 5 minutes via the scheduler, historical data refreshes every 30 minutes.

## 3. Tools Used

### a. Claude (Anthropic)
Used for AI-assisted development throughout the project.
• Generated Python scripts for batch and realtime data ingestion.
• Built the pipeline scheduler for orchestrating both ingestion workflows.

### b. Cortex CLI
Used for AI-assisted SQL generation, analytics, and Streamlit dashboard development directly from the terminal.
• Purpose: Generating quant-grade analytical SQL queries and a Streamlit dashboard on top of Snowflake market data tables using natural language prompts, without writing code manually.
• Integration: Invoked locally from the project directory, connected to Snowflake via RSA key authentication. Queries are executed directly against MARKET_DATA.PUBLIC tables in real time.
• Key commands: Check attached documentation for Cortex Code CLI prompts used in this project.
