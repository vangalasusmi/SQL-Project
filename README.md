![image](https://github.com/vangalasusmi/SQL-Project/assets/9608114/ed8f26a4-3ab9-4cc9-a97a-83c0cf8354ae)# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
(Fill in your description and goals here)

The goal of the project is to do data cleaning, identify the relationships between the entities, and retrieve meaningful insights from them.
Detect the QA process for better data management and performance.


## Process
Firstly I loaded the data from CSV files to tables.
Made Data conversions according to the data.
Performed Data Cleaning Processes like modifying the values, and removing columns which are not useful.
Performed some data Quality checks and modified the data accordingly.
Wrote some queries to provide some meaningful insights on the data.

## Results
This is e-commerce data, which has product details, sales, and visitor data, and buying patterns based on the city and country.

## Challenges 
Logically I can relate relationships between tables, but when I try to implement it is not possible, because of random data.

For example, we have a ProductSKU column in the products table which is a generic code for every product, and an SKU value in the sales_by_sku table. I want to create a relationship between these 2 tables products and sales_by_sku tables where productSKU in the products table is the primary key and sales_by_sku is a foreign key.

But here, I faced a challenge because the data existing in sales_by_sku is not present in the products table, so it was not allowing me to create a primary-foreign key relationship.

One more example, It was very difficult to identify the relationships between all_sessions and analytics table, Though they are similar columns like visitid,fullvisitorid.

## Future Goals
Would have analaysed data in detail and make more and more assumptions as I feel the data set provided is incomplete and would create a better ERD Diagrams
