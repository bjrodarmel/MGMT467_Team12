# Team Contribution Report

**Project:** Real-Time Weather Anomaly Detection Pipeline
**Course:** MGMT-467
**Team Members:** Brandon Rodarmel, Ethan Stover, Sur Mehta

---

## 1. Task Distribution Overview

To ensure equitable contribution, we divided the project scope into three distinct technical domains corresponding to the Lambda Architecture layers: **Batch Infrastructure**, **Streaming Architecture**, and **Analytics & Visualization**.

| Team Member | Primary Responsibility |
| :--- | :--- |
| **Brandon** | End-to-End Architecture design, Cloud Functions, Pub/Sub, and Dataflow pipeline. |
| **Ethan** | Historical data ingestion, GCS storage management, Schema design, and Automation (Scheduler). |
| **Sur** | BigQuery ML modeling (`ARIMA_PLUS`), SQL Views, and Looker Studio Dashboard development. |

---

## 2. Individual Contributions

###  Brandon
**Focus:** Real-Time Ingestion (Phase 2 & 3) & System Design

* **Architecture Design:** Designed the complete GCP diagram, selecting Open-Meteo as the "Secret-Free" data source to ensure security compliance.
* **Serverless Ingestion:** Developed the `fetch-weather` Cloud Run Function (Gen 2) in Python. Implemented the critical `try/except` logic to handle API rate limits (HTTP 429) without crashing the pipeline.
* **Streaming Pipeline:** Configured the Pub/Sub topic (`weather-live-topic`) and deployed the Dataflow job using the "Pub/Sub to BigQuery" template.
* **Ops & Reliability:** Authored the "Failure Playbook" and the "Pipeline Validator" script used in the demo to verify real-time row arrival.

**Lessons Learned:**
* *Time-Step Alignment:* I learned that streaming data often arrives at irregular intervals (e.g., `12:00:04`), necessitating a "Snapping" strategy to align it with the rigid hourly grid required by the ARIMA model.
* *Cost Management:* Adjusting the Cloud Scheduler frequency from 1 minute to 15 minutes was a key operational decision that kept our architecture within the Google Cloud Free Tier limits.

---

###  Ethan
**Focus:** Batch Ingestion (Phase 1) & Storage Infrastructure

* **Historical Data Extraction:** Wrote the Python extraction script to pull 1 year of hourly temperature data from the Open-Meteo Archive API.
* **Storage & Schema:** Managed the Google Cloud Storage (GCS) layer, ensuring raw CSVs were archived before ingestion. Defined the strict BigQuery schema (`TIMESTAMP`, `FLOAT`) and implemented **Day-Partitioning** on the `weather_history` table to optimize query costs.
* **Data Quality:** Implemented the pre-ingestion validation logic in Python to check for and impute `NULL` temperature values.
* **Automation:** Configured the Cloud Scheduler cron job (`*/15 * * * *`) to drive the streaming process.

**Lessons Learned:**
* *Schema Rigidity:* I discovered that BigQuery is extremely strict about timestamp formats. The API returned ISO strings (`2024-01-01T12:00`) which caused load failures until I implemented a transformation step to append seconds (`:00`).
* *Infrastructure as Code:* Managing the dependencies between GCS buckets and BigQuery datasets highlighted the importance of deployment order (Storage first, then Database).

---

###  Sur
**Focus:** Modeling (Phase 4) & Visualization (Phase 5)

* **Machine Learning (BQML):** Trained the `ARIMA_PLUS` model on historical data. Built the secondary Linear Regression model to satisfy the `ML.EXPLAIN_PREDICT` project requirement.
* **Unified View Logic:** Wrote the complex SQL View (`weather_anomalies_view`) that unions the Batch and Streaming tables into a single timeline, enabling the model to score live data instantly.
* **Executive Dashboard:** Built the Looker Studio interface, including the "Live Pulse" chart and the KPIs. Implemented the **Conditional Formatting** rules that highlight anomalies in red.
* **Advanced Analytics:** Generated the Python/Plotly visuals (Histogram & Cluster Analysis) for the "Deep Dive" section of the presentation.

**Lessons Learned:**
* *Model Sensitivity:* Initial tests flagged too many false positives. I learned to tune the `anomaly_prob_threshold` (settling on 0.8) to balance sensitivity with noise reduction.
* *Visualization Limitations:* I found that Looker Studio has limitations with real-time refresh rates, requiring us to manage expectations by adding a "Last Updated" timestamp to the dashboard for transparency.

---

## 3. Pull Requests & Artifacts

* **Brandon:** `streaming/main.py` (Cloud Function), `docs/architecture.png`, `ops/failure_playbook.md`
* **Ethan:** `batch/extract_history.py`, `infra/gcs_setup.sh`, `infra/schema_def.json`
* **Sur:** `analytics/model_training.sql`, `analytics/create_view.sql`, `viz/plotly_analysis.ipynb`
