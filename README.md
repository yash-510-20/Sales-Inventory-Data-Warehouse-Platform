# Sales & Inventory Data Warehouse on Azure

## Project Overview

This project demonstrates the design and implementation of an end-to-end **Sales & Inventory Data Warehouse Solution** using Microsoft Azure cloud services.

The solution integrates data from multiple source systems such as:

- SQL Server Databases
- Flat Files (.csv, .txt, .dat)

The data is processed through a modern data engineering architecture using:

- Azure Data Factory (ADF)
- Azure Data Lake Storage Gen2 (ADLS)
- Azure Databricks (PySpark & Spark SQL)
- Azure SQL Database
- Azure Synapse Analytics
- Power BI

The project supports:

- Incremental data loading
- Slowly Changing Dimensions (SCD Type 1 & Type 2)
- Audit logging
- Data validation & rejection handling
- Secure and scalable cloud-based ETL pipelines
- BI reporting and analytics

---

# Architecture Overview

## High-Level Architecture

Source Systems  
↓  
Azure Blob Storage / SQL Server  
↓  
Azure Data Factory (ADF)  
↓  
ADLS Gen2 (RAW Layer)  
↓  
Azure Databricks (Transformation & Validation)  
↓  
Azure SQL DB (Ingestion Layer)  
↓  
Azure SQL DB (Integration Layer / EDW)  
↓  
Azure Synapse Analytics  
↓  
Power BI Dashboards

---

# Technology Stack

| Technology | Purpose |
|------------|---------|
| Azure Data Factory | ETL Orchestration |
| Azure Data Lake Storage Gen2 | Raw & Processed Data Storage |
| Azure Databricks | Data Transformation |
| PySpark / Spark SQL | Data Processing |
| Azure SQL Database | Data Warehouse Storage |
| Azure Synapse Analytics | Analytics & Querying |
| Power BI | Reporting & Dashboards |
| Azure Key Vault | Secret Management |
| Azure Active Directory | Authentication & Authorization |

---

# Project Objectives

- Build a scalable cloud-based Data Warehouse.
- Implement end-to-end ETL pipelines.
- Support incremental and full data loads.
- Implement SCD Type 1 and Type 2 dimensions.
- Enable near real-time reporting.
- Maintain audit and error logging.
- Ensure data quality and compliance.

---

# Data Sources

## SQL Server Tables

- ORDER_HEADER
- ORDER_DETAILS
- PRODUCT
- RETURN_ITEM
- INVENTORY_LEVELS
- BRANCH

## Flat Files

- ORDER_METHOD.csv
- RETURN_REASON.csv
- COUNTRY.txt
- WAREHOUSE.txt
- RETAILER.csv
- PRODUCT_NAME_LOOKUP.csv

---

# ADLS RAW Layer Design

## RAW Layer Structure

RAW/
│
├── SourceName/
│ ├── Target/
│ ├── Archive/
│ ├── Failure/
│ └── Reject/

### Example

RAW/SQLServer/Target/ORDER_HEADER/OrderHeader_20231016143015.csv

---

# ETL Workflow

## Step 1 — Source to RAW Layer

- Extract data from SQL Server and Flat Files.
- Store files in ADLS RAW layer.
- Archive processed files.
- Move failed files to Failure folder.
- Store rejected records in Reject folder.

## Step 2 — RAW to Ingestion Layer

- Perform validation checks.
- Remove duplicate records.
- Reject null primary keys.
- Store valid data in Azure SQL Ingestion tables.

## Step 3 — Ingestion to Integration Layer

- Apply business transformations.
- Generate surrogate keys.
- Implement SCD Type 1 and Type 2 logic.
- Build dimension and fact tables.

---

# Incremental Load Logic

Implemented incremental loading using:

- Watermark table
- Update_Date column
- Last successful load tracking

## Incremental Tables

- ORDER_HEADER
- ORDER_DETAILS

---

# Slowly Changing Dimensions (SCD)

## SCD Type 1

- Overwrites existing records.
- Used for lookup/reference dimensions.

## SCD Type 2

- Preserves historical changes.
- Adds:
  - BEGIN_EFFECTIVE_DATE
  - END_EFFECTIVE_DATE
  - VERSION_NUM
  - ACTIVE_FLAG

### Example

| Product_ID | Product_Name | Version | Active |
|------------|--------------|---------|--------|
| P1 | Product A | 1 | N |
| P1 | Product A Updated | 2 | Y |

---

# Data Warehouse Model

## Dimension Tables

- DIM_ORDER
- DIM_PRODUCT
- DIM_ORDER_METHOD
- DIM_RETURN_REASON
- DIM_COUNTRY
- DIM_WAREHOUSE
- DIM_RETAILER
- DIM_PRODUCT_NAME
- DIM_DATE_TIME

## Fact Tables

### FACT_SALES

Contains:

- Quantity
- Unit Cost
- Unit Price
- Sales Amount
- Returned Quantity

### FACT_INVENTORY

Contains:

- Opening Inventory
- Closing Inventory
- Inventory Additions
- Quantity Sold

---

# Security & Compliance

Implemented:

- Azure Key Vault for secret management
- Azure AD authentication
- Data masking for sensitive columns
- Row-Level Security (RLS)
- Microsoft Defender for SQL

---

# Audit & Error Logging

## TableLog

Tracks:

- Pipeline start/end time
- Source/target counts
- Pipeline status

## Error_Logging

Tracks:

- Error messages
- Failed activities
- Failure timestamps

---

# Orchestration

## SQL Source Pipelines

- Full Load
- Incremental Load
- Retry mechanism (3 retries)
- Scheduled daily at 9 AM IST

## Flat File Pipelines

- Wait for file arrival (10 mins)
- Ad-hoc execution
- Notification alerts for missing files

---

# Naming Conventions

## Pipelines

PL_EXT_SRC_TGT_ZONE_FULL_DAILY

## Datasets

DS_ADLS_ORDER_HEADER

## Linked Services

LS_SQL_SALESDB

## Activities

ACT_MT_Copy_LoadRawData

---

# Key Features

✔ End-to-End Azure Data Engineering Project  
✔ Incremental ETL Processing  
✔ SCD Type 1 & Type 2  
✔ Dynamic Pipelines  
✔ Watermark Framework  
✔ Audit Logging  
✔ Error Handling  
✔ Data Validation  
✔ Data Masking & Security  
✔ Power BI Reporting

---

# Folder Structure

project/
│
├── adf/
├── databricks/
├── sql/
├── pipelines/
├── notebooks/
├── config/
├── documents/
└── powerbi/

---

# Future Enhancements

- CI/CD using Azure DevOps
- Real-time streaming using Event Hub
- Delta Lake implementation
- Unity Catalog integration
- Data quality monitoring dashboards

---

# Conclusion

This project demonstrates enterprise-level implementation of a modern cloud-based data warehouse solution using Azure technologies. It showcases practical experience in ETL orchestration, cloud architecture, data modeling, data governance, and BI reporting.

---

# Author

Yash More
Azure Data Engineer
