# Sales_data
This project demonstrates an end-to-end data analytics pipeline built using Snowflake, Python (pandas), and Power BI. It focuses on cleaning, transforming, and visualizing sales data to generate actionable business insights.
# 🧩 Sales Data Analysis & Visualization Pipeline (Snowflake + Python + Power BI)

## 📖 Project Overview
This project demonstrates an **end-to-end data analytics pipeline** using **Snowflake**, **Python (Jupyter Notebook)**, and **Power BI**.  
It converts raw sales data into meaningful business insights by creating a **data warehouse model**, performing **data cleaning & transformation**, and building **interactive dashboards**.

---

## 🎯 Problem Statement
A retail company wants to analyze its **sales performance** across multiple dimensions such as products, customers, and time.  
However, the data is **unstructured, inconsistent, and scattered** across different sources, making it hard to extract insights.

The objective is to design a **data warehouse (Star Schema)** in **Snowflake**, clean and merge the data using **Python**, and visualize the final insights using **Power BI** dashboards.

---

## 🧱 Data Warehouse Design (Star Schema)
| Table Type | Table Name | Description |
|-------------|-------------|--------------|
| 🟩 Fact Table | `fact_sales` | Contains sales transactions with keys linking to dimension tables |
| 🟦 Dimension Table | `dim_customer` | Customer details such as name, location, and country |
| 🟧 Dimension Table | `dim_product` | Product details including category, sub-category, and price |
| 🟨 Dimension Table | `dim_date` | Date attributes like day, month, quarter, and year |

---

## 🧠 Key Objectives
✅ Create normalized data tables in **Snowflake** following the Star Schema model.  
✅ Clean and preprocess data using **Python (pandas)**.  
✅ Merge data to form a unified analytical dataset.  
✅ Visualize important KPIs in **Python** and **Power BI**.  
✅ Export the cleaned dataset for further business reporting.

---

## 🛠️ Tools & Technologies
| Category | Tool |
|-----------|------|
| Database | **Snowflake** |
| Data Cleaning | **Python (pandas, numpy)** |
| Visualization | **Matplotlib, Seaborn, Power BI** |
| Database Connection | **Snowflake Connector for Python** |
| IDE | **Jupyter Notebook (Anaconda)** |

---

## ⚙️ Workflow Steps

### 1️⃣ Data Extraction
```python
import snowflake.connector
import pandas as pd

conn = snowflake.connector.connect(
    user='YOUR_USERNAME',
    password='YOUR_PASSWORD',
    account='YOUR_ACCOUNT',
    warehouse='YOUR_WAREHOUSE',
    database='YOUR_DATABASE',
    schema='YOUR_SCHEMA'
)

fact_sales = conn.cursor().execute("SELECT * FROM fact_sales").fetch_pandas_all()
dim_customer = conn.cursor().execute("SELECT * FROM dim_customer").fetch_pandas_all()
dim_product = conn.cursor().execute("SELECT * FROM dim_product").fetch_pandas_all()
dim_date = conn.cursor().execute("SELECT * FROM dim_date").fetch_pandas_all()


---


### 2️⃣ Data Cleaning & Transformation
```python

# Handling missing values
fact_sales.fillna(0, inplace=True)
dim_customer.fillna('Unknown', inplace=True)
dim_product.fillna('Not Available', inplace=True)

# Convert date columns to datetime
dim_date['date'] = pd.to_datetime(dim_date['date'], errors='coerce')

# Merge tables to create analytical dataset
merged_df = fact_sales.merge(dim_customer, on='customer_id', how='left') \
                      .merge(dim_product, on='product_id', how='left') \
                      .merge(dim_date, on='date_id', how='left')

----


###3️⃣ Data Visualization in Python
```python

import matplotlib.pyplot as plt
import seaborn as sns

plt.figure(figsize=(10,6))
sns.barplot(data=merged_df, x='product_line', y='sales_amount')
plt.title('Total Sales by Product Line')
plt.show()

----

###4️⃣ Export Data for Power BI
```python

merged_df.to_csv("cleaned_sales_data.csv", index=False)
print("✅ Cleaned data exported successfully!")
