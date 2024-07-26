# Wakmart-sales
-- Create database
CREATE DATABASE IF NOT EXISTS walmartSales;

-- Create table
CREATE TABLE IF NOT EXISTS sales(
	invoice_id VARCHAR(30) NOT NULL PRIMARY KEY,
    branch VARCHAR(5) NOT NULL,
    city VARCHAR(30) NOT NULL,
    customer_type VARCHAR(30) NOT NULL,
    gender VARCHAR(30) NOT NULL,
    product_line VARCHAR(100) NOT NULL,
    unit_price DECIMAL(10,2) NOT NULL,
    quantity INT NOT NULL,
    tax_pct FLOAT(6,4) NOT NULL,
    total DECIMAL(12, 4) NOT NULL,
    date DATETIME NOT NULL,
    time TIME NOT NULL,
    payment VARCHAR(15) NOT NULL,
    cogs DECIMAL(10,2) NOT NULL,
    gross_margin_pct FLOAT(11,9),
    gross_income DECIMAL(12, 4),
    rating FLOAT(2, 1)
);

    
    alter table sales add column time_of_day varchar(20);
UPDATE sales 
SET 
    time_of_day = (CASE
        WHEN time BETWEEN '00:00:00' AND '12:00:00' THEN 'Morning'
        WHEN time BETWEEN '00:00:00' AND '16:00:00' THEN 'Afternoon'
        ELSE 'Evening'
    END);
    
    
    ALTER TABLE sales ADD COLUMN day_name VARCHAR(10);
    UPDATE sales
    SET day_name=DAYNAME(DATE);
    
    
        ALTER TABLE sales ADD COLUMN Month_name VARCHAR(10);
    UPDATE sales
    SET Month_name=MONTHNAME(DATE);
    
    
    
    -- How many unique cities does the data have?
    
SELECT  DISTINCT city FROM sales;


-- In which city is each branch?

SELECT DISTINCT city,branch
FROM sales;

-- How many unique product lines does the data have?
SELECT COUNT(DISTINCT product_line) FROM sales ;

-- What is the most common payment method?
SELECT payment,COUNT(payment) as cnt FROM sales
GROUP BY payment
ORDER BY cnt DESC;

-- What is the most selling product line?
SELECT product_line, COUNT(product_line) FROM sales
GROUP BY product_line;

-- What is the total revenue by month?
SELECT Month_name ,SUM(total) as month_total fROM sales
	GROUP BY Month_name 
    ORDER BY month_total DESC;

-- What product line had the largest revenue?
SELECT product_line AS producr, SUM(total) AS total_revenue 
FROM sales
GROUP BY product_line
ORDER BY total_revenue DESC LIMIT 1;

-- What month had the largest COGS?
SELECT Month_name,SUM(cogs) AS cogs FROM sales
GROUP BY Month_name
ORDER BY cogs DESC LIMIT 1;

-- Which branch sold more products than average product sold?
SELECT branch, AVG(quantity) as avg_branch 
FROM sales
 GROUP BY branch
HAVING avg_branch> (SELECT  AVG(quantity) as avg_qtysold FROM sales);



-- Fetch each product line and add a column to those product line showing "Good", "Bad". Good if its greater than average sales
 SELECT 
product_line,
ROUND(AVG(total),2) AS avg_sales,
(CASE
WHEN AVG(total) > (SELECT AVG(total) FROM sales) THEN "Good"
ELSE "Bad"
END)
AS Remarks
FROM sales
GROUP BY product_line;

-- What is the most common product line by gender?
SELECT gender,product_line,
COUNT(gender) AS total_cnt
FROM sales 
GROUP BY gender,product_line
ORDER BY total_cnt DESC;

-- What is the average rating of each product line 
SELECT product_line,
AVG(rating) AS avg_rating FROM sales
GROUP BY product_line
ORDER BY avg_rating DESC;













    













    
         
         
         
         
         
    
    
