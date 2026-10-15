# Inventory Data Cleaning & Visualization with Python

A data analytics project that connects to Microsoft SQL Server, loads inventory records into Pandas, cleans and transforms the data, and visualizes order and warehouse patterns.

## Project Objective

Explore inventory and order data to answer questions such as:

- Which warehouses handle the largest order volumes?
- How do stock levels compare with order demand?
- Which product categories may need closer inventory attention?
- How are orders distributed across warehouse locations and fulfillment statuses?

## Technology Stack

| Technology | Use |
|---|---|
| Python | Analysis workflow |
| Pandas | Data extraction, cleaning, and transformation |
| pyodbc | SQL Server connectivity |
| Microsoft SQL Server / SSMS | Data storage and querying |
| Matplotlib | Plot construction and formatting |
| Seaborn | Statistical and comparative visualizations |
| Jupyter Notebook | Interactive analysis |

## Data

The inventory table contains **120 order records across 7 columns**: `Order_ID`, `Date_order`, `Product_Name`, `Stock`, `Orders`, `Status_Order`, and `Warehouse`.

The repository also includes `inv.csv`, a portable snapshot of the data for review and adaptation.

## Data Quality & Preparation

The notebook demonstrates steps such as:

- Inspecting data types and missing values
- Handling missing `Stock` and `Orders` values
- Checking for duplicate rows
- Converting order dates to datetime values
- Standardizing warehouse labels
- Reviewing descriptive statistics before visualization

In the included dataset, the initial checks identified 7 missing `Stock` values, 6 missing `Orders` values, and no duplicate rows. The cleaning choices are documented in the notebook and should be assessed in the context of the analysis.

## Visual Analysis

The notebook includes visualizations for:

1. Total orders by warehouse
2. Relationship between stock and orders
3. Stock/order comparisons grouped by warehouse
4. Share of order volume by warehouse

These visuals are intended to connect the data preparation process to operational questions, rather than display charts without context.

## How to Run

### Option A — Connect to SQL Server

1. Install Python and Jupyter Notebook.
2. Install the required Python packages:

   ```bash
   pip install pandas matplotlib seaborn pyodbc
   ```

3. Ensure SQL Server contains an `Inventory` table with the expected columns.
4. Open `Project.ipynb`.
5. Update the notebook's SQL Server connection settings for your own environment (ODBC driver, server, database, and authentication).
6. Run the notebook cells in order.

The notebook's original database connection is environment-specific. Do not expect its server/database settings to work on another machine without changes.

### Option B — Use the Included CSV Snapshot

If you do not have SQL Server configured, use the included `inv.csv` file. In the notebook's data-loading section, replace the database-loading step with:

```python
import pandas as pd

df = pd.read_csv("inv.csv")
```

Then continue with the cleaning and visualization steps, checking that the column names match the notebook.

## Repository Structure

```text
.
├── Project.ipynb   # SQL extraction, cleaning, analysis, and visualizations
├── inv.csv         # Portable snapshot of the inventory records
└── README.md
```

## Portfolio Summary

This project demonstrates a practical path from data extraction to cleaned data, exploratory analysis, and visualization. It is relevant to entry-level Data Analyst, Python Data Analyst, Reporting Analyst, and Data Quality Analyst applications.

## Author

**Yash Takalkar**  
[LinkedIn](https://www.linkedin.com/in/yash-takalkar-5a364a284/) · [GitHub](https://github.com/Yash-0608)
