# Sales_Performance_Analysis_Project

## Table of Contents

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Project Steps](#project-steps)
- [Excel: Data Exploration and Analysis](#excel-data-exploration-and-analysis)
- [Microsoft SQL Server: Data Querying and Analysis](#microsoft-sql-server-data-querying-and-analysis)
- [Results and Findings](#results-and-findings)
- [PowerBI: Data Visualization](#powerbi-data-visualization)
- [Limitation](#limitation)
- [Conclusion](#conclusion)


## Project Overview

This data analysis project, conducted as part of LITA capstone project, aims to uncover insights into the sales performance of a retail store over the years 2023 and 2024. By analyzing various aspects of the sales data, I seek to identify trends and evaluate performance across different regions. The goal is to make data-driven recommendations and gain a deeper understanding of the store’s overall performance during this period.
 
## Data Sources
The primary dataset used for this analysis is “sales_data.csv,” which includes detailed information about each sale made by the retail store.

## Tools
- Excel: Data Cleaning and Initial Exploration.
  - Used Excel for initial data exploration, data summarization using pivot tables, and calculations.

- SQL Server: Data Quering and Analytics.
  
- PowerBi: Data Visualization.

## Exploratory Data Analysis
Exploratory Data Analysis (EDA) involved exploring the sales data to answer key questions such as:

  - What are the total sales for each product category?
  -  How many sales transactions occurred in each region?
  -  Which product had the highest total sales value?
  -  What is the total revenue generated per product?
  -  What are the monthly sales totals for the current year?
  -  Who are the top 5 customers by total purchase amount?
  -  What percentage of total sales is contributed by each region?
  - Which products had no sales in the last quarter?
    

 ## Project Steps
 
### Excel: Data Exploration and Analysis

**Overview of Excel Analysis**: 

In this phase of the project, I conducted an initial exploration of the sales data using Excel, focusing on key metrics to gain insights into the retail store's performance. The first step was to derive meaningful insights from the dataset by examining trends over time.

1. **Extracting Month from Dates**: Before creating my pivot tables, I extracted the month from the 'OrderDate' column in the dataset. 

In the first cells under the Months column, enter

``` =MONTH(OrderDate) ```

This allowed me to analyze monthly sales variations, highlighting peak performance periods and potential downturns. It also helped identify seasonal patterns and fluctuations that might not be visible in aggregated data.


2. **Monthly Sales Totals**: With the month data extracted, I analyzed monthly sales totals to understand sales trends over the years. This analysis was crucial for identifying peak sales months.

![Screenshot 2024-11-03 011110](https://github.com/user-attachments/assets/cb3151ff-e253-496c-b20e-3d4ad74385ea)

- **Key Insights**: Sales peaked in February during both 2023 and 2024, likely due to factors such as promotional events or seasonal trends that drive higher consumer activity during this period.
  

3. **Total Sales by Product**: I created a pivot table to analyze total sales across different products, which helped identify which products contributed most to overall sales. Additionally, I created another pivot table to assess the total average sales per product, providing further insights into product performance.

![Screenshot 2024-11-03 010617](https://github.com/user-attachments/assets/dfc285d1-cb38-4dbc-ab77-36d5cd313efb)


4. **Average Total Sales By Product**: 

![Screenshot 2024-11-03 011146](https://github.com/user-attachments/assets/65edb88b-3a1f-4113-9ffa-af8940c7fd64)

- **Key Insights**: Shirts led in average sales (₦326.67), while shoes topped in total sales (₦3,087,500). Socks had the lowest average sales (₦121.67) and the lowest total sales (₦912,500), suggesting potential for strategy improvement to boost their performance.
  

5. **Sales by Region**: Analyzing total sales by region helps identify high-performing regions and those that may need targeted marketing efforts or strategic improvements
  
![Screenshot 2024-11-03 011126](https://github.com/user-attachments/assets/164dd093-faa6-41f5-a96a-9d80c6ac954d)

- **Key Insight**: The South region leads in total sales at ₦4,675,000.00, accounting for 44% of overall sales, indicating a strong market presence compared to the other regions.
  



### Microsoft SQL Server: Data Querying and Analysis

1. **Loading the Dataset**: I first loaded my dataset from Excel into my SQL Server environment to write and validate my queries.

2. **Key Insights Queries**

I wrote several SQL queries to extract key insights from the dataset:

---Retrieve the total sales for each product category:

```
SELECT product, SUM (Sales) AS TotalSales
FROM [Sales_Data]
GROUP BY Product;
```


---Find the number of sales transactions in each region:

```
SELECT Region, COUNT(OrderID) AS TransactionCount
FROM [Sales_Data]
GROUP BY Region;
```


---Find the highest-selling product by total sales value:

```
SELECT TOP 1 Product, SUM(Sales) AS TotalSales
FROM [Sales_Data]
GROUP BY Product
ORDER BY TotalSales DESC;
```


---Calculate total revenue per product:

```
SELECT Product, SUM(Sales) AS TotalRevenue
FROM [Sales_Data]
GROUP BY Product;
```

---Calculate monthly sales totals for the current year (current year is 2024):


```
SELECT DATENAME(MONTH, OrderDate) AS SalesMonth, SUM(Sales) AS MonthlyTotal
FROM [dbo].[Sales_Data]
WHERE YEAR(OrderDate) = 2024
GROUP BY DATENAME(MONTH, OrderDate)
ORDER BY MONTH(OrderDate);
```


---Find the top 5 customers by total purchase amount:

```
SELECT TOP 5 Customer_Id, SUM(Sales) AS TotalPurchaseAmount
FROM [Sales_Data]
GROUP BY Customer_Id
ORDER BY TotalPurchaseAmount DESC;
```


---Calculate the percentage of total sales contributed by each region:

```
WITH TotalSales AS (
    SELECT SUM(Sales) AS TotalSalesValue
    FROM [Sales_Data]
	)
SELECT Region, 
       SUM(Sales) AS RegionalSales,
       (SUM(Sales) / (SELECT TotalSalesValue FROM TotalSales)) * 100 AS SalesPercentage
FROM [Sales_Data]
GROUP BY Region;
```


---Identify products with no sales in the last quarter

```
SELECT DISTINCT Product
FROM [Sales_Data]
WHERE Product NOT IN (
    SELECT Product
    FROM [Sales_Data]
    WHERE OrderDate BETWEEN '2024-10-01' AND '2024-12-31'
);
```

## Results and Findings

The analysis conducted using SQL queries on the dataset yielded the following key insights:

- **Total Sales for Each Product Category**:
  - The analysis revealed that Shoes generated the highest revenue, totaling ₦3,087,500.00 for the years 2023 and 2024. In contrast, Socks contributed the least, with total sales of ₦912,500.00 during the same period.

- **Number of Sales Transactions in Each Region**:
  - The analysis showed that each region—East, North, South, and West—recorded an equal number of sales transactions, with 12,500 transactions per region, resulting in a grand total of 50,000 transactions across all regions.

- **Highest-Selling Product by Total Sales Value**:
  - The analysis identified Shoes as the highest-selling product, generating a total sales value of ₦3,087,500.00. This figure highlights Shoes as a key revenue driver within the product lineup, significantly outperforming other products such as Shirts and Hats, which recorded sales of ₦2,450,000.00 and ₦1,587,500.00, respectively.

- **Total Revenue per Product**:

    The analysis provided a detailed breakdown of total revenue generated by each product. The findings are as follows
    
    

| Product | Total Revenue    |
|---------|------------------|
| Gloves  | ₦1,500,000.00    |
| Hat     | ₦1,587,500.00    |
| Jacket  | ₦1,050,000.00    |
| Shirt   | ₦2,450,000.00    |
| Shoes   | ₦3,087,500.00    |
| Socks   | ₦912,500.00      |
| **Grand Total** | **₦10,587,500.00** |


- **Monthly Sales Totals for the Current Year (2024)**:
  - The analysis revealed that total sales for the year 2024 reached ₦5,012,500.00. February emerged as the peak month, generating ₦1,500,000.00 in sales, while July recorded the lowest at ₦187,500.00. This variation highlights significant seasonal trends in sales performance, suggesting potential areas for strategic focus and improvement throughout the year.


| Sales Month | Monthly Total      |
|-------------|--------------------|
| January     | ₦1,000,000.00      |
| February    | ₦1,500,000.00      |
| March       | ₦275,000.00        |
| April       | ₦200,000.00        |
| May         | ₦225,000.00        |
| June        | ₦750,000.00        |
| July        | ₦187,500.00        |
| August      | ₦875,000.00        |
| **Grand Total** | **₦5,012,500.00** |




- **Top 5 Customers by Total Purchase Amount**:
  - The analysis identified the top five customers contributing to total sales, with IDs Cus1488, Cus1375, Cus1023, Cus1059, and Cus1367, who spent ₦29,340, ₦28,925, ₦28,205, ₦28,005, and ₦27,920 respectively. 

| Customer ID | Total Purchase Amount |
|-------------|-----------------------|
| Cus1488     | ₦29,340               |
| Cus1375     | ₦28,925               |
| Cus1023     | ₦28,205               |
| Cus1059     | ₦28,005               |
| Cus1367     | ₦27,920               |


- **Percentage of Total Sales Contributed by Each Region**:

  - The analysis revealed the distribution of total sales across different regions, highlighting their respective contributions. The South region led with 44.16% of total sales, indicating a strong market presence. The East region contributed 23.14%, while the North and West regions accounted for 18.42% and 14.29%, respectively. This distribution underscores the South's dominance in sales performance, while also suggesting opportunities for growth in the North and West regions.

| Region | Percentage of Total Sales |
|--------|---------------------------|
| South  | 44.16%                    |
| East   | 23.14%                    |
| North  | 18.42%                    |
| West   | 14.29%                    |

- Products with No Sales in the Last Quarter:
  - The analysis indicated that there are no recorded sales for the last quarter of 2024 (October to December), as no data entries exist for this period.


## PowerBI: Data Visualization 
- Data Loading: The sales dataset was imported into Power BI from an Excel file. Upon loading, I performed data cleaning using Power Query, which included:
   - Converting date formats for consistency.
   - Ensuring data types were correctly assigned for analysis.
    
- Visualizations Created


![Screenshot 2024-10-29 122105](https://github.com/user-attachments/assets/39cd5998-19d4-41ed-aba9-b46060928543)


This visualization effectively addresses our insights and exploratory data analysis (EDA) questions, providing a clear representation of the key findings derived from the sales data.


## Limitation
- Incomplete Data: The dataset lacks entries for the last quarter of 2024 (October to December), as this period is still ongoing. This absence limits the ability to analyze complete sales trends and performance for the entire year.



## Conclusion

- The analysis of the sales data provided valuable insights into product performance, customer behavior, and regional sales distributions for 2024. Key findings include:
  - Total Sales Analysis: Shirts emerged as the top-performing product, while the South region led in overall sales contribution.
  - Sales Transactions: Consistent sales were recorded across all regions, demonstrating a strong market presence.
  - Customer Insights: The identification of the top customers highlights the importance of understanding consumer behavior for targeted marketing efforts.

Despite limitations such as incomplete data for the last quarter, the insights gained can guide future strategies in inventory management, sales forecasting, and customer engagement. 
