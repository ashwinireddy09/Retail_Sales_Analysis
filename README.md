create database Retail_Sales_Analysis_Project;
use retail_sales_analysis_project;

create  table salesanalytics(
  transactions_id int, 
  sale_date date, 
  sale_time time,  
  customer_id int, 
  gender varchar(30), 
  age int, 
  category varchar(30), 
  quantity int, 
  price_per_unit int, 
  cogs int, 
  total_sale int
);
select * from salesanalytics;
describe salesanalytics;


/****Write a SQL query to retrieve all columns for sales made on '2022-11-05'****/

select * from 
salesanalytics
where sale_date='2022-11-05';



/***Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022***/
SELECT * 
FROM salesanalytics 
WHERE category = 'Clothing' 
  AND DATE_FORMAT(sale_date, '%Y-%m') = '2022-11'
  AND quantity >= 4 
LIMIT 50000;



/******Write a SQL query to calculate the total sales (total_sale) for each category********/
select category,
sum(total_sale) as total_sales
from salesanalytics
group by category;




/****Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category***/

select avg(age) as avg_age
from salesanalytics
where category ='Beauty';


/***Write a SQL query to find all transactions where the total_sale is greater than 1000***/

select * 
from salesanalytics
where transactions_id >1000;



/****Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category****/

select category,gender,count(Transactions_id) as total_transactions
from salesanalytics
group by category, gender;

/****Write a SQL query to calculate the average sale for each month. Find out best selling month in each year***/

-- Average sales per month
SELECT DATE_FORMAT(sale_date, '%Y-%m') AS month, 
       AVG(total_sale) AS avg_sales
FROM salesanalytics
GROUP BY DATE_FORMAT(sale_date, '%Y-%m')
ORDER BY month;

-- Best-selling month per year (highest total sales)
SELECT YEAR(sale_date) AS year, 
       MONTH(sale_date) AS month,
       SUM(total_sale) AS total_sales
FROM salesanalytics
GROUP BY YEAR(sale_date), MONTH(sale_date)
ORDER BY year, total_sales DESC;



/****Write a SQL query to find the top 5 customers based on the highest total sales ****/
select customer_id,sum(total_sale)as total_sales
from salesanalytics
group by customer_id
order by total_sales desc
limit 5;



/****Write a SQL query to find the number of unique customers who purchased items from each category****/

SELECT category, COUNT(DISTINCT customer_id) AS unique_customers
FROM salesanalytics
GROUP BY category;


/****Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)***/

SELECT 
  CASE 
    WHEN HOUR(sale_date) < 12 THEN 'Morning'
    WHEN HOUR(sale_date) BETWEEN 12 AND 17 THEN 'Afternoon'
    ELSE 'Evening'
  END AS shift,
  COUNT(*) AS total_orders
FROM salesanalytics
GROUP BY shift;




