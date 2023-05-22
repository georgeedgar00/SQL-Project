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



Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







