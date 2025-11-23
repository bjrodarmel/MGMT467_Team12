**Section 1:**
We will build a Real-Time Cryptocurrency Anomaly Detection Pipeline to help algorithmic traders instantly identify market manipulation or 'flash crashes.' 
Crypto markets are highly volatile, and delayed data leads to missed arbitrage opportunities or significant portfolio losses. 
Currently, our traders rely on manual chart monitoring, which is too slow. 
Our system will solve this by training a model on years of historical price data to understand 'normal' volatility, while simultaneously streaming live price data from an API to flag anomalies the second they happen.


**Section 2:**
Batch Source: We will download the "Bitcoin Historical Data" Dataset from Kaggle (https://www.kaggle.com/datasets/mczielinski/bitcoin-historical-data)

Streaming Source: We will use the CoinGecko API since it is free and requires no key for basic use.

**Section 3:**
**Architecture Diagram**

<img width="721" height="231" alt="image" src="https://github.com/user-attachments/assets/2280750a-8491-4e68-84e8-d007b3b1e7c6" />


**Section 4:**

Model Type: Time Series Forecasting (ARIMA_PLUS)

Why: This model is built specifically for timestamped data and handles seasonality well.

Target & Features:

Target: daily_avg_sentiment (Average sentiment score aggregated by day)

Features: review_date (Timestamp from the historical and streaming data), total_review_volume (Count of reviews per day)


**Section 5:**

**KPI Visuals**

1. The Pulse Gauge (Real-Time Sentiment): Gauge Chart that shows average sentiment
2. The Futurecast (ML Prediction): Line chart with confidence internal comparing forecasted_sentiment_score vs. actual_sentiment_score
3. Red Flag Volume: Scorecard with comparision that compares sentiment_score to -0.8
4. Product Health Leaderboard: Table with heatmap columns listing Product IDs ranked by lowest average sentiment
