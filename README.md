# Data Warehouse and Analytics Project

This project implements a modern SQL Server data warehouse using a Medallion architecture with `bronze`, `silver`, and `gold` layers. It ingests raw CRM and ERP CSV files, applies data cleaning and standardization, and exposes business-ready views for analytics and reporting.

The repository is designed as a portfolio-grade data engineering project focused on practical warehouse design, ETL logic, data quality validation, and analytical modeling.

## Project Goals

- Consolidate data from multiple source systems into a single analytical warehouse
- Build a layered ETL pipeline in SQL Server
- Clean and standardize raw source data before downstream consumption
- Model business-ready entities for customer, product, and sales analysis
- Create a structure that reflects common real-world data warehousing patterns

## Architecture

The warehouse follows the Medallion pattern:

### Bronze Layer

The Bronze layer stores raw data exactly as it arrives from source CSV files.

- CRM source tables
  - `bronze.crm_cust_info`
  - `bronze.crm_prd_info`
  - `bronze.crm_sales_details`
- ERP source tables
  - `bronze.erp_cust_az12`
  - `bronze.erp_loc_a101`
  - `bronze.erp_px_cat_g1v2`

This layer is optimized for ingestion, not analytics.

### Silver Layer

The Silver layer applies cleansing, normalization, and business rules to the Bronze data.

Examples of transformations implemented in this project:

- Trimming whitespace from text fields
- Deduplicating customer records using `ROW_NUMBER()`
- Standardizing gender and marital status values
- Parsing and validating date fields
- Recalculating invalid sales and price values
- Normalizing country codes into readable country names
- Splitting product keys into category and product identifiers
- Preserving warehouse load timestamps with `dwh_create_date`

### Gold Layer

The Gold layer exposes business-facing analytical views:

- `gold.dim_customers`
- `gold.dim_products`
- `gold.fact_sales`

These views form a star-schema-style analytical model for reporting and SQL analysis.

## Data Flow

1. Raw CRM and ERP CSV files are loaded into Bronze tables using `BULK INSERT`.
2. Bronze data is validated for structural and basic quality issues.
3. Cleaned and standardized data is loaded into Silver tables.
4. Silver data quality checks validate referential integrity and business rules.
5. Gold views join and reshape the curated data into analytics-ready dimensions and facts.

## Tech Stack

- SQL Server
- T-SQL
- SQL Server Management Studio (SSMS)
- CSV source files
- Draw.io for planning and architecture documentation

## Repository Structure

```text
Data Warehouse/
|-- README.md
|-- DDL Bronze Layer.sql
|-- Stored Procedure Insert CSVs into Bronze tables.sql
|-- Checking Data Quality in the Bronze Layer.sql
|-- DDL Silver Layer.sql
|-- Inserting Clean Data From Bronze to Silver Layer.sql
|-- Silver Data Quality Checks.sql
|-- View gold.dim_customers.sql
|-- View gold.dim_products.sql
|-- View gold.fact_sales.sql
`-- Draw.io Project Plan/
    `-- DWH Project Plan.drawio
```

## Key Components

### 1. Bronze Table Creation

`DDL Bronze Layer.sql` creates the raw ingestion tables used to land CRM and ERP data.

### 2. Bronze Load Procedure

`Stored Procedure Insert CSVs into Bronze tables.sql` defines a stored procedure that:

- truncates Bronze tables before reload
- loads CSV files with `BULK INSERT`
- logs load durations
- handles runtime errors with `TRY...CATCH`

Note: the file paths inside the procedure should be updated to match your local dataset location before execution.

### 3. Bronze Data Quality Checks

`Checking Data Quality in the Bronze Layer.sql` contains early validation checks such as:

- duplicate checks
- null checks
- whitespace checks
- invalid date sequence checks

### 4. Silver Table Creation

`DDL Silver Layer.sql` creates the curated Silver tables and adds `dwh_create_date` audit columns.

### 5. Bronze-to-Silver ETL

`Inserting Clean Data From Bronze to Silver Layer.sql` defines the Silver load procedure. It handles:

- customer deduplication
- status and gender normalization
- product category extraction
- product line mapping
- date conversion from integer-based source values
- invalid sales and price correction
- ERP customer ID cleanup
- country standardization

### 6. Silver Data Quality Validation

`Silver Data Quality Checks.sql` validates the curated layer using checks for:

- duplicates
- invalid enums
- invalid dates
- broken customer/product references
- invalid sales calculations

### 7. Gold Analytical Views

The Gold layer is exposed through three views:

- `gold.dim_customers`
  - merges CRM customer data with ERP demographic and location data
- `gold.dim_products`
  - enriches products with category and subcategory attributes
- `gold.fact_sales`
  - connects transactional sales data to customer and product dimensions

## Analytical Model

The Gold layer supports analysis such as:

- customer segmentation
- product performance analysis
- sales trend analysis
- country-level sales reporting
- order and pricing analysis

This is the layer intended for BI tools, dashboards, and ad hoc SQL reporting.

## How to Run the Project

Run the scripts in this order:

1. `DDL Bronze Layer.sql`
2. `Stored Procedure Insert CSVs into Bronze tables.sql`
3. Execute `bronze.load_bonze_layer`
4. `Checking Data Quality in the Bronze Layer.sql`
5. `DDL Silver Layer.sql`
6. `Inserting Clean Data From Bronze to Silver Layer.sql`
7. Execute `silver.load_silver`
8. `Silver Data Quality Checks.sql`
9. `View gold.dim_customers.sql`
10. `View gold.dim_products.sql`
11. `View gold.fact_sales.sql`

## What This Project Demonstrates

- building a layered data warehouse in SQL Server
- designing ETL pipelines with clear separation of raw and curated data
- applying practical data quality checks
- modeling dimensions and facts for analytics
- using SQL as both an engineering and analytics tool

## Future Improvements

- parameterize file paths for easier deployment
- add orchestration with SQL Server Agent or an external scheduler
- introduce incremental loading instead of full reloads
- add surrogate key persistence strategies
- connect the Gold layer to Power BI for dashboarding
- add source-to-target mapping documentation

## Portfolio Value

This project is a strong example of junior data engineering work because it demonstrates the full warehouse lifecycle:

- ingestion
- transformation
- validation
- dimensional modeling
- analytics-ready delivery

It reflects common patterns used in production data platforms, especially in SQL-centric analytics environments.
