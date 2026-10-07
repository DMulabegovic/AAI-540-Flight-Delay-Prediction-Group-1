# AAI-540 Flight Delay Prediction System

## Project Overview

This project builds an end-to-end machine learning system in AWS to predict whether a U.S. domestic flight will arrive at least 15 minutes late using only information available before departure.

The system uses 2025 Bureau of Transportation Statistics (BTS) Reporting Carrier On-Time Performance data and implements the machine learning lifecycle in Amazon SageMaker, including data ingestion, preprocessing, exploratory analysis, feature engineering, model training, batch inference, monitoring, pipeline automation, and model registration.

## Team Members

- Thomas Geraci
- Daniel Sims
- Denis Adi Mulabegovic

## Business Objective

Flight delays create operational challenges for airlines, airports, and travelers. The goal of this project is to identify flights that are at risk of arriving 15 minutes or more late before departure.

Because delayed flights represent the minority class, model evaluation focuses on metrics such as delayed-flight recall and F1 score rather than overall accuracy alone.

## Dataset

**Source:** Bureau of Transportation Statistics (BTS)  
**Dataset:** Reporting Carrier On-Time Performance  
**Coverage:** January 1, 2025 through December 31, 2025  
**Raw records:** 7,001,619

The raw monthly source files are preserved in Amazon S3 as compressed `.csv.gz` files.

## AWS Architecture

The project follows this end-to-end workflow:

1. **BTS Flight Data** — 2025 raw source data
2. **Amazon S3** — storage for raw monthly files
3. **Athena + Processing** — validation, cleaning, and querying
4. **SageMaker Feature Store** — managed storage for engineered pre-departure features
5. **SageMaker XGBoost Training** — binary classification model development
6. **Batch Transform** — managed batch inference
7. **Evaluation + Quality Gate** — model performance validation before registration
8. **Monitoring + CloudWatch** — model quality, data quality, drift, and operational monitoring
9. **SageMaker Pipelines** — automated machine learning workflow
10. **CI/CD Automation** — repeatable training, evaluation, inference, and registration
11. **Model Registry** — versioned model lifecycle and approval control

## Model

The project uses **SageMaker XGBoost** for binary classification.

**Target:**
- `0` — Not delayed
- `1` — Arrival delay of 15 minutes or more

The model uses only pre-departure information to avoid target leakage.

Key model features include:

- Month
- Day of month
- Day of week
- Weekend indicator
- Scheduled departure hour
- Scheduled arrival hour
- Scheduled flight duration
- Flight distance

## Evaluation

The dataset is imbalanced, with approximately:

- **77.7% on-time flights**
- **22.3% delayed flights**

Because of this imbalance, model evaluation emphasizes:

- Delayed-flight recall
- Delayed-flight F1 score
- Precision
- Balanced accuracy
- ROC-AUC
- PR-AUC
- Confusion matrix

The held-out test evaluation demonstrated that the XGBoost model substantially improves delayed-flight detection compared with a majority-class benchmark.

## Batch Inference

SageMaker Batch Transform is used to generate predictions on held-out flight records without maintaining a continuously running real-time endpoint.

The batch inference stage supports repeatable evaluation and integrates with the automated SageMaker workflow.

## Monitoring

The project includes monitoring for:

- Model quality
- Data quality
- Data drift
- Operational metrics
- CloudWatch alarms and dashboards

Monitoring outputs support continued evaluation of model behavior after training.

## Pipeline and Model Registry

SageMaker Pipelines automates the machine learning workflow, including training, evaluation, batch inference, quality checks, and model registration.

Qualified models are registered in the **FlightDelayPrediction** Model Registry package group, where model versions can be reviewed and controlled through an approval process.

## Repository Structure

| File | Purpose |
|---|---|
| `Data_Ingestion.ipynb` | Raw data ingestion and validation |
| `PreProcessing.ipynb` | Cleaning and preprocessing |
| `EDA.ipynb` | Exploratory data analysis |
| `Feature_Engineering_Feature_Store.ipynb` | Feature engineering and Feature Store |
| `Model_Development.ipynb` | XGBoost training and evaluation |
| `Model_Monitoring.ipynb` | Model quality, data quality, drift, and CloudWatch monitoring |
| `Pipeline_Model_Registry.ipynb` | SageMaker Pipelines and Model Registry |
| `README.md` | Project documentation |

## Reproducibility

The repository separates each stage of the machine learning lifecycle into dedicated notebooks. GitHub provides version control and team contribution history, while SageMaker Pipelines provides automated and repeatable execution of the model lifecycle.

## Technologies Used

- Python
- Pandas
- Amazon S3
- Amazon Athena
- AWS Wrangler
- Amazon SageMaker
- SageMaker Feature Store
- SageMaker XGBoost
- SageMaker Batch Transform
- SageMaker Pipelines
- SageMaker Model Registry
- Amazon CloudWatch
- GitHub

## Course

**AAI-540 — Machine Learning Operations**  
University of San Diego
