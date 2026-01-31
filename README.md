# 🛒 Real-Time E-Commerce Analytics Pipeline - Azure

This project demonstrates a live e-commerce order monitoring system. We simulate U.S. online sales, stream the data through Azure Event Hub, process it in Databricks using PySpark, store it in Delta Lake tables, and visualize insights in Power BI.
---

## 🛠️ Technologies Used

### Data Ingestion

![Azure Event Hub](https://img.shields.io/badge/Azure%20Event%20Hub-0078D4?logo=microsoftazure&logoColor=white&style=for-the-badge) 
![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white&style=for-the-badge)

### Processing & Transformation
  
![PySpark](https://img.shields.io/badge/PySpark-E25A1C?logo=apachespark&logoColor=white&style=for-the-badge)
![Azure Databricks](https://img.shields.io/badge/Azure%20Databricks-FF3621?logo=databricks&logoColor=white&style=for-the-badge)

### Storage

![Delta Lake](https://img.shields.io/badge/Delta%20Lake-0A74DA?logo=data%20bricks&logoColor=white&style=for-the-badge)
![Azure Blob Storage](https://img.shields.io/badge/Azure%20Blob%20Storage-0089D6?logo=microsoftazure&logoColor=white&style=for-the-badge)

### Visualization

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?logo=powerbi&logoColor=black&style=for-the-badge)

### Version Control

![Git](https://img.shields.io/badge/Git-F05032?logo=git&logoColor=white&style=for-the-badge)

---

## 🔍 Project Overview

This project demonstrates a **live e-commerce order monitoring system**:

- Simulates U.S. online sales (TX, NY, CA, etc.)  
- Streams data via **Azure Event Hub**  
- Processes data in **Databricks** using PySpark  
- Stores in **Delta Lake** (Bronze → Silver → Gold architecture)  
- Visualizes insights in **Power BI**  

<img width="6863" height="3358" alt="Strategy and planning" src="https://github.com/user-attachments/assets/e04b329f-9a6f-4f69-a199-54240d4c6fc7" />

---

## 🧾 Disclaimer & Attribution

This project is based on an open-source implementation originally created by  
**Jaya Chandra Kadiveti** and licensed under the MIT License.

The repository has been **adapted, executed, and documented independently**
for **educational and portfolio purposes**, including:
- Project setup and configuration
- Code execution and validation
- Documentation and README restructuring
- Dashboard connection and visualization walkthrough

All original license terms and copyright notices
have been preserved in accordance with the MIT License.

This project is **not intended for commercial redistribution** and is shared
solely to demonstrate practical skills in real-time data engineering.


## 1️⃣ Data Simulation & Event Streaming

📦 Created a Python simulator to continuously send fake U.S.-based e-commerce orders (TX, NY, CA...) to **Azure Event Hub**. [generate_orders.py](simulator/generate_orders.py)

- Events included `order_id`, `state`, `city`, `product`, `quantity`, `price`, `timestamp`
- Used Python libraries: `uuid`, `random`, `json`, `time`, `azure-eventhub`
- Streamed events every second using a for-loop and `send_batch()` method

🪛 Commands:
- Used `pip install` to install Azure SDKs
- Set up Event Hub with namespace `e-commerce-namespace` and hub name `e-commerce-orders`
- Git-tracked simulator files with: `git add Simulator/`

---

## 2️⃣ Real-Time Processing in Databricks

Created a **3-layer Delta Lake architecture** using **Structured Streaming** in PySpark.

### 🍂 Bronze Layer [01_stream_orders_to_bronze.py](databricks_notebooks/01_stream_orders_to_bronze.py)
- Read streaming data directly from Event Hub
- Parsed and flattened JSON events
- Wrote raw data to Azure Blob in Delta format

### 🔧 Silver Layer [02_cleaned_values_silver.py](databricks_notebooks/02_cleaned_values_silver.py)
- Cleaned, filtered, and type-casted Bronze data
- Removed nulls, bad records
- Stored refined data as a Delta table

### 🪙 Gold Layer [03_aggregated_to_gold.py](databricks_notebooks/03_aggregated_to_gold.py)
- Performed aggregation with **1-minute windows** (not hourly)
- Grouped by `state`, `product`, and `timestamp`
- Stored final result table (`gold`) to Blob in Delta

🧾 Notebooks:
- Named `1_bronze_stream.py`, `2_silver_clean.py`, `3_gold_agg.py`
- Git tracked using:  
```bash
cd databricks_notebooks
git add 01-streamorders-to-bronze.py
cd ..
git commit -m "Bronze layer streaming for U.S. order data"

cd databricks_notebooks
git add 02_clean_to_silver.py
cd ..
git commit -m "Cleaned and enriched Bronze data into Silver layer for U.S. cities"

cd databricks_notebooks
git add 03_aggregate_to_gold.py
cd ..
git commit -m "Aggregated Silver data to Gold layer for U.S. states (minute totals)"
```

---

## 3️⃣ Storage – Azure Blob (Delta Format)

Used Azure Blob Storage to persist all three layers in **Delta format**, organized as:

```
abfss://ecommerce@ecommercestoragejk.dfs.core.windows.net/bronze/
abfss://ecommerce@ecommercestoragejk.dfs.core.windows.net/silver/  
abfss://ecommerce@ecommercestoragejk.dfs.core.windows.net/gold/
```

- Delta files were automatically updated via streaming write
- All Power BI visuals connected to the **Gold** layer

---

## 4️⃣ Power BI Dashboard

Connected Power BI to **Databricks SQL Endpoint** to read the Gold Delta Table.

### 🔌 Connection Steps:
- Created SQL Warehouse in Databricks
- Connected Power BI using Azure Databricks connector (with token)
- Loaded `gold` table for visuals

### 📈 Visuals Created:
| Visual Type | Description |
|-------------|-------------|
| 📍 Map Chart | State-wise total sales |
| 📉 Line Chart | Sales trend over time (1-min window) |
| 🍩 Donut Chart 1 | Total sales per state |
| 🍩 Donut Chart 2 | Total items sold per state |
| 📄 Raw Data Table | Sorted by highest sales at top |

<img src="powerbi/Ecommerce_Sales_Dashboard.JPG" alt="Power BI Dashboard" width="900"/>

---

## 🔗 Git Version Control

Used Git CLI for all version tracking:
- `git init`, `git remote add origin ...`
- `git add`, `git commit`, `git push` at every phase
- Maintained clear folder-by-folder commits

---

## ✅ Final Outcome

This project delivers a **fully functional real-time analytics pipeline** powered by Azure and Delta Lake. The dashboard provides operations teams with **minute-level insights** into U.S. e-commerce orders by product, state, and time.

---

## 📄 License

This project is licensed under the **MIT License**.

Original work © 2025 **Jaya Chandra Kadiveti**  
Modifications and documentation adapted for learning and portfolio purposes.

The original license and copyright notice
are retained as required by the MIT License.

See the [LICENSE](LICENSE) file for full details.
