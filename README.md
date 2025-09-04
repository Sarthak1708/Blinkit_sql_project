# BLINKIT SQL PROJECT
BlinkIT Grocery Data Analysis using SQL

📌 Overview

This project involves a comprehensive analysis of BlinkIT’s grocery sales dataset using SQL.
The goal is to extract business insights from sales, outlet performance, product categories, and customer preferences.
The following README provides objectives, dataset schema, business problems, queries, and key findings.

🎯 Objectives

Clean and preprocess the dataset.

Analyze total and average sales performance.

Understand sales distribution across fat content, item type, outlet size, and location.

Derive insights into customer preferences and outlet performance.

Provide a foundation for business decision-making using SQL queries.

📊 Dataset

The dataset contains BlinkIT grocery sales data with product and outlet details.

Columns:

Item_Fat_Content – Fat content of the product

Item_Identifier – Unique product ID

Item_Type – Type/category of product

Outlet_Establishment_Year – Year when the outlet was established

Outlet_Identifier – Unique outlet ID

Outlet_Location_Type – Location tier (e.g., Tier 1, Tier 2, Tier 3)

Outlet_Size – Size of the outlet (Small/Medium/High)

Outlet_Type – Outlet type (Supermarket/Grocery Store)

Item_Visibility – Percentage visibility of the item in the store

Item_Weight – Weight of the item

Total_Sales – Sales amount

Rating – Customer rating

🗄️ Schema
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

🧩 Business Problems & Solutions
1. Total Sales
SELECT CAST(SUM(Total_Sales) / 1000000.0 AS DECIMAL(10,2)) AS Total_Sales_Million
FROM blinkit_data;


📌 Objective: Find the total sales in millions.

2. Average Sales
SELECT CAST(AVG(Total_Sales) AS INT) AS Avg_Sales
FROM blinkit_data;


📌 Objective: Calculate average sales per order.

3. Number of Orders
SELECT COUNT(*) AS No_of_Orders
FROM blinkit_data;


📌 Objective: Count the number of transactions/orders.

4. Average Rating
SELECT CAST(AVG(Rating) AS DECIMAL(10,1)) AS Avg_Rating
FROM blinkit_data;


📌 Objective: Find the average customer rating.

5. Sales by Fat Content
SELECT Item_Fat_Content, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Item_Fat_Content;


📌 Objective: Understand how fat content influences sales.

6. Sales by Item Type
SELECT Item_Type, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Item_Type
ORDER BY Total_Sales DESC;


📌 Objective: Identify the highest selling product categories.

7. Fat Content by Outlet (Pivot Analysis)
SELECT Outlet_Location_Type,
    COALESCE(SUM(Total_Sales) FILTER (WHERE Item_Fat_Content = 'Low Fat'), 0) AS Low_Fat,
    COALESCE(SUM(Total_Sales) FILTER (WHERE Item_Fat_Content = 'Regular'), 0) AS Regular
FROM blinkit_data
GROUP BY Outlet_Location_Type
ORDER BY Outlet_Location_Type;


📌 Objective: Compare fat content sales across outlet locations.

8. Sales by Outlet Establishment Year
SELECT Outlet_Establishment_Year, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Outlet_Establishment_Year
ORDER BY Outlet_Establishment_Year;


📌 Objective: Analyze how outlet establishment year affects sales.

9. Sales Percentage by Outlet Size
SELECT 
    Outlet_Size, 
    CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales,
    CAST((SUM(Total_Sales) * 100.0 / SUM(SUM(Total_Sales)) OVER()) AS DECIMAL(10,2)) AS Sales_Percentage
FROM blinkit_data
GROUP BY Outlet_Size
ORDER BY Total_Sales DESC;


📌 Objective: Find which outlet size contributes most to total sales.

10. Sales by Outlet Location
SELECT Outlet_Location_Type, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Outlet_Location_Type
ORDER BY Total_Sales DESC;


📌 Objective: Compare sales across different outlet locations.

11. All Metrics by Outlet Type
SELECT Outlet_Type, 
       CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales,
       CAST(AVG(Total_Sales) AS DECIMAL(10,0)) AS Avg_Sales,
       COUNT(*) AS No_Of_Items,
       CAST(AVG(Rating) AS DECIMAL(10,2)) AS Avg_Rating,
       CAST(AVG(Item_Visibility) AS DECIMAL(10,2)) AS Item_Visibility
FROM blinkit_data
GROUP BY Outlet_Type
ORDER BY Total_Sales DESC;


📌 Objective: Get a combined view of all metrics by outlet type.

🔍 Findings & Conclusion

Total Sales: The dataset highlights millions in grocery sales across different outlets.

Product Insights: Regular-fat items and certain item categories dominate sales.

Outlet Insights: Larger outlets and Tier 1 locations contribute the highest revenue.

Customer Behavior: Ratings remain consistent, but sales vary strongly by outlet type and size.

Business Value: These insights can guide inventory planning, outlet expansion, and product promotion strategies.
