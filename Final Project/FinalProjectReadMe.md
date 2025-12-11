# Real-Time Weather Anomaly Detection Pipeline 

**MGMT 467 Term Project**

This project implements an end-to-end data engineering pipeline on Google Cloud Platform (GCP). It combines historical weather data (Batch) with live temperature streams (Real-Time) to detect anomalies using BigQuery ML (`ARIMA_PLUS`). The system is designed to identify sudden infrastructure risks (e.g., flash freezes, heatwaves) instantly.

##  Architecture

The pipeline follows a hybrid **Lambda Architecture**:

1.  **Batch Layer:** Ingests 1 year of historical data from Open-Meteo API $\rightarrow$ GCS $\rightarrow$ BigQuery.
2.  **Streaming Layer:** Fetches live data every 15 mins via Cloud Functions $\rightarrow$ Pub/Sub $\rightarrow$ Dataflow $\rightarrow$ BigQuery.
3.  **Analytics Layer:** BigQuery ML model trains on history and scores live data in real-time.
4.  **Presentation:** Looker Studio Dashboard for executive monitoring.



---

##  Prerequisites

* Google Cloud Platform Account
* `gcloud` CLI installed and authenticated
* Python 3.10+

## � Service Enablement

Run the following commands to enable the required APIs for the project:

```bash
gcloud services enable \
  bigquery.googleapis.com \
  storage.googleapis.com \
  pubsub.googleapis.com \
  dataflow.googleapis.com \
  cloudfunctions.googleapis.com \
  run.googleapis.com \
  cloudbuild.googleapis.com \
  artifactregistry.googleapis.com \
  cloudscheduler.googleapis.com
