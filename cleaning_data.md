What issues will you address by cleaning the data?
 I will be putting columns in their correct data type, 




Queries:
Below, provide the SQL queries you used to clean your data.

-Firstly, I updated the unit_price price column by dividing it by 1,000,000
```
  UPDATE analytics
SET unit_price = unit_price / 1000000;

```
-Then I used the DROP function to remove null values/empty columns from tables
```
ALTER TABLE all_sessions DROP COLUMN itemquantity;

ALTER TABLE all_sessions DROP COLUMN itemrevenue;

ALTER TABLE all_sessions DROP COLUMN searchkeyword;

ALTER TABLE analytics DROP COLUMN userid;
```
