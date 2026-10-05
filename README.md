# Vendor Performance & Inventory Analysis

## 📊 Project Overview

This project performs **Exploratory Data Analysis (EDA) on vendor, purchasing, sales, pricing and inventory data** to understand business performance and prepare the data for efficient reporting and dashboard development.

The analysis combines multiple relational tables stored in a **SQLite database** and creates an aggregated `Vendor_sales_summary` table containing vendor and product-level metrics.

The project focuses primarily on two business objectives:

* **Vendor Selection for Profitability**
* **Product Pricing Optimization**

The analysis also prepares a structured dataset that can be used for future **business intelligence dashboards and reporting**.

---

## 🎯 Business Objectives

The project aims to answer business-oriented questions such as:

* Which vendors and products generate stronger financial performance?
* How much is being spent on purchasing from each vendor?
* How much revenue is generated from those purchases?
* What is the relationship between purchase price and selling price?
* How much freight cost is associated with each vendor?
* Which products have stronger sales performance?
* How efficiently is inventory being converted into sales?
* Which vendors/products may require further investigation for pricing or profitability optimization?

---

## 🗄️ Dataset & Database

The project uses an `inventory.db` SQLite database containing multiple related tables.

### Database Tables

| Table                  | Description                                                                     |
| ---------------------- | ------------------------------------------------------------------------------- |
| `begin_inventory`      | Beginning inventory information for stores and products                         |
| `end_inventory`        | Ending inventory information for stores and products                            |
| `purchases`            | Vendor purchase transactions, quantities, purchase prices, and purchase amounts |
| `purchase_prices`      | Product-level pricing, volume, purchase price, and vendor information           |
| `sales`                | Product sales transactions, quantities, prices, revenue, and excise tax         |
| `vendor_invoice`       | Vendor invoice information including purchase amounts and freight costs         |
| `Vendor_sales_summary` | Aggregated vendor/product-level analytical table created during the project     |

### Dataset Scale

The database contains millions of transactional records. For example:

* **Purchases:** 2.37M+ records
* **Sales:** 12.8M+ records
* **Purchase Prices:** 12K+ records
* **Vendor Invoices:** 5K+ records
* **Beginning Inventory:** 206K+ records
* **Ending Inventory:** 224K+ records
* **Aggregated Vendor/Product Summary:** 10,692 records

This scale makes SQL-based aggregation important for efficient analysis.

---

## 🛠️ Tools & Technologies

* **Python**
* **Pandas**
* **SQLite**
* **SQL**
* **Jupyter Notebook**

### Python Libraries

```text
pandas
sqlite3
time
```

---

## 🔄 Analysis Workflow

The project follows a structured data analytics workflow:

```text
Raw Database
     │
     ▼
Explore Database Tables
     │
     ▼
Inspect Data Structure & Record Counts
     │
     ▼
Analyze Purchases
     │
     ▼
Analyze Sales
     │
     ▼
Analyze Vendor Freight Costs
     │
     ▼
Join & Aggregate Data
     │
     ▼
Create Vendor/Product Summary
     │
     ▼
Data Cleaning
     │
     ▼
Calculate Business Metrics
     │
     ▼
Store Aggregated Results
     │
     ▼
Prepare Data for Reporting & Dashboards
```

---

## 🔍 Exploratory Data Analysis

The analysis begins by connecting to the SQLite database and inspecting the available tables.

```python
conn = sqlite3.connect('inventory.db')
```

The project then examines:

* Available database tables
* Number of records in each table
* Sample records
* Column structures
* Vendor-level purchasing information
* Vendor-level sales information
* Product pricing
* Freight costs
* Missing values
* Data types
* Vendor naming consistency

---

## 🧩 Data Integration

The required information is distributed across multiple database tables.

To support business analysis, the project combines:

* Purchase transactions
* Sales transactions
* Product pricing
* Vendor information
* Freight costs

A series of SQL aggregations and joins are used to construct a consolidated vendor/product-level dataset.

### Main aggregated dataset

The resulting `Vendor_sales_summary` contains **10,692 records and 14 initial columns**, which are subsequently extended with additional analytical metrics.

---

## 📈 Key Metrics

Several business metrics are calculated to evaluate vendor and product performance.

### Gross Profit

```text
Gross Profit = Total Sales Dollars - Total Purchase Dollars
```

This estimates the difference between revenue generated and purchase expenditure.

### Profit Margin

```text
Profit Margin = Gross Profit / Total Sales Dollars × 100
```

This measures profitability relative to sales revenue.

### Stock Turnover

```text
Stock Turnover = Total Sales Quantity / Total Purchase Quantity
```

This provides an indication of how effectively purchased inventory is converted into sales.

### Sales-to-Purchase Ratio

