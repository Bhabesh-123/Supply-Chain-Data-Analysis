# Supply-Chain-Data-Analysis
# 🏭 Supply Chain Analytics Project (SQL + Power BI)

This project demonstrates how a data analyst can extract insights from a star-schema based **Supply Chain database** using **SQL Server** and visualize KPIs with **Power BI**.

---

## 🔧 Tools & Technologies
- Microsoft SQL Server (SSMS)
- Power BI
- GitHub
- Star Schema Data Modeling

---

## 🗂 Project Structure

- `SQL/`: Contains SQL scripts for data modeling and analysis.
- `PowerBI/`: Contains .pbix file and screenshots of dashboards.
- `ERD_Diagram/`: Entity Relationship Diagram of the data model.
- `Docs/`: Project summary PDF and supporting documentation.

---

## 🧱 Star Schema Overview

- `fact_sales`: Transactional sales data.

- `dim_products`, `dim_customers`, `dim_dates`, `dim_stores`, etc.: Dimension tables.

![Star Schema](https://github.com/Bhabesh-123/Supply-Chain-Data-Analysis/blob/b85d8ecd14e44da04292ec0d780ef4ea38d872c3/Supply%20chain%20stars%20schema.png)

---

## Some KPI'S
```sql
--Total Revenue
SELECT SUM(total_price) AS Total_Sale FROM fact_sales;

--Total Products Price Sum
SELECT SUM(unit_price)AS Products_Total_Price_Sum FROM dim_products;

--Total Quantity of Products Sold.
SELECT SUM(quantity)AS Total_Quantity FROM fact_sales;
```
---
## Result View



---

## 🔍 Key Analysis Performed

1. **Top-selling products and categories**
2. **Sales by region and store**
3. **Month-over-month sales growth**
4. **Rolling 3-month average revenue**
5. **Customer retention & segmentation**

---

## 📊 Power BI Dashboard

The dashboard covers:
- KPIs: Total Sales, Units Sold, Discounts
- Trendlines for MoM analysis
- Regional Performance
- Customer Analysis

> You can find the `.pbix` file in the `PowerBI/` folder.

---

## 📁 Sample Advanced SQL Queries

```sql
-- Rolling 3-Month Average Sales
SELECT 
    d.year, d.month,
    SUM(fs.total_price) AS sales,
    AVG(SUM(fs.total_price)) OVER (
        ORDER BY d.year, d.month 
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS rolling_avg
FROM fact_sales fs
JOIN dim_dates d ON fs.date_id = d.date_id
GROUP BY d.year, d.month;
