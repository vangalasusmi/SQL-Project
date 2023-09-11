## What are your risk areas? Identify and describe them.
(1) Duplicate Records: Duplicate Records may take up more space, also could be a factor for inaccurate values.

(2) Correct Datatypes : Datatypes should be correct across all tables.

(3) Unique IDs :
some IDs that are cruical to the data shoud be unique

## QA Process:
Describe your QA process and include the SQL queries used to execute it.

- Starting with all_sessions column 

-- Making sure there is no duplicate fulllvisitorid as there should be distinct id for every user
```SQL
select 
distinct(fullvisitorid) from all_sessions
```
---unit_price and unit_sold should be in positive number and more than zero to get accurate numbers
```SQL
alter table analytics alter column unit_price type int using unit_price::int

alter table analytics alter column unit_sold type int using unit_sold::int

select * from analytics where unit_price < 0 and units_sold < 0
---0 records retunred.

-- Products Table
-- Each product should only have unique id
select distinct(sku) from products

-- orderedquantity and stocklevel should be more than 0
select * from products
where orderedquantity >= 0 and stocklevel >=0
--0 records returned.

-- Total products ordered should be atleast one and stocklevel should be more than zero
select * 
from sales_report
where total_ordered > 0 and stocklevel >= 0
select * from sales_report
```
