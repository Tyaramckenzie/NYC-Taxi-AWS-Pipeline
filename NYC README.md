# NYC Taxi AWS Data Engineering Pipeline

An end-to-end cloud data engineering pipeline that ingests NYC Yellow Taxi trip data into Amazon S3, transforms the data using AWS Glue, orchestrates the workflow with AWS Step Functions, analyzes the data with Amazon Athena, and connects the resulting cloud data to Power BI for business intelligence and visualization.

## Project Overview

This project demonstrates the design and implementation of a serverless AWS data pipeline using an event-driven architecture.

Raw NYC Yellow Taxi Parquet files are uploaded to an Amazon S3 landing zone. An S3 event triggers an AWS Lambda function, which initiates the downstream workflow through AWS Step Functions. AWS Glue transforms the raw data into analytics-ready Parquet data in a Silver layer. Amazon Athena then queries the transformed data, with results written to a Gold analytics location. Power BI connects to Athena through the Amazon Athena ODBC driver for interactive reporting and visualization.

The pipeline also includes SNS notifications and error handling to provide visibility into successful and failed executions.

---

## Architecture


                     NYC Taxi Parquet Data
                              │
                              ▼
                    ┌──────────────────┐
                    │    Amazon S3     │
                    │   Landing Zone   │
                    └────────┬─────────┘
                             │
                       S3 Event Trigger
                             │
                             ▼
                    ┌──────────────────┐
                    │   AWS Lambda     │
                    │ Event Processing │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ AWS Step         │
                    │ Functions        │
                    │ Orchestration    │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │    AWS Glue      │
                    │   ETL / Spark    │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │    Amazon S3     │
                    │   Silver Layer   │
                    │    Parquet       │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │  Amazon Athena    │
                    │  SQL Analytics    │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │    Amazon S3     │
                    │    Gold Layer    │
                    │ Query Results     │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │    Power BI      │
                    │   Dashboard      │
                    └──────────────────┘

                 ┌─────────────────────────┐
                 │     Amazon SNS          │
                 │ Success / Failure       │
                 │ Notifications           │
                 └─────────────────────────┘