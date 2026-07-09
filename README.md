# Automated Supply Chain Analytics Pipeline with n8n, Supabase, and Quadratic

![n8n](https://img.shields.io/badge/n8n-1555D8?style=for-the-badge&logo=n8n&logoColor=white)
![Gmail](https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white)
![Quadratic](https://img.shields.io/badge/Quadratic-5A54A4?style=for-the-badge)
![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)
![Postgres](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)

This project implements a sophisticated, end-to-end supply chain analytics pipeline. It uses **n8n** to automate two key workflows: an initial bulk data load and a trigger-based system that automatically ingests new data from **Gmail** attachments. All processed data is loaded into a **Supabase** Postgres database for final analysis and visualization in **Quadratic**.

The primary goal is to create a self-updating system that transforms raw order data into actionable business intelligence, such as **On-Time In-Full (OTIF)** metrics, available for interactive exploration.

## 🚀 Key Features

* **Dual-Mode ETL (Extract, Transform, Load) Pipeline**: Supports both an initial bulk data load and automated incremental updates.
* **Email-Triggered Automation**: An n8n workflow listens for specific emails and automatically parses attached CSVs (`fact_order_line`, `fact_aggregate`) to ensure the database is always up-to-date.
* **Data Cleaning & Transformation**: Robustly cleans data, merges multiple sources, and performs currency conversion.
* **KPI Calculation**: Computes essential supply chain metrics (OTIF, LFR, VFR) to measure performance.
* **Scalable Architecture**: Uses a modern tech stack with a cloud-based Postgres (Supabase) database.
* **📊 Interactive Analysis**: Connects the processed data to Quadratic for a powerful, spreadsheet-like interface for ad-hoc analysis and visualization.

## 🛠️ Tech Stack

* **Automation/ETL**: [n8n](https://n8n.io/)
* **Data Trigger**: Gmail
* **Database**: [Supabase](https://supabase.com/) (Postgres)
* **Data Analysis & Visualization**: [Quadratic](https://www.quadratichq.com/)
* **Data Manipulation**: Python (within n8n nodes)
* **Data Source**: CSV Files (local and via email attachments)

## 📊 Project Workflow

The project consists of two primary automated workflows orchestrated by n8n.

### 1. Initial Bulk Data Load

This workflow is run once to set up the foundational dataset.
* **Data Ingestion**: Reads a complete set of historical data from local CSV files (`dim_customers`, `dim_products`, etc.).
* **Full Transformation**: Cleans, merges all dimension and fact tables, generates date and exchange rate tables, and calculates metrics like `total_amount`.
* **Database Seeding**: Loads all the cleaned, foundational tables into the Supabase Postgres database.

### 2. Automated Incremental Updates via Email

This workflow runs automatically whenever new data is received.

* **Gmail Trigger**: The workflow is triggered when a new email with specific criteria (e.g., subject line, sender) arrives.
* **Attachment Parsing**: It extracts the attached `fact_order_line` and `fact_aggregate` CSV files.
* **Data Extraction & Loading**: In parallel, it processes each file and inserts the new rows directly into the corresponding `fact_order_line` and `fact_aggregate` tables in the Supabase database.
* **Analysis**: The new data is immediately available in Quadratic for up-to-the-minute analysis.

## 🗂️ Data Schema

The project utilizes two sets of data files.

#### Initial Load Files (`Postgres Input files`)

These files are used for the initial database setup.
* `dim_customers.csv`: Contains information about customers.
* `dim_products.csv`: Contains details about products.
* `dim_targets_orders.csv`: Contains performance targets for each customer.
* `fact_order_line.csv`: Transactional data for every line item in an order.
* `fact_aggregate.csv`: Pre-aggregated summary of order data.

#### Incremental Update Files (`Incoming Mail Data`)

These are examples of files that arrive periodically via email attachments to update the fact tables.
* `fact_order_line_india_2025-05-17.csv`
* `fact_aggregate_india_2025-05-17.csv`
* `fact_order_line_usa_2025-05-17.csv`
* `fact_aggregate_usa_2025-05-17.csv`

## 💡 Final Note

This project serves as a robust template for building real-time, automated analytics pipelines. The architecture is modular, allowing for easy expansion. Future enhancements could include integrating more data sources (e.g., APIs from logistics providers), adding predictive analytics for demand forecasting, or building more complex dashboards in Quadratic.

Contributions, suggestions, and feedback are always welcome! Feel free to open an issue or submit a pull request if you have ideas for improvement.

Happy automating!
