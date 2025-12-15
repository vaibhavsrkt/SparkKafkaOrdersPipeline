## Spark Kafka Streaming Pipeline (End-to-End Data Engineering Project)

This repository contains a production-style real-time data pipeline built using Apache Kafka and Apache Spark Structured Streaming.
The project simulates e-commerce order events, ingests them via Kafka, processes them using Spark, and persists them into Bronze, Silver, and Gold layers using Parquet.

This project is designed specifically for Data Engineering job preparation and portfolio showcase.

## Architecture Overview

Kafka (Order Events)
        ↓
Spark Structured Streaming
        ↓
Bronze Layer (Raw Data)
        ↓
Silver Layer (Cleaned & Deduplicated)
        ↓
Gold Layer (Aggregated Analytics)


## Architecture Diagram
+---------------------+
| Kafka Producer      |
| (Order Events JSON) |
+----------+----------+
           |
           v
+---------------------+
| Kafka Topic         |
| cust_ords           |
+----------+----------+
           |
           v
+------------------------------+
| Spark Structured Streaming   |
|                              |
|  - JSON parsing              |
|  - Schema enforcement        |
|  - Deduplication             |
|  - Checkpointing             |
+----------+-------------------+
           |
           v
+------------------+
| Bronze Layer     |
| Raw Parquet Data |
+------------------+
           |
           v
+------------------+
| Silver Layer     |
| Cleaned Orders   |
+------------------+
           |
           v
+------------------+
| Gold Layer       |
| Aggregations     |
+------------------+



## Tech Stack

Apache Kafka – real-time event ingestion
Apache Spark Structured Streaming – stream processing
PySpark – transformations & aggregations
Parquet – data storage format
Docker – containerized environment
JupyterLab – development & experimentation

## Repository Structure
.
├── data/
│   ├── bronze/        # Raw Kafka data (immutable)
│   ├── silver/        # Cleaned & validated data
│   └── gold/          # Aggregated business metrics
│
├── chk/               # Spark Structured Streaming checkpoints
├── checkpoint_dir_custords_1/
├── checkpoint_dir_custords_2/
│
├── stream_data/
│   └── json_orders/   # Simulated order data (producer input)
│
├── KafkaGenerateOrdersJsonData.ipynb
│   # Kafka producer – generates & streams order events
│
├── Spark_Kafka_Pipeline.ipynb
│   # Spark Structured Streaming pipeline
│
├── .ipynb_checkpoints/
└── README.md

## Pipeline Details
## Kafka Producer

Generates JSON order events

Simulates real-world e-commerce activity

Publishes events to a Kafka topic

Notebook:

KafkaGenerateOrdersJsonData.ipynb

## Bronze Layer (Raw Ingestion)

Reads events from Kafka

Stores raw data as-is

Provides replay and audit capability

✔ Immutable
✔ Schema-flexible
✔ Append-only

## Silver Layer (Clean & Enriched)

Parses timestamps

Filters valid orders (PAID)

Deduplicates using event_id

Flattens JSON structure

✔ Data quality enforcement
✔ Exactly-once semantics

## Gold Layer (Analytics)

Aggregations such as:

Revenue by category

Best-selling products

Optimized for analytics and reporting

✔ Business-ready datasets

## Example Analytics

Best-selling products by quantity

df.groupBy("product_name") \
  .agg(sum("quantity").alias("total_qty")) \
  .orderBy(col("total_qty").desc())

## Production Concepts Demonstrated

Structured Streaming with checkpoints
Exactly-once processing semantics
Kafka offset management
Layered data architecture (Bronze / Silver / Gold)
File-based analytics storage
Partition-aware writes
Data quality handling
Debugging with memory sink

## How to Run (High Level)

Start Kafka & Spark
Run KafkaGenerateOrdersJsonData.ipynb
Run Spark_Kafka_Pipeline.ipynb

Observe data written under:
data/bronze/
data/silver/
data/gold/

This project mirrors real-world data engineering pipelines used in production:
Kafka is treated as a transport layer
Spark handles stream processing
Data is persisted in analytics-ready formats
Checkpoints ensure fault tolerance
It demonstrates practical, hands-on understanding beyond toy examples.

## Possible Enhancements

Delta Lake integration
Windowed aggregations
Schema evolution handling
Monitoring dashboards
CI/CD for data pipelines

👤 Author

Vaibhav Sarkate
Aspiring Data Engineer | Spark | Kafka | SQL | Python
