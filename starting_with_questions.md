Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

``` SQL
SQL Queries:select  a.city,a.country.a.total_revenue from
(select sum(totaltransactionrevenue) as total_revenue,
	   city,
	   country from  all_sessions group by city,country) a
	   order by total_revenue
```

**Question 2: What is the average number of products ordered from visitors in each city and country?**
``` SQL
SQL Queries:select cast(avg(a.total_products_sold) as int) as avg_products_sold,a.city,a.country from
(select sum(productquantity) as total_products_sold,
	   city,country from all_sessions
	   group by city,country) a group by a.city,a.country --order by cast(avg(a.total_products_sold) as int) desc
```

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
( -- finding top 3 orders with highest volume ordered in each city and country 
        select country, city, v2productcategory, max(total_products) as order,
                'Most Popular' as type 
       from overall_cate
group by country, city, v2productcategory
order by max(total_products) desc
limit 3
)

finding top 3 orders with lowest volume ordered in each city and country 
        select  country, city, v2productcategory, 
                min(total_products) as order,
               'Least Popular' as type
        from overall_cate
group by country, city, v2productcategory
order by min(total_products) desc
limit 3
```
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
**Question 5: Can we summarize the impact of revenue generated from each city/country?**
``` SQL
SQL Queries:This Question is inappropriate, because out of 15,000 records only 81 records has the data, So the impact generated is not accurate according to the data. As we have outliers, the impact of revenue generated will not be accurate.
select count(*) from all_sessions where totaltransactionrevenue is  null
15053
```
