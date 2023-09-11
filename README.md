## Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
(1) Importing eCommerce Data from Files to Postgresql tables.

(2) Cleaning and transforming data for analytics

(3) Study patterns and finding relations across different tables

(4) Running queries to get business insights

## Process
## Part 1 - Importing eCommerce Data from Files to Postgresql tables

//General Syntax for creating Table and Importing Data
```SQL
   create Table table_name
   (
    column_name  data_type,
    column_name2 data_type,
    .
    .,
    primary key (column_name)

   )
   ```
   General syntax for Copying CSV file
  ```SQL
   copy file_name
   from 'file destination'
   delimeter ',' // mentioning delimiter
   CSV header // mentioning that CSV file have header
   ```
##Part 2 - Data Cleaning

Starting with all_sessions Table
(1) Removing duplicate Rows from the table by Creating a Temporary Table with distinct rows and then Dropping the original table and recreating the table from the temporary table

```SQL
-- Creating Temporary Table with Distinct value 
create table all_sessions_distinct as 
 select distinct on (fullvisitorid) *
 from all_sessions
 order by fullvisitorid
---14,223 records inserted
-- Drop Table all_sessions
 drop table all_sessions
--table dropped
---15,134 records deleted
create table all_sessions as 
select *
from all_sessions_distinct
```
(2) Changing Data-types
```SQL
ALTER TABLE all_sessions
 ALTER TABLE all_sessions ALTER COLUMN fullvisitorid TYPE numeric using fullvisitorid::numeric,
 ALTER TABLE all_sessions ALTER COLUMN totaltransactionrevenue TYPE float using totaltransactionrevenue::double precision,
 ALTER TABLE all_sessions ALTER COLUMN transactions TYPE int using transactions::int,
 ALTER TABLE all_sessions ALTER COLUMN pageviews TYPE int using pageviews::int,
 ALTER TABLE all_sessions ALTER COLUMN sessionqualitydim TYPE int using sessionqualitydim::int,
 ALTER TABLE all_sessions ALTER COLUMN visitid TYPE int using visitid::int,
 ALTER TABLE all_sessions ALTER COLUMN productrefundamount TYPE int using productrefundamount::int,
 ALTER TABLE all_sessions ALTER COLUMN productquantity TYPE int using productquantity::int,
 ALTER TABLE all_sessions ALTER COLUMN productprice TYPE int using productprice::int,
 ALTER TABLE all_sessions ALTER COLUMN productrevenue TYPE int using productrevenue::int,
 ALTER TABLE all_sessions ALTER COLUMN itemquantity TYPE int using itemquantity::int,
 ALTER TABLE all_sessions ALTER COLUMN itemrevenue TYPE int using itemrevenue::int,
 ALTER TABLE all_sessions ALTER COLUMN transactionid TYPE int using transactionid::int,
 ALTER TABLE all_sessions ALTER COLUMN ecommerceaction_type TYPE int using ecommerceaction_type::int,
 ALTER TABLE all_sessions ALTER COLUMN ecommerceaction_step TYPE int using ecommerceaction_step::int;

//Replacing Country names where country value is not available.
```SQL
select *,
       case when country = '(not set)' then  'N/A'
	   else country
	   end
from all_sessions
```
// Replacing city names with country where value is not city
```SQL
update all_sessions
set city = case when city = 'not available in demo dataset' then country
	            when city = '(not set)' then country
		        else city
	       end
```
//Replacing Country names where country value is not availble
```SQL
select *,
       case when country = '(not set)' then  'N/A'
	   else country
	   end
from all_sessions
```
Droping Empty Columns from All_sessions
```SQL
alter table all_sessions
drop column searchkeyword
```
```SQL
alter table all_sessions
drop column itemrevenue
```
```SQL
alter table all_sessions
drop column itemquantity 
```
## Results
Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

``` SQL

select country, city, total_revenue
from ( select country,city,
               max(totaltransactionrevenue) as total_revenue,
	             RANK() OVER (PARTITION BY country ORDER BY max(totaltransactionrevenue) DESC) as rank
        from all_sessions
	      where totaltransactionrevenue is not null
        group by country, city
	 ) as sub
where rank = 1
```
![image](https://github.com/vangalasusmi/SQL-Project/assets/9608114/dac8aaeb-6b9b-4164-9470-2ad16e79da6f)

**Question 2: What is the average number of products ordered from visitors in each city and country?**
``` SQL
select cast(avg(a.total_products_sold) as int) as avg_products_sold,a.city,a.country from
(select sum(productquantity) as total_products_sold,
	   city,country from all_sessions
	   group by city,country) a group by a.city,a.country
	   having avg(a.total_products_sold) <> 0
```
![image](https://github.com/vangalasusmi/SQL-Project/assets/9608114/1756e2da-cc51-472b-8e4b-986f3303944b)


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**
``` SQL
create temp table overall_cate as
(    -- Product categories of products ordered from visitors in each city and country
   select  country, city, p.name as productname, v2productcategory,
           sum(orderedquantity) as total_products

   from all_sessions s
   join products p on s.productsku = p.sku
   group by country,city,p.name, v2productcategory
   having sum(orderedquantity) <> 0
   order by country, city, total_products desc
)
```
// Finding pattern
``` SQL
(select country, city, v2productcategory, max(total_products) as order,
                'Most Popular' as type 
       from overall_cate
group by country, city, v2productcategory
order by max(total_products) desc
limit 3)


union all -- Unioning columns from both the results that shows top 3 most and least popular orders placed by volume in each city and country

	
 -- finding top 3 orders with lowest volume ordered in each city and country 
        (select  country, city, v2productcategory, 
                min(total_products) as order,
               'Least Popular' as type
        from overall_cate
group by country, city, v2productcategory
order by min(total_products) desc
limit 3)
```
![image](https://github.com/vangalasusmi/SQL-Project/assets/9608114/fb77eb56-b82c-4f88-be26-745dc74757f7)

**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**
``` SQL
with sale_by_country as     -- Creating CTE with total products sold in each city from each country with products name
(
        select country, city, p.name as productname,
               sum(orderedquantity) as total_products, 
           rank() over(partition by country,city order by sum(orderedquantity) desc) as rank -- Ranking the grouped products sold in each country and city 
from all_sessions s
join products p on s.productsku = p.sku
group by country,city,p.name
order by country, city
) 
-- selecting highest selling productnames from each city within each country 
select country, city, productname
from sale_by_country
where rank = 1
```
![image](https://github.com/vangalasusmi/SQL-Project/assets/9608114/e345d55a-61a1-456c-b712-9d5f7e6a1a5f)


**Question 5: Can we summarize the impact of revenue generated from each city/country?**
``` SQL
SQL Queries:This Question is inappropriate, because out of 15,000 records only 81 records has the data, So the impact generated is not accurate according to the data. As we have outliers, the impact of revenue generated will not be accurate.
select count(*) from all_sessions where totaltransactionrevenue is  null
15053
But provided the details with the exisiting records which has data.
select country, city,
       sum(totaltransactionrevenue) as revenue
from all_sessions 
group by country, city
having sum(totaltransactionrevenue) <> 0
```

![image](https://github.com/vangalasusmi/SQL-Project/assets/9608114/7a53933d-073f-43f2-a626-ed96172ca9a2)

## Challenges 
(1) Most Challenging thing was Data cleaning and understanding the data as it requires a lot of time and effort.

(2) Data was messy and assigning correct DataTypes

## Future Goals
I Would have analyzed data in detail and made more assumptions as I feel the data set provided is incomplete.
