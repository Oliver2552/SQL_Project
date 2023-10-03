Question 1: What is the average time spent on the site by country?

SQL Queries: 
```
SELECT
    country,
    AVG(timeonsite_formatted) AS timeonsite
FROM
	cleaned_all_sessions
GROUP BY
	country;
```
Answer: 

with 129 rows it would take too long to list them but with the highest average time, visitors from Peru spend 13 minutes and 39 seconds and at the lowest average, visitors from Brunei spend about 3 seconds!


Question 2: What are the top 5 most *ordered* products? include the productsku and name.

SQL Queries:
```
SELECT
	productsku,
	name,
	orderedquantity
FROM
	products
GROUP BY
	productsku
ORDER BY
	orderedquantity DESC
LIMIT 5;
```
Answer:
Kickball, 22 oz water battle (two different ones) sunglasses and spiral Journal with pen, with 15,170, 10,075, 8942, 4204 and 3896 units ordered, respectively.


Question 3: Which products (by name) have the top 3 shortest time when it comes to restocking them?


SQL Queries:
```
SELECT
	name,
	restockingleadtime
FROM
	products
GROUP BY
	name,
	restockingleadtime
ORDER BY
	restockingleadtime ASC
LIMIT 3;
```
Answer:
  
PC gaming speakers, badge holder and twill cap with 1, 2 and 2 days, respectively.




