# RetailCo — Modern Data Stack Overview

**Author:** Rohith
**Role:** Junior Data Engineer
**Purpose:** A one-page primer on how data flows through a modern organization
and the tools we use at each stage.

---

## 1. The Data Lifecycle

Data engineering is the practice of building reliable systems that move data
from where it is created to where it can be used for decisions. Every dataset
at RetailCo passes through five stages:

1. **Generation** — Data is created at the source: point-of-sale transactions,
   website clicks, inventory scanners, and third-party APIs. Engineers don't
   control this stage, but we must understand the shape and volume of what's
   produced.

2. **Ingestion** — We pull data into our systems. This happens in *batch*
   (scheduled bulk loads, e.g. nightly sales exports) or *streaming*
   (continuous, e.g. live clickstream events). The goal is to capture data
   reliably without losing or duplicating records.

3. **Storage** — Ingested data lands somewhere durable: a data lake for raw,
   unprocessed files, or a data warehouse for clean, query-ready tables. Good
   storage design balances cost, speed, and accessibility.

4. **Transformation** — Raw data is rarely usable as-is. We clean it (fix nulls,
   standardize formats), join it across sources, and aggregate it into business
   concepts like "daily revenue per store." This is where raw data becomes
   trustworthy and meaningful.

5. **Serving** — Finally, transformed data is delivered to its consumers:
   analysts running queries, executives viewing dashboards, and machine-learning
   models making predictions. If serving is slow or wrong, none of the earlier
   work matters.

A key principle across all stages: pipelines should be **reproducible** and
**idempotent** — re-running them produces the same correct result every time.

---

## 2. Modern Data Stack Diagram
```
  [ GENERATION ]   POS systems, website events, APIs, sensors
        |
        v
  [ INGESTION ]    Airbyte, custom Python  (batch + streaming)
        |
        v
  [ STORAGE ]      Data Lake (S3)  -->  Data Warehouse (Snowflake)
        |
        v
  [ TRANSFORM ]    dbt, SQL, Spark
        |
        v
  [ SERVING ]      Power BI, analysts, ML models


  Orchestration (schedules + monitors the whole flow):
  [ AIRFLOW ] ---> wraps around every stage above
```

## 3. Tool Choices by Layer

| Layer            | Chosen Tool        | Why                                                                 |
|------------------|--------------------|---------------------------------------------------------------------|
| **Ingestion**    | Airbyte + Python   | Airbyte has prebuilt connectors for common sources; custom Python covers anything bespoke. Both are free/open-source. |
| **Storage**      | S3 + Snowflake     | S3 cheaply stores raw files of any format (data lake); Snowflake gives fast, scalable analytical queries (warehouse). |
| **Transformation** | dbt + SQL        | dbt brings software engineering practices — version control, testing, documentation — to SQL transformations. |
| **Orchestration** | Apache Airflow    | Industry standard for scheduling, dependency management, and monitoring pipelines as code (DAGs). |
| **Serving / BI** | Power BI           | Widely used in enterprises, strong data modeling and DAX, integrates cleanly with warehouses. |

**Reasoning summary:** This stack favors open-source and widely-adopted tools,
which keeps costs low, makes hiring easier, and means strong community support
when we hit problems. Each tool does one job well rather than trying to do
everything — a principle called *separation of concerns*.

---

## 4. Key Takeaway

A modern data stack is a pipeline of specialized layers — generation, ingestion,
storage, transformation, and serving — tied together by orchestration. The data
engineer's job is to make sure data flows through all of them **reliably,
reproducibly, and at the quality the business can trust.**