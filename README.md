# Retail-Sales-Analysis-using-SQL
This project involves data cleaning, exploration, and in-depth analysis of a retail store’s sales dataset using SQL. The analysis uncovers customer trends, category-wise performance, peak sales periods, and customer segmentation based on gender and purchase history. It demonstrates real-world use of SQL for extracting insights from structured data.

---

## Objectives
### 1. Set up a retail sales database:
  Create and populate a retail sales database with the provided sales data.
### 2. Data Cleaning: 
  Identify and remove any records with missing or null values.
### 3. Exploratory Data Analysis (EDA): 
  Perform basic exploratory data analysis to understand the dataset.
### 4. Business Analysis: 
  Use SQL to answer specific business questions and derive insights from the sales data.

## 📁 Project Structure

---

### 1. **Database Setup**

- **Database Creation**: The project begins by creating a database named `retail_db`.
- **Table Creation**: A table named `retail_sales` is created to store transaction-level sales data with columns such as transaction ID, date, time, customer details, product category, quantity, price, and total sale metrics.

```sql
CREATE DATABASE retail_db;
USE retail_db;

CREATE TABLE retail_sales(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(35),
    quantiy INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```

---

### 2. **Data Exploration & Cleaning**

- **Initial Data Check**: View sample records and check total entries.
- **Null Handling**: Identified and deleted records with null values across critical columns.
- **Key Insights**: Unique customer count, unique categories, and total sales.

```sql
-- Sample data and count
SELECT * FROM retail_sales ORDER BY transactions_id ASC LIMIT 10;
SELECT COUNT(*) FROM retail_sales;

-- Null check
SELECT * FROM retail_sales
WHERE transactions_id IS NULL
   OR sale_date IS NULL
   OR sale_time IS NULL
   OR customer_id IS NULL
   OR gender IS NULL
   OR age IS NULL
   OR category IS NULL
   OR quantiy IS NULL
   OR price_per_unit IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;

-- Null deletion
DELETE FROM retail_sales
WHERE transactions_id IS NULL
   OR sale_date IS NULL
   OR sale_time IS NULL
   OR customer_id IS NULL
   OR gender IS NULL
   OR age IS NULL
   OR category IS NULL
   OR quantiy IS NULL
   OR price_per_unit IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;
```

---

### 3. **Data Analysis & Insights**

A series of SQL queries were written to answer business-related questions:

#### 🔹 Sales on a Specific Date (2022-11-05)
```sql
SELECT * 
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

#### 🔹 Clothing Transactions (Qty > 4 in Nov-2022)
```sql
SELECT * 
FROM retail_sales
WHERE category = 'Clothing' 
  AND quantiy > 4 
  AND DATE_FORMAT(sale_date,'%Y-%M') = '2022-11';
```

#### 🔹 Total Sales per Category
```sql
SELECT category, SUM(total_sale) AS TOTAL_SALES
FROM retail_sales
GROUP BY category;
```

#### 🔹 Average Age of Beauty Category Buyers
```sql
SELECT ROUND(AVG(age),2) as Avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

#### 🔹 High-Value Transactions (Sale > 1000)
```sql
SELECT * 
FROM retail_sales
WHERE total_sale > 1000;
```

#### 🔹 Transactions by Gender and Category
```sql
SELECT category, gender, COUNT(*) as total_transactions
FROM retail_sales 
GROUP BY category, gender;
```

#### 🔹 Monthly Average Sale and Best-Selling Month
```sql
SELECT 
    DATE_FORMAT(sale_date,'%Y') AS YEAR,
    DATE_FORMAT(sale_date,'%M') AS MONTH,
    AVG(total_sale)
FROM retail_sales
GROUP BY YEAR, MONTH;
```

#### 🔹 Top 5 Customers by Sales
```sql
SELECT customer_id, SUM(total_sale) AS Total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY Total_sales DESC
LIMIT 5;
```

#### 🔹 Unique Customers per Category
```sql
SELECT category, COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;
```

#### 🔹 Sales Shifts (Morning, Afternoon, Evening)
```sql
WITH hourly_sale AS (
    SELECT *,
        CASE
            WHEN HOUR(sale_time) < 12 THEN 'Morning'
            WHEN HOUR(sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift
ORDER BY FIELD(shift, 'Morning', 'Afternoon', 'Evening');
```

---

## 📊 Findings

- **Customer Demographics**: Data spans across various age groups and includes both genders.
- **High-Value Customers**: Top 5 customers contribute significantly to total sales.
- **Product Preferences**: Clothing and Beauty categories are frequent choices.
- **Time-Based Trends**: Evening shift has high sales frequency; monthly analysis highlights peak periods.
- **Category Engagement**: Some categories attract more unique customers than others.

---

## 📈 Reports Overview

- **Sales Summary**: Includes total sales per category, high-value transactions, and transaction volume.
- **Trend Analysis**: Monthly and shift-wise analysis to track performance over time.
- **Customer Insights**: Includes unique buyers, customer segmentation by category, and top spenders.

---

## ✅ Conclusion

This project demonstrates the use of SQL for:

- Structuring and cleaning transactional data.
- Conducting data exploration and descriptive analysis.
- Writing business-driven SQL queries for actionable insights.

It is a useful resource for anyone learning SQL or working on data analytics in retail environments.

---

## 🚀 How to Use

1. Clone this repository to your system.
2. Open the `.sql` file in MySQL Workbench or any SQL tool.
3. Import the provided CSV dataset into your MySQL server.
4. Run the queries step-by-step to create the database, clean the data, and extract insights.
