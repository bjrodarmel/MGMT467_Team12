##  Executive Dashboard & Visualization

This project includes a live Looker Studio dashboard that visualizes the real-time data stream and model predictions.
(https://lookerstudio.google.com/reporting/9e46bd0f-7df6-4541-b897-5eaabcc860a7)

---

### Dashboard Overview
The dashboard is designed for "at-a-glance" monitoring of weather conditions and infrastructure risks. It updates automatically as new data arrives from the streaming pipeline.

### Visual Breakdown

The dashboard is organized into three logical sections based on the final design:

#### 1. Top-Level KPIs (Real-Time Status)
* **Current Temp (C):** Displays the single most recent temperature reading ingested by the system (e.g., 40.3°C). This proves end-to-end pipeline latency is low.
* **Avg Temperature:** Provides context on the baseline temperature across the monitored period (e.g., 13.28°C).
* **Lowest Temperature:** Tracks the minimum recorded temperature (e.g., -16.7°C) to highlight extreme cold events.
* **Current Anomaly Count:** A "Red Flag" counter that sums up detected anomalies. If this number is > 0, the system is actively alerting.

#### 2. Temperature Over Time (Time-Series Chart)
* **Visual:** A continuous line chart plotting `Temperature` vs. `Time` (Jan 2024 – Present).
* **Logic:** This visualizes the seasonal trend and daily fluctuations. It allows operators to visually verify if a current reading follows the expected historical pattern.
* **Data Source:** Unified View (combining History + Stream).

#### 3. Detailed Data Table (The Evidence)
* **Columns:** `Timestamp`, `temperature`, `is_anomaly`, `lower_bound`, `upper_bound`.
* **Purpose:** Provides the granular data backing the charts.
* **Validation:** Allows users to inspect specific timestamps to see the exact recorded temperature and whether the ML model flagged it (`is_anomaly`) based on the calculated confidence bounds.
