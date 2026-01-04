# End-to-End Azure Data Engineering Project

This project demonstrates an **Azure-based data engineering pipeline** built using **Azure Data Factory**, **Azure SQL Database**, **Azure Data Lake Gen2**, and **Azure Databricks**.
The implementation follows an **ELT-based medallion architecture** with **incremental ingestion and transformations**.

---

## Azure Resource Setup

All Azure services are organized under a single **Resource Group**.

<img width="1458" alt="Azure Resources" src="https://github.com/user-attachments/assets/ac02b1f7-8c89-4e75-b506-3c040a81d315" />

**Resource details:**

* Resource Group: `Teja_azuregroup`
* Region: East US
* Services:

  * Azure Data Factory
  * Azure SQL Database
  * Azure Storage Account (Data Lake Gen2)
  * Azure Databricks

<img width="210" alt="Azure Services" src="https://github.com/user-attachments/assets/499e01ed-0041-4aae-ba25-9e1f07a8b210" />

---

## High-Level ELT Architecture

The project follows an **ELT architecture**, where raw data is incrementally ingested from **Azure SQL Database** into **Azure Data Lake Gen2** using **Azure Data Factory**, and transformations are performed using **Azure Databricks**.

Architecture Overview

Source: Azure SQL Database

Ingestion: Azure Data Factory

Storage: Azure Data Lake Gen2

Transformations: Azure Databricks

Table Format: Delta Lake

Data flows through Bronze → Silver → Gold layers.
---

## Incremental Ingestion Pipeline (Azure Data Factory)

**Source:** Azure SQL Database
**Target:** Data Lake Gen2 (Bronze layer)

<img width="573" alt="ADF Pipeline" src="https://github.com/user-attachments/assets/e74b3d5e-ec81-496e-ae74-e35264706f6e" />

### Pipeline logic:

* ForEach activity iterates over multiple source tables
* Lookup activity (`last_cdc`) retrieves the last processed CDC value
* Copy activity loads only new or updated records
* If Condition activity checks for new records before loading

<img width="600" alt="ADF Conditional Logic" src="https://github.com/user-attachments/assets/c5556148-e33c-4dab-a4ee-a4e4d8560556" />

---

## Medallion Architecture

<img width="500" alt="Medallion Architecture" src="https://github.com/user-attachments/assets/b74caf7c-d30c-488a-973c-423ad7f68f35" />

---

## Bronze Layer

* Stores raw, incrementally ingested data
* Data is loaded from Azure SQL Database using Azure Data Factory
* No transformations are applied
* Acts as the source for downstream Databricks processing

---

## Silver Layer (Databricks)

* Data is read from the Bronze layer
* Incremental processing is applied
* Data cleansing, type casting, and deduplication are performed
* Tables are written in Delta format

---

## Gold Layer

* Gold tables are derived from Silver layer data
* Data is structured for analytical use
* Tables are stored in Delta format

---

## Star Schema

* Gold layer tables follow a star schema
* Fact tables store measurable data
* Dimension tables store descriptive attributes
* Fact tables reference dimensions using keys

---

## Slowly Changing Dimensions (SCD)

* Slowly Changing Dimension logic is implemented for dimension tables
* New records are inserted when attribute values change
* Historical records are preserved
* Current and historical records are identified using flags or date columns

---

## Metadata-Driven Pipelines (Jinja2)

* Pipeline logic is parameterized using Jinja2 templates
* Table names, columns, and paths are passed dynamically
* Reduces hardcoded logic across notebooks and pipelines

---

## Databricks Incremental Data Load

* Incremental updates are handled using Delta Lake
* Existing records are updated and new records are inserted
* Full table reloads are avoided

---

## Delta Live Tables (DLT)

* Includes a Delta Live Tables (DLT) implementation
* Tables are defined declaratively using SQL or Python
* Demonstrates dependency handling within Databricks pipelines

---

## Databricks Asset Bundles

* Databricks Asset Bundles are used to organize notebooks and jobs
* Bundle configuration files define Databricks resources
* Supports deployment using Databricks CLI
* Enables version-controlled Databricks workflows

---


Just tell me.
