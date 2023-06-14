What issues will you address by cleaning the data?
 I will be putting columns in their correct data type, removing null/empty columns, removing extra spaces from strings, and adding primary and secondary keys


Queries:
Below, provide the SQL queries you used to clean your data.

-Firstly, I used the DROP function to remove null values/empty columns from tables
```
ALTER TABLE all_sessions DROP COLUMN itemquantity;

ALTER TABLE all_sessions DROP COLUMN itemrevenue;

ALTER TABLE all_sessions DROP COLUMN searchkeyword;

ALTER TABLE analytics DROP COLUMN userid;

ALTER TABLE sales_report
    DROP COLUMN name, 
    DROP COLUMN sentimentScore, 
    DROP COLUMN sentimentMagnitude;
```
-Then, I updated the unit_price, productprice, totaltransactionrevenue, and revenue columns by dividing it by 1,000,000
```
  UPDATE analytics
SET unit_price = unit_price / 1000000;

UPDATE analytics
SET unit_price = (unit_price/1000000)

UPDATE all_sessions
SET productprice = (productprice/1000000)

UPDATE analytics
SET revenue = (revenue/1000000)

--changed revenue
UPDATE all_sessions
SET totaltransactionrevenue = (totaltransactionrevenue/1000000)

```
-Remove/trim excess spaces
```
UPDATE products
SET name = TRIM(both ' ' FROM name)
```

-Changed datatype to integer
```
ALTER TABLE analytics
ALTER column timeonsite TYPE bigint USING (timeonsite::bigint);

ALTER TABLE all_sessions
ALTER column totaltransactionrevenue TYPE bigint USING (totaltransactionrevenue::bigint);
```
- Changed datatype to date
```
ALTER TABLE all_sessions
ALTER COLUMN date TYPE DATE USING date::date;
```
-Changed to date format
```
ALTER TABLE analytics
ALTER COLUMN timeonsite TYPE interval
USING (timeonsite || ' minutes')::interval;
```
-Changed to date and time format 
```
UPDATE analytics
SET "visitStartTime" = TO_CHAR(TO_TIMESTAMP("visitStartTime"::bigint), 'YYYY-MM-DD HH24:MI:SS')
```


-Added primary key to products table

ALTER TABLE products
ADD PRIMARY KEY (sku);

--Added foreign key to sales_report table

ALTER TABLE sales_report
ADD FOREIGN KEY (productsku) REFERENCES products(sku);

--Added foreign key to sales_by_sku  table

ALTER TABLE sales_by_sku 
ADD FOREIGN KEY (productsku) REFERENCES products(sku);

--to connect data i had to add productSKU that don't exist to the 
--SKU in products table

INSERT INTO products (sku)
SELECT DISTINCT productsku
FROM sales_by_sku
WHERE productsku NOT IN (SELECT sku FROM products);

--Added primary key to all_sessions table
ALTER TABLE all_sessions
ADD COLUMN session_id SERIAL;

UPDATE all_sessions
SET session_id = DEFAULT;

ALTER TABLE all_sessions
ALTER COLUMN session_id SET NOT NULL,
ADD PRIMARY KEY (session_id);

--Added primary key to analytics table

ALTER TABLE analytics
ADD COLUMN analytics_id SERIAL;

UPDATE analytics
SET analytics_id = DEFAULT;

ALTER TABLE analytics
ALTER COLUMN analytics_id SET NOT NULL,
ADD PRIMARY KEY (analytics_id);

--Added primary key to sales_by_sku table

ALTER TABLE sales_by_sku
ADD COLUMN sales_by_sku_id SERIAL;

UPDATE sales_by_sku
SET sales_by_sku_id = DEFAULT;

ALTER TABLE sales_by_sku
ALTER COLUMN sales_by_sku_id SET NOT NULL,
ADD PRIMARY KEY (sales_by_sku_id);

--added values to products table in the column SKU, 
--that can be found in all_sessions table

INSERT INTO products (sku)
SELECT DISTINCT productsku
FROM all_sessions
WHERE productsku NOT IN (SELECT sku FROM products);

--linked products and all_session

ALTER TABLE all_sessions 
ADD FOREIGN KEY (productsku) REFERENCES products(sku);
