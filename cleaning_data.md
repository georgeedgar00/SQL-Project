What issues will you address by cleaning the data?
 I will be putting columns in their correct data type, removing null/empty columns and removing extra spaces from strings


Queries:
Below, provide the SQL queries you used to clean your data.

- Firstly, I used the DROP function to remove null values/empty columns from tables
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
- Then, I updated the unit_price, productprice, totaltransactionrevenue, and revenue columns by dividing it by 1,000,000
```
  UPDATE analytics
SET unit_price = unit_price / 1000000;

UPDATE analytics
SET unit_price = (unit_price/1000000)

UPDATE all_sessions
SET productprice = (productprice/1000000)

UPDATE analytics
SET revenue = (revenue/1000000)


UPDATE all_sessions
SET totaltransactionrevenue = (totaltransactionrevenue/1000000)

```
- Remove/trim excess spaces
```
UPDATE products
SET name = TRIM(both ' ' FROM name)
```

- Changed datatype to integer
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
- Changed to date format
```
ALTER TABLE analytics
ALTER COLUMN timeonsite TYPE interval
USING (timeonsite || ' minutes')::interval;
```
- Changed to date and time format 
```
UPDATE analytics
SET "visitStartTime" = TO_CHAR(TO_TIMESTAMP("visitStartTime"::bigint), 'YYYY-MM-DD HH24:MI:SS')
```