```text
Sales-to-Purchase Ratio =
Total Sales Dollars / Total Purchase Dollars
```

This compares generated sales revenue against purchasing expenditure.

### Freight Cost

Freight expenses are aggregated at the vendor level and incorporated into the analytical dataset.

---

## 🧹 Data Cleaning

The project performs several data preparation steps before calculating the final metrics.

### Missing Values

Missing sales-related values are identified and handled:

```python
Vendor_sales_summary.fillna(0, inplace=True)
```

### Vendor Name Standardization

Vendor names are cleaned by removing unnecessary whitespace:

```python
Vendor_sales_summary['VendorName'] = \
    Vendor_sales_summary['VendorName'].str.strip()
```

### Data Type Conversion

The `Volume` field is converted into a numeric format to support analytical operations:

```python
Vendor_sales_summary['Volume'] = \
    Vendor_sales_summary['Volume'].astype('float64')
```

---

## ⚡ Performance Optimization

One of the important aspects of this project is reducing the need to repeatedly perform expensive joins and aggregations on large transactional tables.

Instead of repeatedly querying millions of sales and purchase records, the project creates a pre-aggregated:

```text
Vendor_sales_summary
```

This summary table contains vendor/product-level information that can be directly consumed by future dashboards and reports.

### Benefits

* Faster analytical queries
* Reduced computational overhead
* Easier vendor comparison
* Simplified reporting
* Better foundation for BI dashboards
* Easier calculation of profitability and pricing metrics

---

## 📋 Final Analytical Dataset

The final dataset contains metrics such as:

| Metric                  | Description                                 |
| ----------------------- | ------------------------------------------- |
| `VendorNumber`          | Unique vendor identifier                    |
| `VendorName`            | Vendor name                                 |
| `Brand`                 | Product brand identifier                    |
| `Description`           | Product description                         |
| `PurchasePrice`         | Product purchase price                      |
| `ActualPrice`           | Product selling/actual price                |
| `Volume`                | Product volume                              |
| `TotalPurchaseQuantity` | Total quantity purchased                    |
| `TotalPurchaseDollars`  | Total purchase expenditure                  |
| `TotalSalesQuantity`    | Total quantity sold                         |
| `TotalSalesDollars`     | Total sales revenue                         |
| `TotalSalesPrice`       | Aggregated sales price                      |
| `TotalExciseTax`        | Total excise tax                            |
| `FreightCost`           | Vendor freight cost                         |
| `GrossProfit`           | Estimated gross profit                      |
| `ProfitMargin`          | Estimated profit margin                     |
| `StockTurnover`         | Sales-to-purchase quantity ratio            |
| `SalestoPurchaseRatio`  | Sales revenue to purchase expenditure ratio |

---

## 💡 Business Applications

The resulting dataset can support several business decisions.

### Vendor Selection

Vendor-level profitability and sales performance can help identify vendors that may provide stronger commercial value.

### Product Pricing Optimization

Comparing purchase prices with actual selling prices can help identify products with different pricing and margin characteristics.

### Inventory Management

Stock turnover and purchase/sales quantities can provide insight into inventory movement.

### Freight Cost Analysis

Vendor-level freight costs can be incorporated into profitability analysis and vendor evaluation.

### Business Intelligence

The aggregated dataset provides a cleaner source for future dashboards and recurring reporting.

---

## 📁 Project Structure

A recommended GitHub structure is:

```text
Vendor-Performance-Analysis/
│
├── Exploratory_Analysis.ipynb
├── inventory.db
├── README.md
│
├── data/
│   └── ...
│
└── images/
    └── ...
```

> If `inventory.db` or the raw dataset is too large to upload to GitHub, keep the database out of the repository and provide instructions for obtaining or generating it.

---

## 🚀 How to Run

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd Vendor-Performance-Analysis
```

### 2. Install dependencies

```bash
pip install pandas jupyter
```

`sqlite3` is included with standard Python installations.

### 3. Start Jupyter Notebook

```bash
jupyter notebook
```

### 4. Open the notebook

Open:

```text
Exploratory_Analysis.ipynb
```

### 5. Run the notebook

Make sure the SQLite database is available at the expected location:

```text
inventory.db
```

The notebook will then connect to the database and perform the exploratory analysis and aggregation workflow.

---

## 👨‍💻 Project Purpose

This project demonstrates practical skills in:

* Exploratory Data Analysis
* SQL data aggregation
* Relational data analysis
* Data cleaning
* Business metric development
* Vendor performance analysis
* Profitability analysis
* Pricing analysis
* Inventory analytics
* Data preparation for BI dashboards

It was developed as a portfolio project to demonstrate how **SQL and Python can be used to transform large transactional datasets into business-ready analytical information**.
