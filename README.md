# BlinkIT Grocery Data Analysis using SQL

## Overview

This project involves a comprehensive analysis of BlinkIT’s grocery sales dataset using SQL.
The goal is to extract business insights from sales, outlet performance, product categories, and customer preferences.
The following README provides objectives, dataset schema, business problems, queries, and key findings.

## Objectives

- Clean and preprocess the dataset.
- Analyze total and average sales performance.
- Understand sales distribution across fat content, item type, outlet size, and location.
- Derive insights into customer preferences and outlet performance.
- Provide a foundation for business decision-making using SQL queries.

Columns:

- Item_Fat_Content – Fat content of the product
- Item_Identifier – Unique product ID
- Item_Type – Type/category of product
- Outlet_Establishment_Year – Year when the outlet was established
- Outlet_Identifier – Unique outlet ID
- Outlet_Location_Type – Location tier (e.g., Tier 1, Tier 2, Tier 3)
- Outlet_Size – Size of the outlet (Small/Medium/High)
- Outlet_Type – Outlet type (Supermarket/Grocery Store)
- Item_Visibility – Percentage visibility of the item in the store
- Item_Weight – Weight of the item
- Total_Sales – Sales amount
- Rating – Customer rating

## Schema
```sql
CREATE TABLE blinkit_data (
    Item_Fat_Content           VARCHAR(50),
    Item_Identifier            VARCHAR(50),
    Item_Type                  VARCHAR(50),
    Outlet_Establishment_Year  INT,
    Outlet_Identifier          VARCHAR(50),
    Outlet_Location_Type       VARCHAR(50),
    Outlet_Size                VARCHAR(50),
    Outlet_Type                VARCHAR(50),
    Item_Visibility            FLOAT,
    Item_Weight                FLOAT,
    Total_Sales                FLOAT,
    Rating                     FLOAT
);
```

### Business Problems & Solutions

### 1. Total Sales

**Objective:** Find the total sales in millions.
```sql
SELECT CAST(SUM(Total_Sales) / 1000000.0 AS DECIMAL(10,2)) AS Total_Sales_Million
FROM blinkit_data;
```

### 2. Average Sales

**Objective:** Calculate average sales per order.
```sql
SELECT CAST(AVG(Total_Sales) AS INT) AS Avg_Sales
FROM blinkit_data;
```

### 3. Number of Orders

**Objective:** Count the number of transactions/orders.
```sql
SELECT COUNT(*) AS No_of_Orders
FROM blinkit_data;
```


### 4. Average Rating

**Objective:** Find the average customer rating.

```sql
SELECT CAST(AVG(Rating) AS DECIMAL(10,1)) AS Avg_Rating
FROM blinkit_data;
```

### 5. Sales by Fat Content

**Objective:** Understand how fat content influences sales.

```sql
SELECT Item_Fat_Content, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Item_Fat_Content;
```

### 6. Sales by Item Type

**Objective:** Identify the highest selling product categories.

```sql
SELECT Item_Type, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Item_Type
ORDER BY Total_Sales DESC;
```

### 7. Fat Content by Outlet (Pivot Analysis)

**Objective:** Compare fat content sales across outlet locations.

```sql
SELECT Outlet_Location_Type,
    COALESCE(SUM(Total_Sales) FILTER (WHERE Item_Fat_Content = 'Low Fat'), 0) AS Low_Fat,
    COALESCE(SUM(Total_Sales) FILTER (WHERE Item_Fat_Content = 'Regular'), 0) AS Regular
FROM blinkit_data
GROUP BY Outlet_Location_Type
ORDER BY Outlet_Location_Type;
```

### 8. Sales by Outlet Establishment Year

**Objective:** Compare fat content sales across outlet locations. Analyze how outlet establishment year affects sales.

```sql
SELECT Outlet_Establishment_Year, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Outlet_Establishment_Year
ORDER BY Outlet_Establishment_Year;
```

### 9. Sales Percentage by Outlet Size

**Objective:** Compare fat content sales across outlet locations. Find which outlet size contributes most to total sales.

```sql
SELECT 
    Outlet_Size, 
    CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales,
    CAST((SUM(Total_Sales) * 100.0 / SUM(SUM(Total_Sales)) OVER()) AS DECIMAL(10,2)) AS Sales_Percentage
FROM blinkit_data
GROUP BY Outlet_Size
ORDER BY Total_Sales DESC;
```

### 10. Sales by Outlet Location

**Objective:** Compare fat content sales across outlet locations. Compare sales across different outlet locations.

```sql
SELECT Outlet_Location_Type, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Outlet_Location_Type
ORDER BY Total_Sales DESC;
```

### 11. All Metrics by Outlet Type

**Objective:** Compare fat content sales across outlet locations. Get a combined view of all metrics by outlet type.

```sql
SELECT Outlet_Type, 
       CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales,
       CAST(AVG(Total_Sales) AS DECIMAL(10,0)) AS Avg_Sales,
       COUNT(*) AS No_Of_Items,
       CAST(AVG(Rating) AS DECIMAL(10,2)) AS Avg_Rating,
       CAST(AVG(Item_Visibility) AS DECIMAL(10,2)) AS Item_Visibility
FROM blinkit_data
GROUP BY Outlet_Type
ORDER BY Total_Sales DESC;
```

## Findings & Conclusion

- **Total Sales:** The dataset highlights millions in grocery sales across different outlets.
- **Product Insights:** Regular-fat items and certain item categories dominate sales.
- **Outlet Insights:** Larger outlets and Tier 1 locations contribute the highest revenue.
- **Customer Behavior:** Ratings remain consistent, but sales vary strongly by outlet type and size.
- **Business Value:** These insights can guide inventory planning, outlet expansion, and product promotion strategies.
