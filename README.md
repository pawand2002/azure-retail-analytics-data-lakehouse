# Azure Retail Sales Data Analytics: Modernizing ETL with a Data Lakehouse Approach

## Project Overview

This project demonstrates the modernization of a retail sales data pipeline on Microsoft Azure, showcasing a shift from traditional data warehousing and ETL methods to a scalable, cost-effective, and cloud-native data lakehouse architecture. It covers end-to-end batch ingestion, multi-layered data transformations (Bronze, Silver, Gold), and analytical reporting.

## Architecture

Project Architecture Diagram
This diagram outlines the end-to-end data pipeline, from raw data ingestion to final reporting, following a Medallion Architecture pattern.

+---------------------+
|                     |
|  Raw Sales Data     |
|  (Local Python)     |
|                     |
+----------+----------+
           |
           | Ingest (CSV)
           V
+---------------------+
|                     |
|  Azure Data Lake    |
|  Storage Gen2       |
|  (Bronze Layer)     |
|  salesdata/bronze/  |
+----------+----------+
           |
           | Data Flow: Bronze to Silver
           | (Cleaning, Schema Enforcement)
           V
+---------------------+
|                     |
|  Azure Data Lake    |
|  Storage Gen2       |
|  (Silver Layer)     |
|  salesdata/silver/  |
+----------+----------+
           |
           | Data Flow: Silver to Gold
           | (Aggregation, Business Logic)
           V
+---------------------+
|                     |
|  Azure Data Lake    |
|  Storage Gen2       |
|  (Gold Layer)       |
|  salesdata/gold/    |
+----------+----------+
           |
           | Query via Views
           V
+---------------------+
|                     |
|  Azure Synapse      |
|  Analytics          |
|  (Serverless SQL    |
|  Pool - Views)      |
+----------+----------+
           |
           | DirectQuery
           V
+---------------------+
|                     |
|  Power BI           |
|  (Dashboard &       |
|  Reporting)         |
|                     |
+---------------------+

Explanation of Components and Data Flow:
Raw Sales Data (Local Python):

Role: The starting point of the pipeline. A Python script generates synthetic raw sales data in CSV format.

Flow: This raw CSV data is the initial input to the data lake.

Azure Data Lake Storage Gen2 (Bronze Layer):

Role: The landing zone for all raw, immutable data. Data is stored as-is, preserving its original state.

Location: abfss://salesdata@youradlgen2.dfs.core.windows.net/bronze/

Flow: Raw CSV files are uploaded here.

Azure Data Factory (Data Flow: Bronze to Silver Transformation):

Role: An ETL/ELT service responsible for orchestrating data movement and performing transformations. The first Data Flow reads raw data from the Bronze layer.

Transformation: It cleans the data, enforces schema (e.g., correct data types, handling missing values), and converts it into a more optimized format (Parquet).

Flow: Processes data from Bronze and writes it to Silver.

Azure Data Lake Storage Gen2 (Silver Layer):

Role: Stores cleaned, conformed, and structured data. This layer is reliable and ready for further processing or feature engineering.

Location: abfss://salesdata@youradlgen2.dfs.core.windows.net/silver/

Flow: Receives processed Parquet files from the Bronze-to-Silver Data Flow.

Azure Data Factory (Data Flow: Silver to Gold Aggregation):

Role: The second Data Flow in ADF reads the cleaned data from the Silver layer.

Transformation: It applies business logic, performs aggregations (e.g., calculating daily total sales, unique customers, total transactions), and prepares the data for direct analytical consumption.

Flow: Processes data from Silver and writes the aggregated results to Gold.

Azure Data Lake Storage Gen2 (Gold Layer):

Role: The curated, business-ready layer. Data here is highly optimized for performance and direct consumption by reporting tools.

Location: abfss://salesdata@youradlgen2.dfs.core.windows.net/gold/

Flow: Receives aggregated Parquet files from the Silver-to-Gold Data Flow.

Azure Synapse Analytics (Serverless SQL Pool - Views):

