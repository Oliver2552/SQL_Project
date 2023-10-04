Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

```
SELECT
    cals.country,
    cals.cleaned_city,
    SUM(sbs.total_ordered * ca.cleaned_unit_price) AS total_transaction_revenue
FROM
    sales_by_sku AS sbs
JOIN
    cleaned_all_sessions AS cals USING(productsku)
JOIN
    cleaned_analytics AS ca USING(visitid)
GROUP BY
    country,
    cleaned_city
ORDER BY
    total_transaction_revenue DESC
LIMIT 10;
```

Answer: the top 10 countries are United states with respective cities, except for 8th place which is the UK. The number one transaction revenue was $13,469,795. 

NOTE: we used 'cleaned_unit_price' rather than 'cleaned_productprice' as the revenue figures are higher with cleaned_unit_price - logically it would seem that productprice is appropriate as customers buy 'products' rather than 'units' - but in this case, why would the website lose money on sold products..


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
```
SELECT
	cals.country,
	cals.cleaned_city,
	ROUND(AVG(sbs.total_ordered),2) AS avg_products_ordered
FROM
	cleaned_all_sessions AS cals
JOIN
	sales_by_sku AS sbs USING(productsku)
GROUP BY
	country, cleaned_city
ORDER BY
	avg_products_ordered desc;
```

Answer:

The highest avg number of products ordered are from Czech Republic and Saudi Arabia with 319 products ordered and in third place is the United States with ~250.


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

```
SELECT
    cals.country,
	cals.cleaned_city,
    cals.v2productcategory,
    COUNT(*) AS category_count
FROM
    cleaned_all_sessions AS cals
GROUP BY
    cals.country,
	cals.cleaned_city,
    cals.v2productcategory
ORDER BY
    category_count DESC;
```


Answer:

Products' 'youtube' and 'mens t-shirts' are predominantly bought by US customers... 613 and 435, respectively.



**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

```
WITH ranked_products AS (
    SELECT
        cals.country,
        cals.cleaned_city,
        cals.v2productcategory,
        cals.productsku,
        sbs.total_ordered,
        ROW_NUMBER() OVER(PARTITION BY cals.country, cals.city ORDER BY 		sbs.total_ordered DESC) AS rank
    FROM
        cleaned_all_sessions AS cals
    JOIN
        sales_by_sku AS sbs USING(productsku)
)

SELECT
    country,
    cleaned_city,
    v2productcategory,
    productsku AS top_product,
    total_ordered
FROM
    ranked_products
WHERE
    rank = 1
ORDER BY
    country,
    cleaned_city;
```

Answer:

too many country and city combination to give a single answer, the resultant table gives 356 rows.


**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

```
SELECT
    cals.country,
    cals.cleaned_city,
    SUM(ca.cleaned_unit_price * sbs.total_ordered) AS total_revenue
FROM
    cleaned_all_sessions AS cals
JOIN
    sales_by_sku AS sbs USING(productsku)
JOIN
    cleaned_analytics AS ca USING(visitid)
GROUP BY
    cals.country, 
    cals.cleaned_city
ORDER BY
    total_revenue DESC;
```
```
SELECT
    SUM(ca.cleaned_unit_price * sbs.total_ordered) AS total_revenue
FROM
    cleaned_all_sessions AS cals
JOIN
    sales_by_sku AS sbs USING(productsku)
JOIN
    cleaned_analytics AS ca USING(visitid)
GROUP BY
    cals.country,
    cals.cleaned_city
ORDER BY
    total_revenue DESC
LIMIT 5;

```

Answer:

The first query shows all the revenue broken down by country and city, while the second query sums up all the revenues to show a total revenue of ~$50m. 

In terms of impact, we can see from the first query that the first 5 rows, ordered in descending order show that just over half of the total revenue comes from the United States, more specifically ~$26m.

This would make the United States as a huge and important marketshare for this website.




