# Customer Invoice Analytics: Lakehouse Medallion Framework

## Overview
This project builds a data lakehouse on Azure Databricks using the Lakehouse Medallion architecture. By organizing data into bronze (raw), silver (cleaned), and gold (enriched) layers, it facilitates efficient data management and processing. This project sets up the infrastructure, develops data pipelines, and executes them to populate these layers, ultimately unlocking valuable insights for data-driven applications. 

## Architecture Setup
- **Azure Cloud Subscription**: Utilize an Azure subscription for deploying resources.
- **Azure Databricks Workspace Creation**: Establish an Azure Databricks workspace to facilitate notebook creation, cluster management, etc.
- **Azure Storage Account Creation**: Set up an Azure Storage Account to serve as the storage solution for various data components.
- **Container Creation**:
   - **Metastore-Root**: Created to store managed tables data.
   - **Datazone/Landing Zone (Staging_Area)**: Intended for storing input data files.

## Access Management:
- **Access Connector for Azure Databricks**: Establish an Access Connector with managed identity to enable Azure Databricks access to data within the workspace.

## Unity Catalog Setup:
- **Unity Catalog Creation**:
   - Use the Databricks workspace to create a Unity catalog (metastore).
   - Link it to the metastore-root container in the Azure Storage Account.
   - Provide the Resource ID of the Access Connector for Azure Databricks to ensure seamless data access.

## Notebook Execution:
- **Notebook Implementation**:
   - Follow the Lakehouse Medallion architecture to process data and build data pipelines.
   - **Lakehouse Medallion Architecture Layers**:
     - **Storage Layer**: Physical storage for input data as well as data within managed tables and volumes.
     - **Logical Storage Layer**: Management of catalogs, databases, tables, and views as logical storage objects.
     - **Unity Catalog (Data Governance Solution)**:
       - Handles metadata management, user management, and access management functions.
       - Facilitates user access to both logical and physical storage through the Unity catalog, enforcing permissions and security.
     - **Databricks Workspace**: Utilized for creating notebooks and clusters to process data effectively.

## Notebook Steps:
- **Create External Location**: Define an external location to access input data from Azure Data Lake Storage using SQL commands (`CREATE EXTERNAL LOCATION`).
- **Create Catalog and Databases**: Establish a catalog (`projects_catalog`) and three databases (`bronze_db`, `silver_db`, `gold_db`) using SQL commands (`CREATE CATALOG`, `CREATE DATABASE`).
- **Read Input Data**: Utilize Spark DataFrame to read input data (customers and invoices) from the specified location.
- **Create Bronze Layer Tables**: Create tables (`customers_raw` and `invoices_raw`) in the bronze layer from Spark DataFrames representing raw input data.
- **Check Raw Tables Records**: Perform count operations on raw tables to verify the number of records.
- **Create Silver Layer Tables**: Define tables (`customers_cleaned` and `invoices_cleaned`) in the silver layer and populate them from the bronze layer tables with necessary transformations and filtering.
- **Implement SCD Type 2 in Customers Dimension Table**: Apply Slowly Changing Dimension (SCD) Type 2 logic on the `customers` dimension table in the silver layer to capture data changes.
- **Remove Duplicate Records from Invoices**: Read the `invoices_cleaned` table, remove duplicates, and create a new table (`invoices`) in the silver layer with unique records, partitioned by country.
- **Create View in Gold Layer**: Establish a view (`country_yearly_sales`) in the gold layer to monitor yearly sales of each country.
