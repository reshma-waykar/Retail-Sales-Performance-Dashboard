# Retail Sales Performance Dashboard

## 1. Project Overview
A mid-sized retail chain with stores across Maharashtra was struggling to identify which product categories and store locations were underperforming. Monthly sales data existed in fragmented Excel sheets — but no consolidated view, no trend analysis, and no actionable reporting. This project simulates the end-to-end data analyst workflow: data collection, cleaning, SQL querying, and dashboard delivery.

## 2. Business Problem
* Sales managers had no single view of store-wise or category-wise performance across months.
* Manual Excel consolidation took 3–4 hours per week, with frequent formula errors.
* No visibility into top-selling vs. slow-moving SKUs, leading to poor inventory decisions.
* No automated alert for months where a store's revenue fell below ₹5 lakh threshold.

## 3. Dataset Description
* **Rows:** 1,200 rows (Simulated 6 months of sales data)
* **Stores:** 5 locations across Maharashtra
* **Categories:** 8 product categories
* **Fields:** Store_ID, Store_Name, City, Month, Category, Product_Name, Units_Sold, Unit_Price, Revenue, Returns, Net_Revenue.

## 4. SQL Queries Used for Analysis

### Query 1: Total Revenue and Units Sold per Store
This query calculates the total gross revenue and total units sold for each store to identify the top-performing locations.
```sql
SELECT 
    Store_Name, 
    City, 
    SUM(Revenue) AS Total_Gross_Revenue, 
    SUM(Units_Sold) AS Total_Units_Sold
FROM Sales_Data
GROUP BY Store_Name, City
ORDER BY Total_Gross_Revenue DESC;
```

### Query 2: Monthly Sales Trend to Identify Low Performing Months
This query analyzes the sales trend month-on-month to find out out which months saw a drop in business.
```SQL
SELECT 
    Month, 
    SUM(Revenue) AS Monthly_Revenue
FROM Sales_Data
GROUP BY Month;
```

### Query 3: Stores Falling Below the ₹5 Lakh Threshold
This query filters out specific stores and months where the total revenue generated was less than ₹5,000,000, alerting management for immediate action.
```SQL
SELECT 
    Store_Name, 
    Month, 
    SUM(Revenue) AS Total_Revenue
FROM Sales_Data
GROUP BY Store_Name, Month
HAVING Total_Revenue < 500000;
```

### Query 4: Top 3 Product Categories by Revenue (Using Window Function)
This query uses a window function (RANK) to rank product categories by sales and filters the top 3 categories driving maximum retail growth.
```SQL
WITH Category_Sales AS (
    SELECT 
        Category, 
        SUM(Revenue) AS Total_Sales,
        RANK() OVER (ORDER BY SUM(Revenue) DESC) as Sales_Rank
    FROM Sales_Data
    GROUP BY Category
)
SELECT Category, Total_Sales
FROM Category_Sales
WHERE Sales_Rank <= 3;
```
