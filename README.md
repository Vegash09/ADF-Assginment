Sales View Data Engineering Project
📌 Overview

This project builds an end-to-end data pipeline using Azure Data Factory (ADF), Azure Data Lake Storage (ADLS), and Databricks.
The pipeline ingests raw CSV files, processes them through the Bronze → Silver → Gold architecture, and prepares curated data for analytics.

🚀 Workflow
1. Data Ingestion (ADF + ADLS)

Created an ADLS container → sales_view_devtst.

Folders created: customer, product, store, sales.

Uploaded CSV files into each folder.

Built an ADF pipeline to:

Identify the latest modified file from each folder.

Copy the latest file into the Bronze layer (bronze/sales_view/{folder}).

Used Foreach activity with variables:

latestFile → stores the most recently modified file.

referenceDate → initialized to 1900-01-01 00:00:00 for comparison.

2. Bronze → Silver Transformations (Databricks)
Customer

Converted headers → snake_case (UDF).

Split name → first_name, last_name.

Extracted domain from email.

Mapped gender → M/F.

Split joining_date into date (yyyy-MM-dd) and time.

Derived expenditure_status → (MINIMUM if spent < 200 else MAXIMUM).

Upserted into → silver/sales_view/customer.

Product

Converted headers → snake_case.

Added sub_category (mapping category_id → category).

Upserted into → silver/sales_view/product.

Store

Converted headers → snake_case.

Extracted store_category from email domain.

Formatted created_at & updated_at → yyyy-MM-dd.

Upserted into → silver/sales_view/store.

Sales

Converted headers → snake_case.

Upserted into → silver/sales_view/customer_sales.

3. Silver → Gold Transformation

Joined product and store tables → selected attributes such as store_name, location, product_name, price, stock_quantity, image_url etc.
