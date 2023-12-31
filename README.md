# Customer_Insights_SQL - CHALLENGE 3

### Introduction
You are a Customer Insights Analyst for 'The General Store'.
Can you analyse the following tables to find out crucial information about your customers to provide to your marketing team?

Sharing the link to [SteelData-SQL Challenge 3](https://codebasics.io/)  

-----------------------------------------------------------------------------------------------------------------------------------------
## Table Details 

| Table Name | Description | Column Name |
| ---------- | ----------- | ----------- |
| Customers | contains customer-related data. | Customer_id,first_shop,age,rewards,can_email |
| Orders | contains order-related data. | Order_id,customer_id,date_shop,sales_channel,country_id |
| Baskets | contains order and product ID data. | Order_id,product_id |
| Products | contains product-related data. | Product_id,category,price |
| Country | contains location-related data. | Country_id,country_name,head_office |

------------------------------------------------------------------------------------------------------------------------------------------

## Analysis

1. #### Solved for Average Age:
   Conducted an analysis to determine the average age of customers who made orders in the 'vitamins' product category. Employed **Subquery** and **Join** operations to refine the data for accurate insights.

2. #### Identified Top Ordering Country:
   Employed **Join** and the **LIMIT clause** to identify the country with the highest number of orders. This facilitated a focused approach for our marketing team.

3. #### Found Latest Marketing Email-eligible Order:
   Implemented **IF()** and **MAX()** functions to identify the latest order made by a customer eligible to receive marketing emails. This ensures targeted communication with engaged customers.

4. #### Analyzed Orders by Sales Channel:
   Utilized **COUNT()** and **GROUP BY** to determine the number of orders made in each sales channel. This insight aids in optimizing marketing strategies for different channels.

5. #### Calculated Revenue by Product Category:
   Employed **ORDER BY, SUM()** and **GROUP BY** to calculate the total revenue generated by each product category. This information informs product-focused marketing initiatives.

6. #### Segmented Customers Aged 30 or Older:
   Employed **Join** to identify orders made by customers aged 30 or older.

7. Employed **DISTINCT()** to determine the total number of customers and retrieve the names of all countries.   

## Result: 
Generated comprehensive data insights facilitating targeted marketing strategies for the team. Examples include determining average customer age, identifying top-ordering countries, understanding customer preferences in different product categories, and segmenting customers based on age for more effective marketing outreach.

------------------------------------------------------------------------------------------------------------------------------------------
## Code

*1. What are the names of all the countries in the country table?*
``` js
select distinct(country_name)
from country;
``` 

```
			       Output
Listed all the unique Country names from Country table and they are : UK,USA,China

                            Concept learned
1.DISTINCT()
 
```
-----------------------------------------------------------------------------------------------------

*2. What is the total number of customers in the customers table?*
``` js
select distinct(count(*)) as Number_of_customers
from customers;
``` 

```
                  Output
The total number of customers in the customers table are 8

                 Concept learned
1.DISTINCT()
2.COUNT()

```
-----------------------------------------------------------------------------------------------------

*3. What is the average age of customers who can receive marketing emails (can_email is set to 'yes')?*
``` js
select round(avg(age),0)
from customers
where can_email='yes';
``` 

```
                  Output
The average age of customers who can receive marketing emails (can_email is set to 'yes') is 37

                  Concept learned
1.ROUND()
2.AVG()

```
-----------------------------------------------------------------------------------------------------

*4. How many orders were made by customers aged 30 or older?*
``` js
select count(*) as Number_Of_Orders_Made_By_Customers_Aged_30_or_more
from orders o
join customers c
using(c_id)
where c.age >=30
;
``` 

```
                     Output
3 orders were made by customers aged 30 or older

                  Concept learned
1.JOIN
2.USING

```
-----------------------------------------------------------------------------------------------------

*5. What is the total revenue generated by each product category?*
``` js
select category,sum(price) as Revenue_For_each_Product
from product
group by category
order by Revenue_For_each_Product desc;
``` 

```
			                Output
The revenue generated by the "vitamins" product category amounts to $22.98,
while the "sports" category brings in $12.49 in revenue,
and the "food" category contributes $6.88 in revenue.

                  		Concept learned
1.GROUP BY
2.ORDER BY
3.SUM()

```
-----------------------------------------------------------------------------------------------------

*6. What is the average price of products in the 'food' category?*
``` js
select category,round(avg(price),2) as Average_Price_Food
from product
where category='food';
``` 

```
		    Output
The average price of products in the 'food' category is 3.44

                  Concept learned
1.ROUND()
2.AVERAGE ()
```
-----------------------------------------------------------------------------------------------------

*7. How many orders were made in each sales channel (sales_channel column) in the orders table?*
``` js
select sales_channel,count(*) as Number_Of_Orders_in_Each_Channel
from orders
group by sales_channel;
``` 

```
			Output
There were 5 orders placed through the Retail sales channel and 3 orders placed through the Online sales channel.

                  Concept learned
1.COUNT()
2.GROUP BY
```
-----------------------------------------------------------------------------------------------------

*8.What is the date of the latest order made by a customer who can receive marketing emails?*
``` js
select max(if(can_email='yes',first_shop,'')) as Lastest_Order_Date
from customers;
``` 

```
	             Output
the date of the latest order made by a customer who can receive marketing emails : 2022-07-04

                  Concept learned
1.IF ()
2.MAX ()
```
-----------------------------------------------------------------------------------------------------

*9. What is the name of the country with the highest number of orders?*
``` js
select c.country_name,count(o.o_id) as Highest_number_of_Order
from orders o
join country c
using(country_id)
group by c.country_name
limit 1;
``` 

```
			Output
The country with the highest number of orders, totaling 5, is the United Kingdom (UK).

                  Concept learned
1.JOIN
2.LIMIT

```
-----------------------------------------------------------------------------------------------------

*10. What is the average age of customers who made orders in the 'vitamins' product category?*
``` js
SELECT round(AVG(age),1) AS average_age
FROM customers
WHERE c_id IN (
    SELECT o.c_id
    FROM orders o
    JOIN basket b ON o.o_id = b.o_id
    JOIN product p ON b.p_id = p.p_id
    WHERE p.category = 'vitamins'
);
``` 

```
			Output
The average age of customers who made orders in the 'vitamins' product category is 32.5.

                  Concept learned
1.SUBQUERY
2.JOIN
3.AVG()
4.ROUND()

```
-----------------------------------------------------------------------------------------------------
