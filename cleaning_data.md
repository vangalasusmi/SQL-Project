## What issues will you address by cleaning the data?

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
## Analytics table
--Remove the column data userid from the analytics table
``` SQL
alter table analytics drop column userid
```
```SQL
-- Change Datatype for unit_price 
alter table analytics
alter column unit_price type float using unit_price::float
```

```SQL
-- Change Datatype for revenue 
alter table analytics
alter column revenue type bigint using revenue::bigint
```

```SQL
-- Change Datatype for bounces
alter table analytics
alter column bounces type int using bounces::int
```
```SQL
-- Change Datatype for pageviews
alter table analytics
alter column pageviews type int using pageviews::int
```
```SQL
-- Change Datatype for units_sold
alter table analytics
alter column units_sold type int using units_sold::int
```
```SQL
-- Change Datatype for date 
alter table analytics
alter column date type date using date::date
```
```SQL
-- Change Datatype for visitid
alter table analytics
alter column visitid type int using visitid::integer
```
```SQL
-- Change Datatype for visitnumber 
alter table analytics
alter column visitnumber type int using visitnumber::integer
```
--- Visitid and visitstart time in the analytics table have the same data, Noticed that it appears in Unix format, so changed the visitstarttime to postgres Time
``` SQL
SELECT TO_TIMESTAMP(CAST(visitstarttime AS integer))::TIME AS converted_time
FROM analytics
LIMIT 10;
```
--Modified the datatypes of columns based on the length of the field.(// bigint datatype can store upto 8 bytes while numeric datatype can store with a very high degree of precision and scale)

``` SQL
ALTER TABLE all_sessions ALTER COLUMN fullvisitorid TYPE numeric using fullvisitorid::numeric.
ALTER TABLE all_sessions ALTER COLUMN revenue TYPE bigint using revenue::bigint.
```
-- Modified the unit-price by diving the unit-price/1000000
``` SQL
update analytics set unit_price=unit_price/1000000
```

### Cleaning Products Table

```SQL
-- Change Datatype of sentimentmagnitude
alter table products
alter column sentimentmagnitude type float using sentimentmagnitude::float
```
```SQL
-- Change Datatype of senitmentscore
alter table products
alter column sentimentscore type float using sentimentscore::float
```
```SQL
-- Change Datatype of restockingleadtime 
alter table products
alter column restockingleadtime type int using restockingleadtime::int
```
```SQL
-- Change Datatype of stocklevel
alter table products
alter column stocklevel type int using stocklevel::int
```
```SQL
-- Change Datatype of orderedquantity 
alter table products
alter column orderedquantity type int using orderedquantity::int
```
### Changing Datatype and assigning pk for sales_by_sku
```SQL
--Assigning PK to sales_by_sku
alter table sales_by_sku
add primary key (productsku)
```
```SQL
-- Changing datatype of total_ordered
alter table sales_by_sku
alter column total_ordered type int using total_ordered::int
```