Role: Provides a flexible, on-demand SQL interface over the data stored in ADLS Gen2. It allows querying the Parquet files in the Gold layer using standard T-SQL, without managing dedicated infrastructure. Logical views are created here (e.g., gold_daily_sales_summary) to simplify data access.

Flow: Queries the Gold layer data directly from ADLS Gen2.

Power BI (Dashboard & Reporting):

Role: The final visualization layer. Power BI connects to the Synapse Serverless SQL Pool views using DirectQuery mode.

Flow: Retrieves aggregated data from Synapse views in real-time to create interactive dashboards and reports for business users.

This architecture demonstrates a modern data lakehouse approach, combining the scalability and flexibility of a data lake with the structure and query capabilities of a data warehouse.

**Layers Explained:**

* **Bronze Layer (Raw Data Lake):** Stores raw, immutable sales data as ingested from the source (CSV). This layer acts as a landing zone.
* **Silver Layer (Cleaned & Conformed Data Lake):** Data from the Bronze layer is cleaned, validated, and transformed into a structured, conformed format (Parquet). This layer is ready for further aggregation and feature engineering.
* **Gold Layer (Business-Ready Data Lake):** Aggregated and highly optimized data (Parquet) for direct consumption by business intelligence tools and analytical applications. This layer contains key business metrics like daily total sales, unique customers, and transaction counts.

## Azure Services Used

* **Azure Data Lake Storage Gen2 (ADLS Gen2):** Scalable and secure data lake for storing data in various formats across all layers (Bronze, Silver, Gold).
* **Azure Data Factory (ADF):** Cloud-based ETL/ELT service used for orchestrating the data movement and performing complex data transformations via Mapping Data Flows.
* **Azure Synapse Analytics (Serverless SQL Pool):** Provides a SQL interface over the data in ADLS Gen2, enabling ad-hoc querying and creating logical views (Gold Layer) without provisioning dedicated resources.
* **Power BI:** Business intelligence tool for creating interactive dashboards and reports, consuming data directly from the Synapse Serverless SQL Pool view (DirectQuery).

## Key Learnings & Demonstrated Skills

This project was a hands-on exercise in transitioning to modern cloud data platforms, specifically on Azure. It highlights my ability to:

* **Architect Cloud-Native Data Solutions:** Designed and implemented a robust Medallion Architecture, a best practice for data lakes.
* **Master Modern ETL/ELT:** Gained in-depth proficiency in Azure Data Factory's Data Flows for complex data transformations and pipeline orchestration.
* **Leverage Data Lakehouse Capabilities:** Successfully integrated ADLS Gen2 with Azure Synapse Analytics to provide a flexible and performant analytical layer directly on the data lake.
* **Ensure Data Quality & Integrity:** Implemented data cleaning and aggregation steps to deliver high-quality, business-ready data.
* **Integrate BI Tools:** Connected Power BI to a Synapse view using DirectQuery, showcasing real-time analytical capabilities.
* **Problem-Solving & Debugging:** Successfully navigated and resolved complex data type inference, authentication, and pathing challenges across different Azure services.
* **Multi-Cloud Versatility:** This project complements my existing expertise in GCP, demonstrating my adaptability and capability to design and implement robust data solutions across major cloud platforms. It showcases my commitment to continuous learning and expanding my cloud data engineering skillset.

## Getting Started: Deploying and Running the Project

Follow these steps to deploy and run the entire data pipeline in your Azure environment:

### **Phase 1: Azure Resource Deployment**

