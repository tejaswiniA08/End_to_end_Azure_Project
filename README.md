
# End-to-End Azure Data Engineering Project

This project demonstrates a **production-style Azure data engineering pipeline** built using Azure Data Factory, Azure SQL Database, and cloud data engineering best practices such as **incremental ingestion, looping pipelines, and ELT architecture**.

---

## Azure Resource Setup

<img width="1458" height="694" alt="Screenshot 2025-12-30 at 3 19 18 PM" src="https://github.com/user-attachments/assets/ac02b1f7-8c89-4e75-b506-3c040a81d315" />


All Azure services are organized under a single **Resource Group** for simplified management and cost tracking.

* Resource Group: `Teja_azuregroup`
* Region: **East US**
* Services deployed:
<img width="210" height="202" alt="Screenshot 2026-01-02 at 5 52 44 PM" src="https://github.com/user-attachments/assets/499e01ed-0041-4aae-ba25-9e1f07a8b210" />


  * Azure Data Factory
  * Azure SQL Database
  * Azure Storage Account
  * Azure Databricks

---

## High-Level ELT Architecture


The project follows a modern ELT architecture, where raw data is incrementally ingested from an Azure SQL Database into a cloud data lake using Azure Data Factory, and transformed at scale using Azure Databricks.

This approach improves scalability, performance, and maintainability.

---

## Incremental Ingestion Pipeline (Azure Data Factory)
<img width="573" height="268" alt="Screenshot 2026-01-02 at 2 41 59 PM" src="https://github.com/user-attachments/assets/e74b3d5e-ec81-496e-ae74-e35264706f6e" />


The ingestion pipeline is implemented using **Azure Data Factory** and designed for **incremental loading**.

### Pipeline Logic:

* **ForEach activity** loops through multiple source tables
* **Lookup activity (`last_cdc`)** retrieves the last processed CDC value
* **Copy activity** moves only new or updated records from Azure SQL DB to the data lake(bronze layer)
* **If Condition activity** checks whether new records exist before loading


---

## Medallion Architecture & Databricks ELT Transformations
<img width="1186" height="438" alt="Screenshot 2026-01-02 at 3 36 47 PM" src="https://github.com/user-attachments/assets/c5556148-e33c-4dab-a4ee-a4e4d8560556" />


### 🥉 Bronze Layer – Raw Data Ingestion

* Incremental data ingested from **Azure SQL Database** to **Azure Data Lake** using **Azure Data Factory**
* Change Data Capture (CDC) logic ensures only new or updated records are processed
* Raw data is stored without transformation to preserve source fidelity and enable reprocessing

---

### 🥈 Silver Layer – Cleansed & Enriched Data (Databricks)

Transformations are performed in Databricks using **Apache Spark** with a focus on reliability and scalability.

**Key implementation details:**

* **Databricks Auto Loader** for incremental file ingestion from the data lake
* **Spark Structured Streaming** to handle continuous data arrival
* **Idempotent processing** to avoid duplicate records across re-runs
* **Schema evolution support** to automatically adapt to upstream schema changes
* Data quality checks, type casting, deduplication, and standardization

Transformation logic is modularized into reusable Databricks notebooks/scripts (included in the repository).

---




