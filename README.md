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
This is e-commerce data, which has product details, sales, and visitor data, and buying patterns based on the city and country.

## Challenges 
(1) Most Challenging thing was Data cleaning and understanding the data as it requires a lot of time and effort.

(2) Data was messy and assigning correct DataTypes

## Future Goals
I Would have analyzed data in detail and made more assumptions as I feel the data set provided is incomplete.
