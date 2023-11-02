Question 1:  
With the column of sales_reports as inspiration, find what item has the highest ratio of total_ordered to stock level using sales_by_sku
SQL Queries:

Select p."SKU", 
	p."name",
	ss."total_ordered", 
	p."stockLevel", 
	Case
	when p."stockLevel" = 0 then 0.0
	else cast(ss."total_ordered" as  numeric)/cast(p."stockLevel" as numeric) 
	end as "ratio"
from sales_by_sku_view ss
join products_view p
on p."SKU" = ss."productSKU"
order by "ratio" desc

Answer: 
The highest ratio of ordered/stock is an Android Infant Short Sleeve Tee Pewter of SKU GGOEAAWJ062548. 


Question 2: How many products have their number of orders accurately represented in the sales_by_sku table?

SQL Queries:
SELECT count(*) 
FROM products p
Join sales_by_sku s
on p."SKU" = s."productSKU"
where p."orderedQuantity" = s."total_ordered" and
	p."orderedQuantity" >0 and
	s."total_ordered" >0
Answer: 
Only 5 rows match.


Question 3: Add a row to the cleaned sales_reports table for a SKU *not* already present in the table  

SQL Queries:

--to find a suitable sku

Select "productSKU"
FROM sales_by_sku_view
where "productSKU" IN (
    Select p."SKU"
    From products_view p
    left join sales_report_view sr
    on p."SKU" = sr."productSKU"
    where sr."productSKU" is null
)

Answer:
As it turns out, there are no entries in sales_by_sku_view
for the skus that are missing from sales_report_view.

Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
