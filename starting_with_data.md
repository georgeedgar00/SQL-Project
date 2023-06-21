Question 1: What three items were the most ordered

SQL Queries:
```
SELECT sr."productSKU", sr.total_ordered, p.name
FROM sales_report sr
JOIN products p 
ON sr."productSKU"  = p."SKU"
ORDER BY total_ordered DESC
LIMIT 3

```

Answer: Ballpoint LED Light Pen at 456 orders, 17oz Stainless Steel Sport Bottle at 334 orders and Leatherette Journal at 319 orders



Question 2: What products do we have the most in stock

SQL Queries:
```
SELECT 
	als."v2ProductName" AS "Name of the Product", sr."stockLevel"
FROM all_sessions als
JOIN products p
ON als."productSKU" = p."SKU"
JOIN sales_report sr
ON p."SKU" = sr."productSKU"
GROUP BY "v2ProductName", sr."stockLevel"
ORDER BY sr."stockLevel" DESC
LIMIT 10;
```

Answer: Google 22 oz Water Bottle at 19678 units, 22 oz Mini Mountain Bottle at 15607 units and Google 22 oz Water Bottle at 15607 units



Question 3: How many products have been viewed over 100 times?

SQL Queries:
```
SELECT 
    "v2ProductName", 
    "Unique Visits"
FROM (
    SELECT 
        "v2ProductName", 
        COUNT(DISTINCT("fullVisitorId")) AS "Unique Visits"
    FROM all_sessions
    GROUP BY "v2ProductName"
) subquery
WHERE "Unique Visits" > 100
ORDER BY "Unique Visits";
```

Answer: 27 products have been viewed over 100 times

Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
