**Section 1: Project Overview**

We will build a Real-Time Weather Anomaly Detection Pipeline designed to monitor critical infrastructure risks. Extreme weather events (like sudden heatwaves or flash freezes) pose significant threats to energy grids, transportation networks, and agriculture. Traditional weather reporting is often static or delayed. Our system solves this by training a machine learning model on one year of historical hourly temperature data to understand seasonal "normals," while simultaneously streaming live temperature data every 15 minutes from an API. This allows for instant detection of statistical anomalies as they happen.


**Section 2: Data Sources**

Batch Source (History): We will programmatically extract 1 year of historical hourly temperature data for New York City (Latitude: 40.71, Longitude: -74.01) from the Open-Meteo Archive API.

Role: Provides the baseline "normal" behavior for model training.

Streaming Source (Live): We will use the Open-Meteo Forecast API to fetch the current temperature every 15 minutes via a Cloud Function.

Role: Provides the real-time feed to be scored against the model.


**Section 3: Architecture Diagram**

Batch Layer: Open-Meteo API → Python Extract (Colab) → Google Cloud Storage (Raw CSV) → BigQuery (weather_history table).

Streaming Layer: Cloud Scheduler (15 min) → Cloud Function (Producer) → Pub/Sub (weather-live-topic) → Dataflow → BigQuery (weather_streaming table).

Analytics Layer: BigQuery Views (Unified) → BigQuery ML (ARIMA_PLUS) → Looker Studio Dashboard.


**Section 4: Modeling Strategy**

Model Type: Time Series Forecasting (ARIMA_PLUS in BigQuery ML).

Why: ARIMA_PLUS is ideal for this use case because weather data is highly seasonal (daily and yearly cycles) and requires automatic handling of time-series decomposition (trends vs. seasonal spikes).

Target & Features:

Target: temperature (Air temperature in Celsius).

Features: timestamp (Standardized UTC timestamp from both historical and live sources).

Anomaly Threshold: We utilize a probability threshold of 0.8 to balance sensitivity (catching real shifts) with noise reduction.



**Section 5: KPI Visuals (Executive Dashboard)**

Current Temp (Scorecard): Displays the most recent temperature reading from the live stream to prove real-time connectivity.

24h High/Low (Scorecards): Tracks the daily range to provide immediate context to the current reading.

Anomalies Detected (Scorecard): A red-flag counter showing the number of data points in the last 24 hours that statistically deviated from the expected model.

The "Live Pulse" (Time-Series Chart): A line chart visualizing the temperature over time, with the streaming data overlaid on the historical context.

Anomaly Detail Table: A table listing specific timestamps, actual temperatures, and the model's expected lower_bound / upper_bound, with conditional formatting highlighting anomalies in red.
