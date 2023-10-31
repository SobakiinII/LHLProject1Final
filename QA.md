What are your risk areas? Identify and describe them.

The data sets came in with massive ammounts of nulls and inaccurate data.
Overall, this is less of a database and more five tables that have one column in common. The only thing that consistently works as a foreign key is the product sku.
Convieniently enough, this column also works as a primary key for the products table. 

Problems would then arise is the tables using the sku as a foreign key have null entries in that column or entries that don't exist in the products table. 

In a similar fashion, in the sales_% tables, it would be problematic if the SKUs were not unique across the rows. 
Sales_by_sku is named its purpose, to list each individual sku its total sales. If any skus were entered twice, it could interfere with any query trying to access a specific report or make a large select query inaccurate. 

QA Process:
Describe your QA process and include the SQL queries used to execute it.

Firstly, we need to make absolutely sure there are no duplicate entries in products.SKU so that it camn be used as the table's primary key.

    SELECT "SKU", count(*)
    FROM products_view
    GROUP BY "SKU"
    having count(*) >1

This query returns zero rows, meaning all rows have unique entries for this column.

Following that, it would be helpful to ensure the entries in the sales_% tables are just as unique:
    Select "productSKU", count(*)
    From sales_by_sku
    Group By "productSKU"
    having count(*) >1

    Select "productSKU", count(*)
    From sales_report
    Group By "productSKU"
    having count(*) >1

Afterwards, it would be helpful to run this query 

    ALTER TABLE IF EXISTS public.products
        ADD PRIMARY KEY ("SKU");

with the primary key set, we can assign it as a foreign key to the other tables that need it.
Using varients of the following query, I do just that:

ALTER TABLE IF EXISTS public.sales_by_sku
    ADD CONSTRAINT "fk_productSKU" FOREIGN KEY ("productSKU")
    REFERENCES public.products ("SKU") MATCH SIMPLE
    ON UPDATE NO ACTION
    ON DELETE NO ACTION
    NOT VALID;

With this, we have a proper connected database. There still may be some things wrong with it, but this is a good first step.

Next I should ensure there aren't any skus in the sales tables that arent identified in the products table.

    Select p."SKU", sr."productSKU"
    from products_view p
    right join sales_report sr
    on p."SKU" = sr."productSKU"
    where p."SKU" is null

This returns no rows, meaning every sku in sales_report is identified in products.

As far as I can see, there is no column in all_sessions or analytics that works well as primary keys for themselves or as foreign key to connect to other tables.