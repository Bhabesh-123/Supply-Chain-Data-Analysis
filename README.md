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

## 📁 CSV Files
*click the Link to see the dataset Files*
-[DATASET](https://github.com/Bhabesh-123/Supply-Chain-Data-Analysis/tree/4e8b133b2f8e6b7c35d7c83f9922bc64d0e57469/Csv%20Files)

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
3. **Sales by store**
*Total Revenue From Stores* 
```sql
SELECT SUM(Store_Wise_Total_Revenue ) AS Stores_total_Revenue_
FROM 
(SELECT
    s.store_name,
    Sum(fs.total_price) AS Store_Wise_Total_Revenue 
FROM Fact_sales fs
JOIN dim_stores s ON fs.store_id = s.store_id
GROUP BY s.store_name)
AS Store_Revenue
```
![Revenue Contribtion By Stores](https://github.com/Bhabesh-123/Supply-Chain-Data-Analysis/blob/e92891c2e10bd78a3ea2ef01d4c438bc3ba27df7/Stores%20Total%20Revenue.png)
---
*TOP 10 Stores IndiVidual ReveNue Contribution* 
```sql
SELECT TOP 10
    s.store_name,
    Sum(fs.total_price) AS Store_Wise_Total_Revenue 
FROM Fact_sales fs
JOIN dim_stores s ON fs.store_id = s.store_id
GROUP BY s.store_name
ORDER BY Store_Wise_Total_Revenue DESC;
```
![TOP 10 Stores Revenue](https://github.com/Bhabesh-123/Supply-Chain-Data-Analysis/blob/cbafe8a5c360e81176c09bbc933b18dcfb028fd0/Store%20wise%20Revenue.png)

---
*POWER BI RESULT*

![PBI result of Store wise revenue](https://github.com/Bhabesh-123/Supply-Chain-Data-Analysis/blob/9e9e50e44c8e0d4f0c94701352a389be251d9bd0/Power%20Bi%20%20Total%20Store%20wise%20Revenue%20with%20Grand%20Revenue%20Contribution.png)

4. **Total Sales Per Month**
 ```sql
SELECT 
    d.year,
    d.month,
    SUM(fs.total_price) AS monthly_sales
FROM fact_sales fs
JOIN dim_dates d ON fs.date_id = d.date_id
GROUP BY d.year, d.month
ORDER BY d.year, d.month;
```
![Sql Result Monthwise Sales](https://github.com/Bhabesh-123/Supply-Chain-Data-Analysis/blob/e4f62616ac865e59db12b06b5f06b090e7e577ed/Total%20sales%20Per%20Month.png)

* To View the SUM(Full Sales of Months) in SQL*
```sql
   SELECT  SUM(monthly_sales) AS Month_Wise_Total_Sales 
FROM
(SELECT 
    d.year,
    d.month,
    SUM(fs.total_price) AS monthly_sales
FROM fact_sales fs
JOIN dim_dates d ON fs.date_id = d.date_id
GROUP BY d.year, d.month
)AS
Total_Month_sales
```
* PBI_RESULT *
  
![Pbi](https://github.com/Bhabesh-123/Supply-Chain-Data-Analysis/blob/e4f62616ac865e59db12b06b5f06b090e7e577ed/Month%20Wise%20Sales%20.png)
---

## 📊 Power BI Dashboard

The dashboard covers:
- KPIs: Total Sales, Units Sold, Discounts
- Trendlines for MoM analysis
- Regional Performance
- Customer Analysis

> You can find the `.pbix` file in the `PowerBI/` folder.

---

## Advance Analysis
1. **Repeat Customer ID**

*Try this to View the Repeated Customer*

```sql
SELECT 
    fs.customer_id,
    COUNT(DISTINCT d.month + d.year * 100) AS active_months
FROM fact_sales fs
JOIN dim_dates d ON fs.date_id = d.date_id
GROUP BY fs.customer_id
HAVING COUNT(DISTINCT d.month + d.year * 100) > 1;
```
---
2. **Repeat customer Vs One-Time Customer**

   *WE will visualize This one On PBI*

```sql
WITH customer_months AS (
    SELECT 
        fs.customer_id,
        COUNT(DISTINCT d.month + d.year * 100) AS active_months
    FROM fact_sales fs
    JOIN dim_dates d ON fs.date_id = d.date_id
    GROUP BY fs.customer_id
)
SELECT 
    CASE 
        WHEN active_months > 11 THEN 'Repeat Customer'
        ELSE 'One-Time Customer'
    END AS customer_type,
    COUNT(*) AS customer_count
FROM customer_months
GROUP BY 
    CASE 
        WHEN active_months > 11 THEN 'Repeat Customer'
        ELSE 'One-Time Customer'
    END;
```
![Repeat Vs one Time Customer](https://github.com/Bhabesh-123/Supply-Chain-Data-Analysis/blob/046058a0ae5d12d03790341fa41f9babd98cd950/Repeat%20Customer%20Vs%20One-TimeCustomer%20.png)

---
**PBI Visual**

Repeat vs One-Time Customers

🔨 Steps:

Create a calculated column in Power BI on fact_sales:

**DAX**

Purchase_Month = FORMAT(dim_dates[date], "YYYY-MM")


*Create a summary table:*

**DAX**

CustomerMonthCount = 
SUMMARIZE(
    fact_sales,
    fact_sales[customer_id],
    "MonthsPurchased", DISTINCTCOUNT(fact_sales[Purchase_Month])
)
Create a calculated column on that table:

**DAX**

CustomerType = 
IF([MonthsPurchased] > 1, "Repeat Customer", "One-Time Customer")

---
**Create a Donut Chart or Bar Chart:**

Axis: CustomerType

Values: Count of customer_id

✔️ This will now visually match the SQL output.

![PBI VISUAL](https://github.com/Bhabesh-123/Supply-Chain-Data-Analysis/blob/c5be3bc239b948a5d62061c3150efde7cb91a2db/PBI%20Repeat%20Vs%20One%20Time%20Customers.png)

   
**✅ Create Views (Reusable Layers for BI Tools)**
```sql
CREATE VIEW vw_monthly_category_sales AS
SELECT 
    d.year,
    d.month,
    c.category_name,
    SUM(fs.total_price) AS monthly_sales
FROM fact_sales fs
JOIN dim_dates d ON fs.date_id = d.date_id
JOIN dim_products p ON fs.product_id = p.product_id
JOIN dim_categories c ON p.category_id = c.category_id
GROUP BY d.year, d.month, c.category_name;
```

Then Select This Line and Execute-To View the Result .
```sql
SELECT * FROM vw_monthly_category_sales;
 ```

![]()
---

**📈 Year-over-Year Revenue Growth**
```sql
CREATE VIEW vw_yearly_revenue AS
SELECT 
    d.year,
    SUM(fs.total_price) AS total_revenue
FROM fact_sales fs
JOIN dim_dates d ON fs.date_id = d.date_id
GROUP BY d.year;
```
Then Select This Line and Execute-To View the Result .
```sql
SELECT * FROM vw_yearly_revenue
```

![]()
---

**To Calculate Growth Year By Year In percentage(Previous Year To Current Year**
```sql
SELECT 
    curr.year,
    curr.total_revenue,
    prev.total_revenue AS previous_year_revenue,
    ROUND(((curr.total_revenue - prev.total_revenue) * 100.0) / NULLIF(prev.total_revenue, 0), 2) AS growth_percentage
FROM vw_yearly_revenue curr
LEFT JOIN vw_yearly_revenue prev ON curr.year = prev.year + 1
ORDER BY curr.year;
```
![]()
---
   
1. **Month-over-month sales growth**
2. **Rolling 3-month average revenue**
3. **Customer retention & segmentation**

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
