Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

- Country query:
```
SELECT 
    country, 
    SUM("totalTransactionRevenue") AS transaction_revenue
FROM all_sessions
WHERE "totalTransactionRevenue" IS NOT Null
GROUP BY country
ORDER BY transaction_revenue DESC

```

- City query:
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

- City query:
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

- Country query:
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

- Country query
```WITH all_sessions_clean AS (
    SELECT 
        CASE
            WHEN als.country = '(not set)' THEN NULL
            ELSE als.country
        END AS country,
        CASE
            WHEN als."v2ProductCategory" = '(not set)' THEN NULL
            ELSE als."v2ProductCategory"
        END AS v2productcategory,
        als."productSKU"
    FROM all_sessions AS als
),
total_orders_by_category AS (
    SELECT 
        country, 
        v2productcategory, 
        SUM(p."orderedQuantity") AS ordered_quantity_sum,
        ROW_NUMBER() OVER (PARTITION BY country ORDER BY SUM(p."orderedQuantity") DESC) AS row_num
    FROM all_sessions_clean AS alc
    JOIN products AS p ON p."SKU" = alc."productSKU"
    WHERE 
        alc.v2productcategory IS NOT NULL
        AND alc.country IS NOT NULL
        AND p."orderedQuantity" > 0
    GROUP BY alc.country, alc.v2productcategory
)
SELECT 
    country,
    v2productcategory,
    ordered_quantity_sum"
FROM total_orders_by_category
WHERE row_num = 1
ORDER BY ordered_quantity_sum DESC
LIMIT 10;
```

- City query
```
WITH all_sessions_clean AS (
    SELECT 
        CASE
            WHEN als.city = '(not set)' THEN NULL
			WHEN als.city = 'not available in demo dataset' THEN NULL
            ELSE als.city
        END AS city,
        CASE
            WHEN als."v2ProductCategory" = '(not set)' THEN NULL
            ELSE als."v2ProductCategory"
        END AS v2productcategory,
        als."productSKU"
    FROM all_sessions AS als
),
total_orders_by_category AS (
    SELECT 
        city, 
        v2productcategory, 
        SUM(p."orderedQuantity") AS ordered_quantity_sum,
        ROW_NUMBER() OVER (PARTITION BY city ORDER BY SUM(p."orderedQuantity") DESC) AS row_num
    FROM all_sessions_clean AS alc
    JOIN products AS p ON p."SKU" = alc."productSKU"
    WHERE 
        alc.v2productcategory IS NOT NULL
        AND alc.city IS NOT NULL
        AND p."orderedQuantity" > 0
    GROUP BY alc.city, alc.v2productcategory
)
SELECT 
    city,
    v2productcategory,
    ordered_quantity_sum
FROM total_orders_by_category
WHERE row_num = 1
ORDER BY ordered_quantity_sum DESC
LIMIT 10;
```


Answer: The most popular product group was 'Home/Shop by Brand/YouTube/' for countries.
The most popular category of products was 'Home/Nest/Nest-USA/' for cities.





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

- Country query:
```
WITH all_sessions_clean AS (
    SELECT 
        CASE
            WHEN als.country = '(not set)' THEN NULL
            ELSE als.country
        END AS country,
        CASE
            WHEN als."v2ProductName" = '(not set)' THEN NULL
            ELSE als."v2ProductName"
        END AS v2productname,
        als."productSKU"
    FROM all_sessions AS als
),
total_orders_by_product AS (
    SELECT 
        country, 
        v2productname, 
        SUM(p."orderedQuantity") AS ordered_quantity_sum,
        ROW_NUMBER() OVER (PARTITION BY country ORDER BY SUM(p."orderedQuantity") DESC) AS row_num
    FROM all_sessions_clean AS alc
    JOIN products AS p ON p."SKU" = alc."productSKU"
    WHERE 
        alc.v2productname IS NOT NULL
        AND alc.country IS NOT NULL
        AND p."orderedQuantity" > 0
    GROUP BY alc.country, alc.v2productname
)
SELECT 
    country,
    v2productname,
    ordered_quantity_sum
FROM total_orders_by_product
WHERE row_num = 1
ORDER BY ordered_quantity_sum DESC
LIMIT 10;
```

- City query:
```
WITH all_sessions_clean AS (
    SELECT 
        CASE
            WHEN als.city = '(not set)' THEN NULL
            ELSE als.city
        END AS city,
        CASE
            WHEN als."v2ProductName" = '(not set)' THEN NULL
            ELSE als."v2ProductName"
        END AS v2productname,
        als."productSKU"
    FROM all_sessions AS als
),
total_orders_by_product AS (
    SELECT 
        city, 
        v2productname, 
        SUM(p."orderedQuantity") AS ordered_quantity_sum,
        ROW_NUMBER() OVER (PARTITION BY city ORDER BY SUM(p."orderedQuantity") DESC) AS row_num
    FROM all_sessions_clean AS alc
    JOIN products AS p ON p."SKU" = alc."productSKU"
    WHERE 
        alc.v2productname IS NOT NULL
        AND alc.city IS NOT NULL
        AND p."orderedQuantity" > 0
    GROUP BY alc.city, alc.v2productname
)
SELECT 
    city,
    v2productname,
    ordered_quantity_sum
FROM total_orders_by_product
WHERE row_num = 1
ORDER BY ordered_quantity_sum DESC
LIMIT 10;
```

Answer:
'YouTube Custom Decals' is the top-selling product in 70% of countries. While 'Google Kick Ball' sold the most based on combined quantity.
'Nest Cam Indoor Security Camera - USA' is the top-selling product in most cities. While 'Google Kick Balll' sold the most based on combined quantity.





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

- Country query:
```SELECT 
    country, 
    SUM("totalTransactionRevenue") AS "Transaction Revenue",
	ROUND(SUM("totalTransactionRevenue")/SUM(SUM("totalTransactionRevenue")) OVER () * 100, 2) AS "Transaction Revenue Rate by Country"
FROM all_sessions
WHERE "totalTransactionRevenue" IS NOT Null
GROUP BY country
ORDER BY "Transaction Revenue" DESC;
```

- City query:
```
SELECT 
    city , 
    SUM("totalTransactionRevenue") AS "Transaction Revenue",
	ROUND(SUM("totalTransactionRevenue")/SUM(SUM("totalTransactionRevenue")) OVER () * 100, 2) AS "Transaction Revenue Rate by City"
FROM all_sessions
WHERE "totalTransactionRevenue" IS NOT Null AND city != 'not available in demo dataset'
GROUP BY city
ORDER BY "Transaction Revenue" DESC;
```


Answer: The United States generated the most impact revenue. Customers from the United States are generating 92% of the revenue.
Cities in the United States like San Francisco which is responsible for 19% of the revenue, followed by Sunnyvale (12%) and Atlanta (10%) generated the most impact revenue.







