## Spark Kafka Streaming Pipeline (End-to-End Data Engineering Project)

his repository implements a production-style real-time data pipeline using Apache Kafka and Apache Spark Structured Streaming.
The pipeline simulates e-commerce order events, ingests them through Kafka, processes them using Spark with schema enforcement, deduplication, and fault tolerance, and persists the data into Bronze, Silver, and Gold layers using Parquet.
This project mirrors how modern data engineering systems are built in real-world environments.

## Architecture Overview

Kafka Producer (Order Events - JSON)
            |
            v
Kafka Topic (custords)
            |
            v
Spark Structured Streaming
  - Schema Enforcement
  - JSON Parsing
  - Offset Management
  - Checkpointing
            |
            v
Bronze Layer (Raw Parquet)
  - Immutable
  - Replayable
            |
            v
Silver Layer (Cleaned & Deduplicated)
  - Filtered (PAID orders)
  - Flattened schema
  - Exactly-once semantics
            |
            v
Gold Layer (Aggregations)
  - Revenue by category
  - Top products
  - Customer spend analytics

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

# Pipeline Details
## Kafka Producer

Generates JSON order events
Simulates real-world e-commerce activity
Publishes events to a Kafka topic

## Notebook:

#KafkaGenerateOrdersJsonData.ipynb
## Bronze Layer (Raw Ingestion)
Reads from Kafka using Spark Structured Streaming
Applies schema using from_json
Writes raw, immutable data to Parquet

### Key Characteristics
Append-only
Schema-flexible
Replayable
Audit-friendly

## Silver Layer (Clean & Enriched)
Converts event timestamps
Filters valid orders (status = PAID)
Deduplicates using event_id
Flattens nested JSON structure

### Key Characteristics
Data quality enforcement
Exactly-once processing (checkpointing + offsets)
Analytics-ready schema

## Gold Layer (Analytics)

Aggregates streaming data for insights
Supports windowed and non-windowed aggregations

### Examples
Revenue by category
Best-selling products
Top customers by spend

df.groupBy("product_name") \
  .agg(sum("quantity").alias("total_qty")) \
  .orderBy(col("total_qty").desc())

## Production Concepts Demonstrated

## Structured Streaming with checkpoints
Kafka offset management
Spark Structured Streaming checkpoints
Exactly-once processing semantics
Schema enforcement on streaming data
Bronze / Silver / Gold architecture
Watermarking & windowed aggregations
Fault-tolerant streaming writes
Memory sink for debugging
Partition-aware Parquet storage

## How to Run (High Level)

Start Kafka & Spark
Run KafkaGenerateOrdersJsonData.ipynb
Run Spark_Kafka_Pipeline.ipynb

Observe data written to:
data/bronze/
data/silver/
data/gold/

## Possible Enhancements

Delta Lake integration (ACID + schema evolution)
Late-arriving data handling
Stateful joins
Data quality metrics & alerts
Monitoring via Spark metrics / Prometheus
CI/CD for streaming pipelines
Cloud deployment (Azure / AWS)

👤 Author

Vaibhav Sarkate
Data Engineer | Spark | Kafka | SQL | Python
