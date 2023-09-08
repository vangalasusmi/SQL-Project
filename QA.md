What are your risk areas? Identify and describe them.



QA Process:
Describe your QA process and include the SQL queries used to execute it.

1.Data Validation: the Visitid and visitstarttime are same, Here the data accuracy and completeness of the data is missing.
Unit price defined for each unit_product is incomplete as the transactionrevenue is low.

 Data Integrity is not maintained between the products tables and sales_by_sku tables as the foreign key constraints are not satisfied.
 Issue when considering primary table as product (ProductSKU as a pk), Sales_By_sku (SKU as fk)
ERROR:  Key (productsku)=(GGOEYAXR066128) is not present in table "products".insert or update on table "sales_by_sku" violates foreign key constraint "sales_by_sku_productsku_fkey" 

ERROR:  insert or update on table "sales_by_sku" violates foreign key constraint "sales_by_sku_productsku_fkey"
SQL state: 23503
Detail: Key (productsku)=(GGOEYAXR066128) is not present in table "products".


