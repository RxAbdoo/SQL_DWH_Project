# Data Warehouse Project - End-to-End ETL

## Overview

This project demonstrates a complete Data Warehouse pipeline moving data from **Bronze → Silver → Gold** layers, applying transformations, business rules, and building a **Star Schema** for analytics and BI reporting.

## Architecture Layers

### **Bronze Layer (Raw Layer)**

* Stores raw source data as-ingested without modifications
* Used for traceability and auditing
* Sources include CRM, ERP, and Sales systems

### **Silver Layer (Cleansed & Standardized)**

* Standardizes values (gender, marital status, country, product lines)
* Cleans data (remove duplicates, trim spaces, fix nulls)
* Validates fields (date checks, cost calculations, referential correction)
* Generates the latest record using window functions

### **Gold Layer (Business Layer / Star Schema)**

Designed for analytics, performance, and BI. Includes Fact and Dimension tables.

#### **Star Schema Structure**

```
                    Dim_Date
                       |
                       |
Dim_Product ---- Fact_Sales ---- Dim_Customer
                       |
                       |
                   Dim_Location
```

### **Fact Table: Fact_Sales**

| Column          | Description                        |
| --------------- | ---------------------------------- |
| sales_order_num | Sales Order ID                     |
| product_key     | Product key from product dimension |
| customer_id     | Reference to customer dimension    |
| order_date      | Sale order date                    |
| ship_date       | Shipment date                      |
| due_date        | Due date                           |
| quantity        | Quantity sold                      |
| price           | Unit price                         |
| total_sales     | Calculated sales                   |

### **Dimensions**

#### **Dim_Customer**

| Column         | Description          |
| -------------- | -------------------- |
| customer_id    | Surrogate key        |
| cst_id         | Business customer ID |
| cst_name       | Full customer name   |
| marital_status | Single / Married     |
| gender         | Male / Female        |
| create_date    | Record creation date |

#### **Dim_Product**

| Column       | Description               |
| ------------ | ------------------------- |
| product_key  | Business product key      |
| category_id  | Product category          |
| product_name | Name                      |
| product_line | (Road, Mountain, Touring) |
| cost         | Standard cost             |
| start_date   | Start validity            |
| end_date     | End validity              |

#### **Dim_Location**

| Column      | Description   |
| ----------- | ------------- |
| location_id | Surrogate key |
| country     | Country name  |

#### **Dim_Date**

Generated using calendar logic Including:

| Column    |
| --------- |
| date_key  |
| full_date |
| day       |
| month     |
| year      |
| weekday   |

---

## ETL Orchestration

* Implemented using SQL Stored Procedures and TRY/CATCH error handling
* Full truncate-load approach used for learning and testing
* Can be extended to incremental loads

## Key Transformation Techniques Used

* Window functions (`ROW_NUMBER()`)
* `CASE WHEN` standardization
* `ISNULL()` & validation checks
* Derived columns and field splitting
* `LEAD()` analytic window function for SCD Type 2

---

## Business Value

* Centralized cleansed single version of the truth
* Fast analytics using star schema
* High-quality data for BI dashboards
* Supports enterprise decision making

---

## Future Enhancements

* Incremental / CDC loads
* Audit & Logging tables
* Slowly Changing Dimensions Type 2 (Historical tracking)
* Integration with Power BI / Tableau / Looker
* Automation using Airflow / ADF / SSIS

---

## Author

Technologies: SQL Server, Data Warehousing, ETL, Star Schema, Stored Procedures


