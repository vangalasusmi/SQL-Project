What issues will you address by cleaning the data?
Queries:
Below, provide the SQL queries you used to clean your data.

1. Remove the column data userid from the analytics table
 Query:  alter table analytics drop column userid
2. Visitid and visitstart time in the analytics table have the same data, Noticed that it appears in Unix format, so changed the visitstarttime to postgres Time
Query: 
SELECT TO_TIMESTAMP(CAST(visitstarttime AS integer))::TIME AS converted_time
FROM analytics
LIMIT 10;
3. I thought to Normalize by replacing the repetitive socialEngagementType values in your original table(analytics) with more space-efficient integer references to a lookup table.
4. Modified the datatypes of columns based on the length of the field.
For example fullvisitorid is a numeric datatype
Queries
ALTER TABLE all_sessions ALTER COLUMN fullvisitorid TYPE numeric using fullvisitorid::numeric.
ALTER TABLE all_sessions ALTER COLUMN revenue TYPE bigint using revenue::bigint.
bigint datatype can store upto 8 bytes while numeric datatype can store with a very high degree of precision and scale
5. Modified the unit-price by diving the unit-price/1000000
update analytics set unit_price=unit_price/1000000

