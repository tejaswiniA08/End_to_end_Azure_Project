
# End-to-End Azure Data Engineering Project

This project demonstrates a **production-style Azure data engineering pipeline** built using Azure Data Factory, Azure SQL Database, and cloud data engineering best practices such as **incremental ingestion, looping pipelines, and ELT architecture**.

---

## Azure Resource Setup

<img width="1458" height="694" alt="Screenshot 2025-12-30 at 3 19 18 PM" src="https://github.com/user-attachments/assets/ac02b1f7-8c89-4e75-b506-3c040a81d315" />


All Azure services are organized under a single **Resource Group** for simplified management and cost tracking.

* Resource Group: `Teja_azuregroup`
* Region: **East US**
* Services deployed:

  * Azure Data Factory
  * Azure SQL Database
  * Azure Storage Account
  * Azure Databricks

This setup mirrors how real-world Azure data platforms are structured.

---

## High-Level ELT Architecture


The project follows a **modern ELT architecture**:

* Raw data is ingested into a **Data Lake**
* Transformations occur downstream
* Version control and collaboration are managed using **GitHub**
* Analytical models are built on curated datasets

This approach improves scalability, performance, and maintainability.

---

## Incremental Ingestion Pipeline (Azure Data Factory)
<img width="573" height="268" alt="Screenshot 2026-01-02 at 2 41 59 PM" src="https://github.com/user-attachments/assets/e74b3d5e-ec81-496e-ae74-e35264706f6e" />


The ingestion pipeline is implemented using **Azure Data Factory** and designed for **incremental loading**.

### Pipeline Logic:

* **ForEach activity** loops through multiple source tables
* **Lookup activity (`last_cdc`)** retrieves the last processed CDC value
* **Copy activity** moves only new or updated records from Azure SQL DB to the data lake
* **If Condition activity** checks whether new records exist before loading

This design avoids full table reloads and supports **efficient, scalable ingestion**.

---

## Key Features

* Incremental data ingestion using CDC logic
* Looping pipelines for multiple tables
* Conditional execution to prevent unnecessary loads
* Cloud-native orchestration with Azure Data Factory
* Enterprise-style Azure resource organization

---

