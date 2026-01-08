# Retail Analytics Pipeline with DuckDB, SQLMesh and Google Cloud

This project, developed for the course *Ambientes Distribuídos de Processamento de Dados*, implements an automated analytics pipeline over large-scale retail data using DuckDB, SQL and SQLMesh, executed on Google Cloud. 

## Objectives

- Validate and explore large transactional, customer and product datasets from a fashion retail chain.  
- Build analytical models for customer and product profiling and temporal sales analysis.  
- Automate the pipeline with SQLMesh and run it in a reproducible way on a Google Cloud VM. 

## Data

- Three main tables: `transactions_train`, `customers`, `articles`.  
- Tens of millions of transaction records and over 1M customers, motivating the use of columnar engines and efficient storage formats. 
- Data is read directly from CSV files stored in Cloud Storage. 

## Technology Stack

- **DuckDB** for analytical SQL over large CSV/Parquet files.
- **SQLMesh** to define models and manage the pipeline DAG and re-runs (`sqlmesh plan`).  
- **Google Cloud Compute Engine + Cloud Storage** for execution and data storage. 
- **Python** script (`pipeline.py`) to orchestrate validation, transformations and exports.

## Pipeline Steps

1. **Data validation and understanding**  
   - Row counts per table, checks for invalid prices, null dates, missing IDs and referential integrity.  
   - Detection of exact duplicate transactions and out-of-range dates, with a summary printed to the console. 

2. **Customer profiling (`customer_profile`)**  
   - Metrics per customer: number of items, total spent, proxy for number of transactions, last purchase date and recency.  
   - Favourite sales channel and favourite department computed via window functions. 

3. **Product profiling (`product_profile`)**  
   - Global metrics: units sold and total sales per article.  
   - Seasonal sales by quarter/season (Winter, Spring, Summer, Autumn) and most frequently co-purchased article (cross‑selling) based on top-N sellers. 

4. **Top customers and their products**  
   - Identification of the top 10 customers by total spent.  
   - Detailed breakdown of which products they buy, quantities, spend and first/last purchase dates. 

5. **Temporal performance**  
   - Monthly sales volume and value by product group and by colour, exported as separate tables. 

6. **Automation and cloud execution**  
   - SQLMesh project (`sqlmesh_hnm`) describing models and dependencies.  
   - Execution on a GCE VM via SSH, using a Python virtual environment, with outputs stored back to Cloud Storage. 

## Outputs

The pipeline generates several analytical tables exported as CSV (and optionally Parquet):

- `customer_profile.csv`  
- `product_profile.csv`  
- `sales_by_group_month.csv`  
- `sales_by_colour_month.csv`  
- `top10_customers.csv`  
- `top10_customers_products.csv` 

## Documentation

- `docs/relatorio-de-ambientes.pdf` – full report describing methodology, performance analysis (CSV vs Parquet) and architecture.  
