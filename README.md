
A complete end-to-end **Change Data Capture (CDC)** pipeline built using **Databricks Lakeflow Declarative Pipelines (formerly Delta Live Tables)** following the **Medallion Architecture (Bronze → Silver → Gold)**.

This project demonstrates how customer change events are incrementally processed, validated, historized using **Slowly Changing Dimension (SCD) Type 2**, and transformed into business-ready analytics tables.

---

# 📌 Project Overview

Traditional ETL pipelines reload entire datasets, which is inefficient for modern data platforms.

This project showcases a production-style CDC pipeline that processes only incremental changes while maintaining complete historical records.

### Key Capabilities

* ✅ Incremental CDC processing
* ✅ Streaming ingestion
* ✅ Data quality validation
* ✅ Schema standardization
* ✅ SCD Type 2 history tracking
* ✅ Gold-layer aggregations
* ✅ Medallion Architecture implementation

---

# 🏗 Solution Architecture

```text
                Source CDC Data
                      │
                      ▼
        customers_cdc_bronze
                      │
                      ▼
         customers_cdc_clean
              /           \
             ▼             ▼
      customers_history   customers
             │
             ▼
    customers_history_agg
```

---

# 📂 Pipeline Layers

## 🥉 Bronze Layer

**Table**

```text
customers_cdc_bronze
```

### Purpose

* Ingest raw CDC events
* Preserve original source records
* Append-only streaming table

### Events Captured

* INSERT
* UPDATE
* DELETE

---

## 🥈 Silver Layer

**Table**

```text
customers_cdc_clean
```

### Purpose

* Clean incoming records
* Remove invalid data
* Apply data quality expectations
* Standardize schema

### Data Quality Rules

* Customer ID cannot be NULL
* Email must be valid
* Age must be greater than zero

---

## 📜 History Layer

**Table**

```text
customers_history
```

### Purpose

Maintain complete customer history using **Slowly Changing Dimension (SCD) Type 2**.

### Tracks

* Current record
* Previous versions
* Effective start timestamp
* Effective end timestamp

---

## 👥 Current Customer Snapshot

**Table**

```text
customers
```

Contains only the latest customer information.

Ideal for:

* Operational reporting
* Customer dashboards
* Application queries

---

## 🥇 Gold Layer

**Materialized View**

```text
customers_history_agg
```

Business-ready aggregated dataset.

Example Metrics:

* Total Customers
* Active Customers
* Historical Record Count

---

# ⚙ Technologies Used

| Technology                     | Purpose                |
| ------------------------------ | ---------------------- |
| Databricks                     | Cloud Data Platform    |
| Lakeflow Declarative Pipelines | ETL Orchestration      |
| Delta Lake                     | ACID Storage           |
| Apache Spark SQL               | Data Processing        |
| Structured Streaming           | Incremental Processing |
| Unity Catalog                  | Data Governance        |
| Materialized Views             | Reporting Layer        |


```

---

#  Pipeline Flow

```text
Source CDC Data
        │
        ▼
customers_cdc_bronze
        │
        ▼
customers_cdc_clean
        │
        ├────────► customers
        │
        ▼
customers_history
        │
        ▼
customers_history_agg

