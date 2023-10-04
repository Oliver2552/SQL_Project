Question 1: What are the top 10 countries when it comes to average time spent on the site? Rank them in order of most time spend to least.


SQL Queries: 
```
SELECT
    country,
    AVG(timeonsite_formatted) AS avg_timeonsite,
    RANK() OVER (ORDER BY AVG(timeonsite_formatted) DESC) AS rank_by
FROM
    cleaned_all_sessions
GROUP BY
    country
HAVING
    AVG(timeonsite_formatted) IS NOT NULL
ORDER BY
    avg_timeonsite DESC
LIMIT 10;
	
```
Answer: 

The top 10 countries with the highest average time spent on the site, ranked from most to least, are:

Peru: 14 minutes and 19 seconds
Nigeria: 12 minutes and 32 seconds
Tunisia: 12 minutes and 6 seconds
Slovenia: 11 minutes and 36 seconds
Kenya: 9 minutes and 32 seconds
El Salvador: 9 minutes and 28 seconds
Jersey: 8 minutes and 38 seconds
Morocco: 7 minutes and 37 seconds
Guatemala: 7 minutes and 24 seconds
Belarus: 7 minutes and 21 seconds.

The United States sits at 53rd with an average of 3 minutes and 37 seconds, does this inform about the US spending habits? Also, why is it that Peru, who spends the most time, fail to convert to buyers - would need more data to understand why.. (eg. interenet connectivity, cursor heatmap etc...)


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




