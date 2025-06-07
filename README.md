# Supply-Chain-Data-Analysis
![Project Workflow](https://github.com/Bhabesh-123/Supply-Chain-Data-Analysis/blob/0753697213dfcddb7fb6e91aaeff625a332d851d/%F0%9F%93%A6%20Project%20Title_%20Supply%20Chain%20Pipeline%20Dashboard%20-%20visual%20selection.png)

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
## Result 
![Power Bi](https://github.com/Bhabesh-123/Supply-Chain-Data-Analysis/blob/173d5a6d71d0002036908edd3dc6848ecbc29bc2/Sale_Quantity%20sold%20And%20Product%20total%20Price.png)
![Sql View](https://github.com/Bhabesh-123/Supply-Chain-Data-Analysis/blob/511f89f4439b3f498dc3361c50d667ea96643f6b/Power%20Bi%20First%20KPI.png)


---

## 🔍 Key Analysis Performed

1. **Top-selling products and categories**
```sql
--top 10 products by revenue:

SELECT TOP 10
    p.product_name,
    SUM(fs.total_price) AS total_revenue
FROM fact_sales fs
JOIN dim_products p ON fs.product_id = p.product_id
GROUP BY p.product_name
ORDER BY total_revenue DESC;
```
## Sql Result Image
![Top 10 Products](https://github.com/Bhabesh-123/Supply-Chain-Data-Analysis/blob/f3f50c73cef7255aaf6d69904beb6723de01ae0b/Top%2010%20Products%20By%20Revenue.png)
![Top 10 Ctegories](https://github.com/Bhabesh-123/Supply-Chain-Data-Analysis/blob/83de22192343389c7501716e488d5d6579533d30/Top%2010%20Categories%20By%20Revenue.png)
## Power Bi Result Image
![Top Ctegories and Products](https://github.com/Bhabesh-123/Supply-Chain-Data-Analysis/blob/f3f50c73cef7255aaf6d69904beb6723de01ae0b/PowerBi%20%20Top%2010%20product%20and%20category%20by%20Revenue%20.png)

---   
3. **Sales by region and store**
4. **Month-over-month sales growth**
5. **Rolling 3-month average revenue**
6. **Customer retention & segmentation**

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
