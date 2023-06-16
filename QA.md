What are your risk areas? Identify and describe them.

- Duplicate values in the dataset, especially in the analytics table.
- Empty and null values in multiple columns.
- Duplicate columns within the same table
- Appropriate data types were not assigned to appropriate columns.
- There was no link between tables because primary and secondary keys were not established.


QA Process:
Describe your QA process and include the SQL queries used to execute it.

- Checked for columns with Null values where data needs to be complete, as well as empty columns
```
SELECT *
FROM fullvisitorId
WHERE fullvisitorId IS Null
```

- Checked if there are any duplicate values
```
WITH cte_city AS (
	SELECT 
        city, 
        totalTransactionRevenue
	FROM all_sessions
	WHERE city NOT IN ('(not set)', 'not available in demo dataset'))

SELECT 
    city,
    SUM(totaltTansactionRevenue) AS transaction_revenue
FROM cte_city
WHERE totalTransactionRevenue IS NOT Null
GROUP BY city
ORDER BY transaction_revenue DESC;
```

Checked with output results from this query

```
SELECT 
    city,
    SUM(totalTransactionRevenue) AS transaction_revenue
FROM all_sessions
GROUP BY city
ORDER BY transaction_revenue DESC;
```
- Checked for consistency, changed data types, formatting

```
UPDATE analytics
SET "visitStartTime" = TO_CHAR(TO_TIMESTAMP("visitStartTime"::bigint), 'YYYY-MM-DD HH24:MI:SS')
```

### If I had a bit more time, I would have also conducted the following steps:
- Performed data validation with business rules or reference data, verifying that the data aligns with expected criteria.
- Formatting, and decimal placements 
