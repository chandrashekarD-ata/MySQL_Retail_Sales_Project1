**SQL Retail Sales Analysis**

**Project Overview**

This project is designed to analyze retail sales data using SQL. 
It focuses on creating a structured database, cleaning the data, and performing various analyses to extract meaningful insights. 
The analysis aims to answer key business questions and support data-driven decision-making.

**Database Creation**

**Create Database**

```sql
CREATE DATABASE sql_project_p1;
USE sql_project_p1;
```
**Create Table**

```sql
CREATE TABLE retail_sales (
    transaction_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```
**Data Cleaning**
**Select and Count Records**

``sql
SELECT * FROM retail_sales LIMIT 7;
SELECT COUNT(*) FROM retail_sales;
```

**Identify and Delete Null Records**

```sql
SELECT * FROM retail_sales
WHERE transaction_id IS NULL OR sale_date IS NULL OR sale_time IS NULL OR
      gender IS NULL OR category IS NULL OR quantity IS NULL OR cogs IS NULL OR
      total_sale IS NULL;
```

**DELETE FROM retail_sales**
```sql
WHERE transaction_id IS NULL
OR
sale_date IS NULL
OR
sale_time IS NULL
OR
gender IS NULL
OR
category IS NULL
OR
quantity IS NULL
OR
cogs IS NULL
OR
total_sale IS NULL;
```

**Data Analysis**

**General Queries**

```sql
SELECT COUNT(*) as total_sales FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) as total_customers FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;
```

**Business Key Problems & Answers**

**Sales on Specific Date**

```sql
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';
```

**Clothing Sales in Nov-2022**

```sql
SELECT * FROM retail_sales 
WHERE category = 'Clothing' 
AND DATE_FORMAT(sale_date, 'YY-mm') = '2022-11' 
AND quantity > 4;
```

**Total Sales by Category**

```sql
SELECT category, SUM(total_sale) as net_sale, COUNT(*) as total_orders 
FROM retail_sales 
GROUP BY category;
```

**Average Age of Beauty Product Customers**

```sql
SELECT ROUND(AVG(age), 2) as avg_age FROM retail_sales WHERE category = 'Beauty';
```
**Transactions Over 1000**

```sql
SELECT * FROM retail_sales WHERE total_sale > 1000;
```

**Transactions by Gender in Each Category**

```sql
SELECT category, gender, COUNT(*) as total_trans 
FROM retail_sales 
GROUP BY category, gender 
ORDER BY category;
```

**Best Selling Month Each Year**

```sql
SELECT year, month, avg_sale FROM (
    SELECT
        YEAR(sale_date) as year,
        MONTH(sale_date) as month,
        AVG(total_sale) as avg_sale,
        RANK() OVER(PARTITION BY YEAR(sale_date) ORDER BY AVG(total_sale) DESC) as rank_no
    FROM retail_sales
    GROUP BY YEAR(sale_date), MONTH(sale_date)
) as t1
WHERE rank_no = 1;
```

**Top 5 Customers by Sales**

```sql
SELECT customer_id, SUM(total_sale) as total_sales 
FROM retail_sales 
GROUP BY customer_id 
ORDER BY total_sales DESC 
LIMIT 5;
```

**Unique Customers by Category**

```sql
SELECT category, COUNT(DISTINCT customer_id) as cnt_unique_cs 
FROM retail_sales 
GROUP BY category;
```

**Orders by Shift**

```sql
WITH hourly_sale AS
(
    SELECT *,
    CASE
        WHEN HOUR(sale_time) < 12 THEN 'Morning'
        WHEN HOUR(sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
    FROM retail_sales
)
SELECT shift, COUNT(*) as total_orders 
FROM hourly_sale 
GROUP BY shift;
```

**Contact
ChandrashekarD**

**Email: sincerrechandrav@gmail.com**

Feel free to contribute to this project by submitting issues or pull requests. Thank you!
