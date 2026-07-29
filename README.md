# 📦 Inventory Data Cleaning and Visualization using Python

A Python data analytics project that connects directly to a **Microsoft SQL Server (SSMS)** database, extracts live inventory records with **Pandas**, cleans and transforms the data, and visualizes warehouse performance using **Matplotlib** and **Seaborn**.

![Python](https://img.shields.io/badge/Python-3.x-blue)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-11557C)
![Seaborn](https://img.shields.io/badge/Seaborn-Statistical%20Plots-4C72B0)
![SQL Server](https://img.shields.io/badge/Database-SQL%20Server-CC2927)

---

## 📌 Project Overview

This project analyzes inventory and order data for a multi-warehouse retail/e-commerce style operation. Instead of reading from a static file, the data is pulled **live from a SQL Server database** using `pyodbc`, giving the project a realistic, industry-style data pipeline: **Database → Python → Cleaned DataFrame → Visual Insights**.

The goal is to answer practical inventory-management questions:
- Which warehouse is handling the most order volume?
- Is stock availability keeping pace with order demand?
- Are certain products consistently over- or under-stocked?

---

## 🎯 Objective

- Connect to a live SQL Server database from Python.
- Extract the `Inventory` table into a Pandas DataFrame.
- Clean the data (missing values, data types, category consistency).
- Perform exploratory analysis and visualize warehouse/order/stock trends.
- Turn raw operational data into insights that support inventory decisions.

---

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| **Python** | Core programming language for the entire pipeline |
| **Jupyter Notebook** | Interactive environment for step-by-step analysis |
| **pyodbc** | Connects Python to the SQL Server database via ODBC driver |
| **Pandas** | Data extraction (`read_sql`), cleaning, and transformation |
| **Matplotlib** | Base plotting engine — figure sizing, titles, axis labels |
| **Seaborn** | Statistical visualizations (bar plots, heatmaps, scatter plots) |
| **Microsoft SQL Server (SSMS)** | Source database storing the raw `Inventory` table |

---

## 🗄️ Data Source

Unlike typical portfolio projects that read a CSV, this project queries a **live database**:

```python
import pyodbc
import pandas as pd

conn = pyodbc.connect(
    "DRIVER={ODBC Driver 17 for SQL Server};"
    "SERVER=LAPPY;"
    "DATABASE=Project;"
    "Trusted_Connection=yes;"
)

query = "SELECT * FROM Inventory"
df = pd.read_sql(query, conn)
```

This demonstrates the ability to work with **real database connectivity** (SQL Server + ODBC + Windows Trusted Authentication) rather than only static files — a skill closer to real workplace data pipelines.

> **Note:** A flattened export of the same table (`inv.csv`) is included in this repo purely as a portable snapshot of the data, since the live SSMS connection can't be shared outside the original machine.

---

## 📊 Dataset Overview

The `Inventory` table contains **120 order records** across **7 columns**:

| Column | Data Type | Description | Business Significance |
|---|---|---|---|
| `Order_ID` | Text | Unique order identifier (e.g., `ORD001`) | Primary key for tracking each transaction |
| `Date_order` | Date (converted from text) | Date the order was placed | Enables time-based trend analysis |
| `Product_Name` | Text | Item ordered (10 categories: Laptop, Mouse, Keyboard, Monitor, Printer, Speaker, Scanner, Router, Webcam, SSD) | Identifies which products drive demand |
| `Stock` | Numeric | Units remaining in inventory | Core signal for stockout / overstock risk |
| `Orders` | Numeric | Units ordered in that transaction | Measures demand volume |
| `Status_Order` | Text | Delivered / Pending / Shipped / Cancelled | Tracks fulfillment pipeline health |
| `Warehouse` | Text | Fulfilling location (Pune, Mumbai, Chennai, Bengaluru) | Identifies regional workload distribution |

**Data quality found:** `Stock` had 7 missing values and `Orders` had 6 missing values; no duplicate rows were present.

---

## 🧹 Data Cleaning Steps

| Step | Pandas Function | Why |
|---|---|---|
| Inspect structure & data types | `df.info()`, `df.dtypes` | Understand columns, types, and non-null counts before cleaning |
| Check missing values | `df.isnull().sum()` | Quantify data quality issues |
| Fill missing `Stock` | `df['Stock'].fillna(df['Stock'].mean())` | Mean imputation preserves the overall stock distribution |
| Fill missing `Orders` | `df['Orders'].fillna(df['Orders'].median())` | Median is more robust to outliers for order counts |
| Check duplicates | `df.duplicated().sum()` | Confirm no repeated order records (result: 0) |
| Convert date column | `pd.to_datetime(df['Date_order'])` | Enables date-based operations and correct sorting |
| Standardize category labels | `df['Warehouse'].replace('Bangalore', 'Bengaluru')` | Fixes naming inconsistency so the city is grouped correctly in analysis |
| Statistical summary | `df.describe()` | Sanity-check ranges, means, and spread after cleaning |

---

## 📈 Visualizations (Matplotlib + Seaborn)

1. **Bar Chart — Total Orders by Warehouse**
   `sns.barplot()` on `df.groupby('Warehouse')['Orders'].sum()` → shows which warehouse handles the highest order volume.

2. **Heatmap — Correlation Matrix (Stock vs Orders)**
   `sns.heatmap()` with `annot=True` → quantifies how strongly stock levels and order volume move together.

3. **Scatter Plot — Stock vs Orders (colored by Warehouse)**
   `sns.scatterplot(hue='Warehouse')` → visualizes the stock-to-demand relationship per warehouse.

4. **Pie Chart — Percentage Share of Orders by Warehouse**
   `plt.pie()` with `autopct='%1.1f%%'` → shows each warehouse's proportional contribution to total orders.

---

## 💡 Key Business Insights

- **Mumbai** handled the highest total order volume, followed by **Bengaluru**, with **Pune** and **Chennai** trailing.
- **Stock and Orders show a strong positive correlation (~0.84)** — warehouses/products with higher stock levels also tend to receive higher order volumes, suggesting stocking decisions are broadly demand-aligned.
- **Delivered** is the most common order status, followed by Pending, Shipped, and Cancelled — indicating a generally healthy fulfillment pipeline with room to reduce pending/cancelled orders.
- Products like **Mouse, Keyboard, and Speaker** carry the highest average stock, while **Scanner and Printer** run leaner — useful for prioritizing restocking attention.

---

## 🚧 Challenges Faced

- Handling missing numeric values without distorting the overall stock/order distribution (solved via mean/median imputation).
- Standardizing inconsistent category naming (`Bangalore` vs `Bengaluru`) before grouping.
- Establishing a reliable ODBC connection to SQL Server from Python (driver setup, trusted authentication).
- Choosing chart types that clearly answer specific business questions rather than just displaying data.

---

## ▶️ How to Run

```bash
pip install pandas matplotlib seaborn pyodbc
```

1. Ensure SQL Server (SSMS) is running with the `Project` database and `Inventory` table available.
2. Update the `SERVER` name in the connection string if needed.
3. Open `Project.ipynb` in Jupyter Notebook and run all cells top to bottom.

---

## 📁 Repository Structure

```
├── Project.ipynb     # Main analysis notebook (DB connection, cleaning, EDA, visuals)
├── inv.csv           # Flattened snapshot of the Inventory table
└── README.md
```

---

## 🔮 Future Scope

- Add automated stockout alerts based on a minimum stock threshold.
- Build an interactive dashboard (e.g., Power BI or Streamlit) on top of the same SQL Server source.
- Expand time-series analysis once more historical data accumulates.

---

## 👤 Author

Developed as part of self-driven Data Analytics portfolio building, combining SQL Server connectivity with Python-based data cleaning and visualization.