1.  **Azure Subscription:** Ensure you have an active Azure subscription. A [free trial](https://azure.microsoft.com/en-us/free/) is sufficient.
2.  **Resource Group Creation:**
    * Go to the Azure Portal.
    * Search for "Resource groups" and click **+ Create**.
    * Provide a name (e.g., `retail-analytics-rg`) and select your region. Click **Review + create**, then **Create**.
3.  **Azure Data Lake Storage Gen2 (ADLS Gen2) Deployment:**
    * In the Azure Portal, search for "Storage accounts" and click **+ Create**.
    * Select your Resource Group.
    * **Storage account name:** Choose a unique name (e.g., `pawandlgen2retail`).
    * **Region:** Select the same region as your Resource Group.
    * **Performance:** `Standard`.
    * **Redundancy:** `Locally-redundant storage (LRS)`.
    * Go to the **Advanced** tab, under "Data Lake Storage Gen2," enable **"Hierarchical namespace"**.
    * Review + create, then Create.
    * **Create Containers:** Once deployed, navigate to the storage account, go to **Containers**, and create the `salesdata` container. Inside the `salesdata` container, create folders named `bronze`, `silver`, and `gold`.
4.  **Azure Data Factory (ADF) Deployment:**
    * In the Azure Portal, search for "Data factories" and click **+ Create**.
    * Select your Resource Group.
    * **Name:** Choose a unique name (e.g., `pawan-retail-adf`).
    * **Region:** Select the same region.
    * **Version:** `V2`.
    * Review + create, then Create.
5.  **Azure Synapse Analytics Workspace Deployment:**
    * In the Azure Portal, search for "Azure Synapse Analytics workspaces" and click **+ Create**.
    * Select your Resource Group.
    * **Workspace name:** Choose a unique name (e.g., `pawan-retail-synapse-001`).
    * **Region:** Select the same region.
    * **Select Data Lake Storage Gen2:** Choose your `pawandlgen2retail` account.
    * **File system name:** Select your `salesdata` container.
    * **SQL Administrator credentials:** Set a strong username and password for the Synapse SQL Administrator. **Remember these credentials!**
    * Review + create, then Create.

### **Phase 2: Data Generation & Initial Ingestion (Bronze Layer)**

1.  **Generate Sample Data:**
    * Ensure you have Python installed locally.
    * Run the `generate_sales_data.py` script (provided in this repo) to create `sales_data.csv`.
2.  **Upload to Bronze Layer:**
    * Go to your `pawandlgen2retail` storage account in the Azure Portal.
    * Navigate to the `salesdata` container -> `bronze` folder.
    * Upload the `sales_data.csv` file into this folder.

### **Phase 3: Azure Data Factory Pipeline Setup**

1.  **Launch ADF Studio:** From your ADF resource in the Azure Portal, click "Launch studio."
2.  **Create Linked Services:**
    * In ADF Studio, go to the **Manage** hub (monitor icon).
    * Under "External connections," select **Linked services**.
    * Click **+ New**.
    * Search for `Azure Data Lake Storage Gen2`.
    * **Name:** `ls_adls_gen2_retail`.
    * **Authentication type:** `Managed Identity`.
    * **Storage account name:** Select your `pawandlgen2retail` account.
    * Test connection and Create.
3.  **Create Datasets:**
    * In ADF Studio, go to the **Author** hub (✍️ icon).
    * Under "Factory Resources," select **Datasets**.
    * **`ds_bronze_sales_csv`:**
        * Type: `Azure Data Lake Storage Gen2`, Format: `Delimited text`.
        * Linked service: `ls_adls_gen2_retail`.
        * File path: `salesdata/bronze/sales_data.csv`.
        * First row as header: Checked. Import schema: From connection/store.
    * **`ds_silver_sales_parquet`:**
        * Type: `Azure Data Lake Storage Gen2`, Format: `Parquet`.
        * Linked service: `ls_adls_gen2_retail`.
        * File path: `salesdata/silver/sales_cleaned/` (no specific file name).
        * Import schema: None.
    * **`ds_gold_daily_sales_parquet`:**
        * Type: `Azure Data Lake Storage Gen2`, Format: `Parquet`.
        * Linked service: `ls_adls_gen2_retail`.
        * File path: `salesdata/gold/daily_sales_summary/` (no specific file name).
        * Import schema: None.
4.  **Import Data Flows and Pipeline:**
    * Use the JSON files provided in this GitHub repository for `df_bronze_to_silver_sales.json`, `df_silver_to_gold_sales.json`, and `pl_bronze_to_silver_sales.json`.
    * In ADF Studio, go to the **Author** hub.
    * For Data Flows and Pipelines, click the three dots `...` next to their respective sections, then select **"Import"**. Upload the JSON files.
    * **Review and Adjust:** After import, open each Data Flow and Pipeline. Verify that all linked services and dataset paths are correctly mapped to your deployed resources. Pay close attention to the `toString(SalesDate, 'yyyy-MM-dd')` in `finalPrepareForSink` within `df_silver_to_gold_sales` and `count(distinct toString(CustomerID))` or `size(distinct(collect(CustomerID)))` expressions.
5.  **Publish All:** Click the **"Publish all"** button at the top of ADF Studio to save all changes.

### **Phase 4: Azure Synapse Analytics Setup**

1.  **Launch Synapse Studio:** From your Synapse Workspace in the Azure Portal, click "Open Synapse Studio."
2.  **Create Database:**
    * In Synapse Studio, go to the **Develop** hub (paper icon).
    * Click **+** > **SQL script**.
    * Ensure connected to `Built-in (serverless)`.
    * Run: `CREATE DATABASE retail_analytics_db;`
    * Publish the script.
3.  **Create Master Key, Credential, and External Data Source:**
    * In the same SQL script (or a new one), ensure connected to `retail_analytics_db`.
    * Run the SQL from `CREATE MASTER KEY.sql` (replace `'YourStrongPassword123!'` with a strong password).
    * Run the SQL from `CREATE DATABASE SCOPED CREDENTIAL and EXTERNAL DATA SOURCE.sql` (replace `youradlgen2` with your storage account name, ensure `LOCATION` is `https://youradlgen2.dfs.core.windows.net/salesdata/`).
    * Publish the script.
4.  **Create Gold Layer View:**
    * Open `CREATE OR ALTER VIEW gold_daily_sales_summary.sql` from this repo.
    * Ensure connected to `retail_analytics_db`.
    * Verify `BULK` path is `gold/daily_sales_summary/*.parquet` and `DATA_SOURCE = 'adls_gen2_source'`.
    * Verify `CAST(sales.SalesDate AS DATE) AS SalesDate` in the SELECT list.
    * Run the script.
    * Publish the script.

### **Phase 5: Execute Pipeline & Verify Data**

1.  **Trigger ADF Pipeline:**
    * In ADF Studio, go to the **Author** hub > **Pipelines**.
    * Open `pl_bronze_to_silver_sales`.
    * Click **"Add trigger"** > **"Trigger now"**.
    * Monitor the run in the **Monitor** hub. Ensure it succeeds.
2.  **Verify Gold Layer in Synapse:**
    * Once the pipeline is successful, go to Synapse Studio.
    * Open a new SQL script, connect to `retail_analytics_db`.
    * Run: `SELECT TOP 100 * FROM gold_daily_sales_summary ORDER BY SalesDate DESC, Region;`
    * Confirm data is present and `SalesDate` is not null.

### **Phase 6: Power BI Dashboard**

1.  **Open Power BI Desktop.**
2.  **Connect to Synapse Serverless SQL Pool View:**
    * Get Data > Azure Synapse Analytics SQL database.
    * Server: Your Synapse Serverless SQL endpoint (e.g., `pawan-retail-synapse-001-ondemand.sql.azuresynapse.net`).
    * Database: `retail_analytics_db`.
    * Data Connectivity mode: **DirectQuery**.
    * Authenticate.
    * Select `dbo.gold_daily_sales_summary` and Load.
3.  **Build Dashboard:** Create visuals as described in the Power BI Dashboard Creation Steps (e.g., Card for Total Sales, Line Chart for Sales Trend by Date, Bar Chart for Sales by Region, Slicer for Month).
4.  **Save and Publish:** Save your `.pbix` file and publish it to Power BI Service.

## Cost Management

**IMPORTANT:** To avoid incurring unnecessary costs, please remember to delete all Azure resour
