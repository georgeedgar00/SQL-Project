Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:



Answer:
**Query for Country**
```
SELECT 
    country, 
    SUM("totalTransactionRevenue") AS transaction_revenue
FROM all_sessions
WHERE "totalTransactionRevenue" IS NOT Null
GROUP BY country
ORDER BY transaction_revenue DESC

```
**Query for City**
```
WITH city_ntr AS (
	SELECT 
        city, 
        "totalTransactionRevenue"
	FROM all_sessions
	WHERE city NOT IN ('null', 'not available in demo dataset'))

SELECT 
    city,
    SUM("totalTransactionRevenue") AS transaction_revenue
FROM city_ntr
WHERE "totalTransactionRevenue" IS NOT Null
GROUP BY city
ORDER BY transaction_revenue DESC
```
### Answer: 
The cities with the highest level of transaction revenue are San Francisco, Sunnyvale and Atlanta. The countries with the highest level of transaction revenue are the United States, Israel and Australia.



**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
**Query for City**
```
SELECT 
	CASE
		WHEN als.city = '(not set)' THEN Null
		WHEN als.city = 'not available in demo dataset' THEN Null
	 	ELSE city
	 END,
	ROUND(AVG(p."orderedQuantity")) AS average
FROM products p
JOIN all_sessions als
ON p."SKU" = als."productSKU"
GROUP BY als.city
ORDER BY average
```

**Query for Country**
```
SELECT 
	CASE
		WHEN als.country = '(not set)' THEN Null
		ELSE country
	END,
	ROUND(AVG(p."orderedQuantity")) AS average
FROM products p
JOIN all_sessions als
ON p."SKU" = als."productSKU"
GROUP BY als.country
ORDER BY average
```


### Answer:
There are 252 cities on the list, with 2 cities from the United States in the top 5, Council Bluffs (USA), Cork (Ireland), Bellflower(USA). Cote d'Ivoire and Montenegro round out the top 5 without any specific city given.

There are 134 countries on the list, with Montenegro, Mali, Papua New Guinea, Reunion, and Georgia in the top 5 in that order. There are 2 countries Iceland and Rwanda at the bottom with 0 average orders.





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







