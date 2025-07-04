# DSA-PROJECT-CASE-STUDY-2-
This repository contains my second work on a data analysis project as part of my coursework in the (Digital Skill-up Africa program by the incubator hub). As a data analyst in training, i am excited to apply my skills and knowledge to real-world problems and contribute to the success of @incubator hub. This project will be graded by the organization, and i look forward to receiving feedback and improving my skills. Thank you and GOD bless you.
## PROJECT TOPIC: Kultra Mega Stores Inventory (CASE STUDY 2)
### PROJECT OVERVIEW 
This project contains one repository and it is aimed at analyzing the data and presenting key insights and findings by solving two case scenarios provided to make reasonable decisions whiich then enable us to tell compelling stories around our data from the insight gotten and to know the best performance from our data.
### SKILLS DEMOSTRATED 
- Data analysis
- Problem solving
- Data interpretation
### DATA SOURCES
The primary source of data used here is inventory csv and this file is an open source data that can be freely downloaded from an open source online such as kaggle or FRED or any other data repository site.
### TOOLS USED 
- SQL Server [download here] (https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
  1.  For data retrieval
  2.  Data filtering
  3.  Data Aggregation
  4.  Data joining
  5.  Data grouping and sorting
### DATA CLEANING AND PREPARATION
1. Data importation; I imported my csv file to my sql server environment using "import flat file".
2. Data inspection; I inspected the data for quality issues, data type inconsistencies and i also set my order_id column as my primary key. 
3. Data loading; I loaded my cleaned data set into the sql server to begin analysis.
### EXPLORATORY DATA ANALYSIS (EDA)
- Case Scenario I
1. Which product category had the highest sales?
2. What are the Top 3 and Bottom 3 regions in terms of sales?
3. What were the total sales of appliances in Ontario?
4. Advise the management of KMS on what to do to increase the revenue from the bottom
10 customers
5. KMS incurred the most shipping cost using which shipping method?
- Case Scenario II
6. Who are the most valuable customers, and what products or services do they typically
purchase?
7. Which small business customer had the highest sales?
8. Which Corporate Customer placed the most number of orders in 2009 - 2012?
9. Which consumer customer was the most profitable one?
10. Which customer returned items, and what segment do they belong to?
11. If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company appropriately spent shipping costs based on the order priority. 

### DATA ANALYSIS 
These are some of the DAX expressions used during my analysis;

 ``` SQL
select top 1 product_category, sum(sales) as total
from [KMS Sql Case Study]
group by product_category
order by total desc 
```
 ``` SQL
select top 3 region, sales 
from [KMS Sql Case Study]
order by sales asc
```
``` SQL
 select top 1 customer_name, sum(sales) as highest_sales
 from [KMS Sql Case Study]
 where customer_segment = 'small business'
 group by Customer_name
 order by highest_sales desc
```
### ANALYSIS 
1. Which product category had the highest sales?
   
``` SQL
select top 1 product_category, sum(sales) as total
from [KMS sql Case Study]
group by product_category
order by total desc 
```
2. What are the Top 3 and Bottom 3 regions in terms of sales?

 ``` SQL
select top 3 Region, sales
from [KMS Sql Case Study]
order by sales desc
```
``` SQL
select top 3 region, sales 
from [KMS Sql Case Study]
order by sales asc
```
3. What were the total sales of appliances in Ontario?
   
``` SQL
select sum(sales) as total_sales 
from [KMS Sql Case Study]
where product_sub_category = 'appliances'
and region = 'ontario'
```
4. Advise the management of KMS on what to do to increase the revenue from the bottom 10 customers

``` SQL
select top 10 customer_name, sum(sales) as revenue
from  [KMS Sql Case Study]
group by Customer_Name
order by revenue asc
```
- Comment; To increase revenue from the bottom 10, understanding the specific needs and pain points of these customers to tailor services that meet their requirement would go a long way,
upsell and cross-sell services, identify unmet needs,offer discounts and rebates, optimize retention efforts. These points should be considered clearly by the management to increase revenue from the bottom 10.

5. KMS incurred the most shipping cost using which shipping method?

``` SQL
select top 1 ship_mode, sum(shipping_cost) as total_shipping_cost
from [KMS Sql Case Study]
group by ship_mode
order by total_shipping_cost desc
```
- Case Scenario II
6. Who are the most valuable customers, and what products or services do they typically purchase?

``` SQL
select top 10 customer_name, sum(sales) as total_spent
from [KMS Sql Case Study]
group by customer_name
order by total_spent desc
```
``` SQL
select top 10 (customer_name), product_name, sum(sales) as total_sales
from [KMS Sql Case Study]
where customer_name in (
select customer_name
from (
select top 10 customer_name, sum(sales) as total_spent
from [KMS Sql Case Study]
group by customer_name
order by total_spent desc
) as top_customers
)
group by customer_name, product_name
order by total_sales desc;
```
7. Which small business customer had the highest sales?

``` SQL
select top 1 customer_name, sum(sales) as highest_sales
from [KMS Sql Case Study]
where customer_segment = 'small business'
group by Customer_name
order by highest_sales desc
```
8. Which Corporate Customer placed the most number of orders in 2009 - 2012?

``` SQL
 select top 1 customer_name, count(order_id) as total_order
 from [KMS Sql Case Study]
 where customer_segment = 'corporate'
 and order_date between '2009-01-01' and '2012-12-31'
 group by customer_name
 order by total_order desc
```
9. Which consumer customer was the most profitable one?

``` SQL
 select top 1 customer_name, sum(profit) as total_profit
 from [KMS Sql Case Study]
 where customer_segment = 'consumer'
 group by customer_name
 order by total_profit desc
```
10. Which customer returned items, and what segment do they belong to?

``` SQL
 select distinct customer_name, customer_segment
 from [KMS Sql Case Study]
 join [dbo].[Order_Status]
 on [KMS Sql Case Study].Order_ID = [dbo].[Order_Status].Order_ID
 where Status = 'returned'
```
11. If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company appropriately spent shipping costs based on the order priority.

``` SQL
 select count(order_id) as countofid, order_priority, ship_mode, sum(Sales - profit) as estimatedshippingcost, 
 avg(datediff(day,[order_date], [ship_date])) as avgshipdays
 from [KMS Sql Case Study]
 group by order_priority, ship_mode
 order by order_priority, ship_mode desc
```
- Comment; from this query written and based on our analysis carried out high priority orders are being shipped via the fastest method(express air) and low priority orders are being shipped via the most economical method (delivery truck) and also high priority orders are also being shipped via a slower method (delivery truck) and low priority orders  are also shipped via an expensive method(express air). so in conclusion, we can say that the company's shipping cost strategy appears to be inconsistent or not well optimized and this could lead to inefficiences and unnecessary expenses.
 
### RESULTS AND RECOMMENDATIONS 
- Based on our analysis carried out it revelas that Technology emerges as the top-performing product category, generating the highest sales.
- Shipping cost insight; Our analysis reveals that KMS incurred the highest shipping costs, primarily due to the use of the DELIVERY TRUCK shipping method.





