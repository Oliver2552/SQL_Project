# Identifying and Dealing with Risk Areas


## Known Relevant Risks
With respect to the 5 tables provided (sales_report, sales_by_sku, products, analytics and all_sessions), the main risk areas are, ensuring duplicates are removed, missing data is dealt with accordingly, whether that be through ommision of relevant columns, and outliers are scouted for.


## QA Process:

### 'sales_report' table:

With only 8 columns, are main concern was ensuring that 'productsku', the primary key was truly unqiue. Printing out the entire table using, would show us that this table is 454 rows long and so we'd want the distinct count to be 454 for 'productsku':

```
SELECT
	*
FROM
	sales_report;
```
We also did a count for all unqiues and NULL values for all columns in the table using the below. As hoped for, productsku showed as having 454 unique values and thus can be the primary key. 

All columns had 0 NULL values except for ratio with 78 'NULL' occurnences.

```
SELECT
    'productsku' AS column_name,
    COUNT(DISTINCT productsku) AS unique_values,
    COUNT(CASE WHEN productsku IS NULL THEN 1 ELSE NULL END) AS null_values
FROM
    sales_report
	
UNION ALL

SELECT
    'total_ordered' AS column_name,
    COUNT(DISTINCT total_ordered) AS unique_values,
    COUNT(CASE WHEN total_ordered IS NULL THEN 1 ELSE NULL END)
FROM
    sales_report
	
UNION ALL

SELECT
    'name' AS column_name,
    COUNT(DISTINCT name) AS unique_values,
    COUNT(CASE WHEN name IS NULL THEN 1 ELSE NULL END)
FROM
    sales_report
	
UNION ALL

SELECT
    'stocklevel' AS column_name,
    COUNT(DISTINCT stocklevel) AS unique_values,
    COUNT(CASE WHEN stocklevel IS NULL THEN 1 ELSE NULL END)
FROM
    sales_report
	
UNION ALL

SELECT
    'restockingleadtime' AS column_name,
    COUNT(DISTINCT restockingleadtime) AS unique_values,
    COUNT(CASE WHEN restockingleadtime IS NULL THEN 1 ELSE NULL END)
FROM
    sales_report
	
UNION ALL

SELECT
    'sentimentscore' AS column_name,
    COUNT(DISTINCT sentimentscore) AS unique_values,
    COUNT(CASE WHEN sentimentscore IS NULL THEN 1 ELSE NULL END)
FROM
    sales_report
	
UNION ALL

SELECT
    'sentimentmagnitude' AS column_name,
    COUNT(DISTINCT sentimentmagnitude) AS unique_values,
    COUNT(CASE WHEN sentimentmagnitude IS NULL THEN 1 ELSE NULL END)
FROM
    sales_report
	
UNION ALL

SELECT
    'ratio' AS column_name,
    COUNT(DISTINCT ratio) AS unique_values,
    COUNT(CASE WHEN ratio IS NULL THEN 1 ELSE NULL END)
FROM
    sales_report;
```

### 'sales_by_sku' table:

With only 2 columns - 'productsku' and 'total_orderd', our only main concern was that of the 462 rows-long this table was, as found by the code below,

```
SELECT
	*
FROM
	sales_by_sku;
```

that the 'productsku' column was unique and much like with 'sales_report', we checked for that with the following query:
```
SELECT
    'productsku' AS column_name, 
    COUNT(DISTINCT productsku) AS unique_values,
    COUNT(CASE WHEN productsku IS NULL THEN 1 ELSE NULL END) AS null_values
FROM
    sales_by_sku -- unqiue with 462 values and 0 nulls
	
UNION ALL

SELECT
    'total_ordered' AS column_name,
    COUNT(DISTINCT total_ordered) AS unique_values,
    COUNT(CASE WHEN total_ordered IS NULL THEN 1 ELSE NULL END)
FROM
    sales_by_sku; -- not unique with unique 60 values and 0 nulls
```

### 'products' table:

Housing 7 columns and 1092 rows long, as shown be the code below, 

```
SELECT
    *
FROM
    products;
```

once again, our main concern is ensuring that the 'productsku' column, our unique identifier (primary key) is truly unique, whereby a unique count should be equal to the tables total rows. After running the below query, we can see that productsku is entirely unique with 1092 unique values, in addition, we have very close to no NULL values with only columns' 'sentimentscore' and 'sentimentmagnitude' showing 1 NULL value each.

```
SELECT
    'productsku' AS column_name, 
    COUNT(DISTINCT productsku) AS unique_values,
    COUNT(CASE WHEN productsku IS NULL THEN 1 ELSE NULL END) AS null_values
FROM
    products -- this tables unqiue identifier with 1092 unique values
	
UNION ALL

SELECT
    'name' AS column_name,
    COUNT(DISTINCT name) AS unique_values,
    COUNT(CASE WHEN name IS NULL THEN 1 ELSE NULL END)
FROM
    products 
	
UNION ALL

SELECT
    'orderedquantity' AS column_name,
    COUNT(DISTINCT orderedquantity) AS unique_values,
    COUNT(CASE WHEN orderedquantity IS NULL THEN 1 ELSE NULL END)
FROM
    products 

UNION ALL

SELECT
    'stocklevel' AS column_name,
    COUNT(DISTINCT stocklevel) AS unique_values,
    COUNT(CASE WHEN stocklevel IS NULL THEN 1 ELSE NULL END)
FROM
    products

UNION ALL

SELECT
    'restockingleadtime' AS column_name,
    COUNT(DISTINCT restockingleadtime) AS unique_values,
    COUNT(CASE WHEN restockingleadtime IS NULL THEN 1 ELSE NULL END)
FROM
    products

UNION ALL

SELECT
    'sentimentscore' AS column_name,
    COUNT(DISTINCT sentimentscore) AS unique_values,
    COUNT(CASE WHEN sentimentscore IS NULL THEN 1 ELSE NULL END)
FROM
    products -- one observation with a NULL value

UNION ALL

SELECT
    'sentimentmagnitude' AS column_name,
    COUNT(DISTINCT sentimentmagnitude) AS unique_values,
    COUNT(CASE WHEN sentimentmagnitude IS NULL THEN 1 ELSE NULL END)
FROM
    products; -- one observation with a NULL value
```

### 'analytics' table:

Our largest table, containing 4,301,122 rows, as evidenced below, 

```
SELECT
    *
FROM
    analytics;
```

this one proved to be messier than the prior - present were several columns containing most, if not all, NULL values, two time colums, 'visitstarttime' and 'timeonsite' were in formats that appeared to be seconds so they will require transformation and column 'unit_price' needed to be divided by 1,000,000 to attain its true price values. The below does a unique and NULL count for all columns.

In regards to identifying the primary key, initially it was between the 'visitid' and 'fullvisitorid' however, upon further examination and with results of the below query, visitid seems like the only reasonable id as each visit to the site is represented by its own id where each visitor will retain their id, no matter how many times they return.

```
SELECT
    'visitnumber' AS column_name, 
    COUNT(DISTINCT visitnumber) AS unique_values,
    COUNT(CASE WHEN visitnumber IS NULL THEN 1 ELSE NULL END) AS null_values
FROM
    analytics 
	
UNION ALL

SELECT
    'visitid' AS column_name,
    COUNT(DISTINCT visitid) AS unique_values,
    COUNT(CASE WHEN visitid IS NULL THEN 1 ELSE NULL END)
FROM
    analytics 
	
UNION ALL

SELECT
    'visitstarttime' AS column_name,
    COUNT(DISTINCT visitstarttime) AS unique_values,
    COUNT(CASE WHEN visitstarttime IS NULL THEN 1 ELSE NULL END)
FROM
    analytics 

UNION ALL

SELECT
    'date' AS column_name,
    COUNT(DISTINCT date) AS unique_values,
    COUNT(CASE WHEN date IS NULL THEN 1 ELSE NULL END)
FROM
    analytics

UNION ALL

SELECT
    'fullvisitorid' AS column_name,
    COUNT(DISTINCT fullvisitorid) AS unique_values,
    COUNT(CASE WHEN fullvisitorid IS NULL THEN 1 ELSE NULL END)
FROM
    analytics

UNION ALL

SELECT
    'userid' AS column_name,
    COUNT(DISTINCT userid) AS unique_values,
    COUNT(CASE WHEN userid IS NULL THEN 1 ELSE NULL END) -- 4,301,122 / 4,301,122 (ALL) rows as NULL
FROM
    analytics

UNION ALL

SELECT
    'channelgrouping' AS column_name,
    COUNT(DISTINCT channelgrouping) AS unique_values,
    COUNT(CASE WHEN channelgrouping IS NULL THEN 1 ELSE NULL END)
FROM
    analytics

UNION ALL

SELECT
    'socialengagementtype' AS column_name,
    COUNT(DISTINCT socialengagementtype) AS unique_values,
    COUNT(CASE WHEN socialengagementtype IS NULL THEN 1 ELSE NULL END)
FROM
    analytics

UNION ALL

SELECT
    'units_sold' AS column_name,
    COUNT(DISTINCT units_sold) AS unique_values,
    COUNT(CASE WHEN units_sold IS NULL THEN 1 ELSE NULL END) -- 4,205,975 / 4,301,122 rows as NULL
FROM
    analytics

UNION ALL

SELECT
    'pageviews' AS column_name,
    COUNT(DISTINCT pageviews) AS unique_values,
    COUNT(CASE WHEN pageviews IS NULL THEN 1 ELSE NULL END)
FROM
    analytics

UNION ALL

SELECT
    'timeonsite' AS column_name,
    COUNT(DISTINCT timeonsite) AS unique_values,
    COUNT(CASE WHEN timeonsite IS NULL THEN 1 ELSE NULL END)
FROM
    analytics

UNION ALL

SELECT
    'bounces' AS column_name,
    COUNT(DISTINCT bounces) AS unique_values,
    COUNT(CASE WHEN bounces IS NULL THEN 1 ELSE NULL END) -- 3,826,283 / 4,301,122 rows as NULl
FROM
    analytics

UNION ALL

SELECT
    'revenue' AS column_name,
    COUNT(DISTINCT revenue) AS unique_values,
    COUNT(CASE WHEN revenue IS NULL THEN 1 ELSE NULL END) -- 4,285,767 / 4,301,122 rows as NULL
FROM
    analytics

UNION ALL

SELECT
    'unit_price' AS column_name,
    COUNT(DISTINCT unit_price) AS unique_values,
    COUNT(CASE WHEN unit_price IS NULL THEN 1 ELSE NULL END)
FROM
    analytics;
```

We also, did a search any outliers in the 'unit_price' column and we found no values below 0 and our highest value was 995 which seems reasonable, using the query below:

```
SELECT
    MAX(unit_price/1000000), -- 995,
    MIN(unit_price/1000000) -- 0
FROM
    analytics;
```
After performing the cleaning as in 'cleaning_data.md', we performed the same union table as above but with the changes and is seen below:
```
SELECT
    'visitnumber' AS column_name, 
    COUNT(DISTINCT visitnumber) AS unique_values,
	COUNT(CASE WHEN visitnumber IS NULL THEN 1 ELSE NULL END) AS null_values
FROM
    cleaned_analytics 
	
UNION ALL

SELECT
    'visitid' AS column_name,
    COUNT(DISTINCT visitid) AS unique_values,
	COUNT(CASE WHEN visitid IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_analytics 
	
UNION ALL

SELECT
    'visit_time' AS column_name,
    COUNT(DISTINCT visit_time) AS unique_values,
	COUNT(CASE WHEN visit_time IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_analytics 

UNION ALL

SELECT
    'date' AS column_name,
    COUNT(DISTINCT date) AS unique_values,
	COUNT(CASE WHEN date IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_analytics

UNION ALL

SELECT
    'fullvisitorid' AS column_name,
    COUNT(DISTINCT fullvisitorid) AS unique_values,
	COUNT(CASE WHEN fullvisitorid IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_analytics

UNION ALL

SELECT
    'channelgrouping' AS column_name,
    COUNT(DISTINCT channelgrouping) AS unique_values,
	COUNT(CASE WHEN channelgrouping IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_analytics

UNION ALL

SELECT
    'socialengagementtype' AS column_name,
    COUNT(DISTINCT socialengagementtype) AS unique_values,
	COUNT(CASE WHEN socialengagementtype IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_analytics

UNION ALL

SELECT
    'pageviews' AS column_name,
    COUNT(DISTINCT pageviews) AS unique_values,
	COUNT(CASE WHEN pageviews IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_analytics

UNION ALL

SELECT
    'timeonsite_clean' AS column_name,
    COUNT(DISTINCT timeonsite_clean) AS unique_values,
	COUNT(CASE WHEN timeonsite_clean IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_analytics
```
It should be noted that 'timeonsite' does still present with ~477k NULL rows and while it is a lot, there are 4.3m rows in total. We considered replacing them with another value but wasnt sure what to replace them with.. That is something that deserves more attention with more data - with more information, the question, why are they blank needs answering.



### 'all_sessions' table:

With 15,134 rows in total, as seen with the  below query, 

```
SELECT
    *
FROM
    all_sessions;
```

this table appears to be an amalgamation of the prior (or atleast an attempt to..) - several columns present as entirely (or close to) filled with NULL values, questioning their presence and whether they should be dropped.

The 'time' and 'timeonsite' columns, like the prior needs to be converted from what appears to be seconds to a timestamp. 

The 'city' column has many instances of missing values.

The below does a unique and NULL check on all the columns:

```
SELECT
    'fullvisitorid' AS column_name, 
    COUNT(DISTINCT fullvisitorid) AS unique_values,
    COUNT(CASE WHEN fullvisitorid IS NULL THEN 1 ELSE NULL END) AS null_values
FROM
    all_sessions 
	
UNION ALL

SELECT
    'channelgrouping' AS column_name,
    COUNT(DISTINCT channelgrouping) AS unique_values,
    COUNT(CASE WHEN channelgrouping IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions 
	
UNION ALL

SELECT
    'time' AS column_name,
    COUNT(DISTINCT time) AS unique_values,
    COUNT(CASE WHEN time IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'country' AS column_name,
    COUNT(DISTINCT country) AS unique_values,
    COUNT(CASE WHEN country IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'city' AS column_name,
    COUNT(DISTINCT city) AS unique_values,
    COUNT(CASE WHEN city IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'totaltransactionrevenue' AS column_name,
    COUNT(DISTINCT totaltransactionrevenue) AS unique_values,
    COUNT(CASE WHEN totaltransactionrevenue IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'transactions' AS column_name,
    COUNT(DISTINCT transactions) AS unique_values,
    COUNT(CASE WHEN transactions IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'timeonsite' AS column_name,
    COUNT(DISTINCT timeonsite) AS unique_values,
    COUNT(CASE WHEN timeonsite IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'pageviews' AS column_name,
    COUNT(DISTINCT pageviews) AS unique_values,
    COUNT(CASE WHEN pageviews IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'sessionqualitydim' AS column_name,
    COUNT(DISTINCT sessionqualitydim) AS unique_values,
    COUNT(CASE WHEN sessionqualitydim IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'date' AS column_name,
    COUNT(DISTINCT date) AS unique_values,
    COUNT(CASE WHEN date IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'visitid' AS column_name,
    COUNT(DISTINCT visitid) AS unique_values,
    COUNT(CASE WHEN visitid IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'type' AS column_name,
    COUNT(DISTINCT type) AS unique_values,
    COUNT(CASE WHEN type IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'productrefundamount' AS column_name,
    COUNT(DISTINCT productrefundamount) AS unique_values,
    COUNT(CASE WHEN productrefundamount IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions
	
UNION ALL

SELECT
    'productquantity' AS column_name,
    COUNT(DISTINCT productquantity) AS unique_values,
    COUNT(CASE WHEN productquantity IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'productprice' AS column_name,
    COUNT(DISTINCT productprice) AS unique_values,
    COUNT(CASE WHEN productprice IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'productrevenue' AS column_name,
    COUNT(DISTINCT productrevenue) AS unique_values,
    COUNT(CASE WHEN productrevenue IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'productsku' AS column_name,
    COUNT(DISTINCT productsku) AS unique_values,
    COUNT(CASE WHEN productsku IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'v2productname' AS column_name,
    COUNT(DISTINCT v2productname) AS unique_values,
    COUNT(CASE WHEN v2productname IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'v2productcategory' AS column_name,
    COUNT(DISTINCT v2productcategory) AS unique_values,
    COUNT(CASE WHEN v2productcategory IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'productvariant' AS column_name,
    COUNT(DISTINCT productvariant) AS unique_values,
    COUNT(CASE WHEN productvariant IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions
	
UNION ALL

SELECT
    'currencycode' AS column_name,
    COUNT(DISTINCT currencycode) AS unique_values,
    COUNT(CASE WHEN currencycode IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'itemquantity' AS column_name,
    COUNT(DISTINCT itemquantity) AS unique_values,
    COUNT(CASE WHEN itemquantity IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'itemrevenue' AS column_name,
    COUNT(DISTINCT itemrevenue) AS unique_values,
    COUNT(CASE WHEN itemrevenue IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'transactionrevenue' AS column_name,
    COUNT(DISTINCT transactionrevenue) AS unique_values,
    COUNT(CASE WHEN transactionrevenue IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'transactionid' AS column_name,
    COUNT(DISTINCT transactionid) AS unique_values,
    COUNT(CASE WHEN transactionid IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'pagetitle' AS column_name,
    COUNT(DISTINCT pagetitle) AS unique_values,
    COUNT(CASE WHEN pagetitle IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'searchkeyword' AS column_name,
    COUNT(DISTINCT searchkeyword) AS unique_values,
    COUNT(CASE WHEN searchkeyword IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'pagepathlevel1' AS column_name,
    COUNT(DISTINCT pagepathlevel1) AS unique_values,
    COUNT(CASE WHEN pagepathlevel1 IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'ecommerceaction_type' AS column_name,
    COUNT(DISTINCT ecommerceaction_type) AS unique_values,
    COUNT(CASE WHEN ecommerceaction_type IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'ecommerceaction_step' AS column_name,
    COUNT(DISTINCT ecommerceaction_step) AS unique_values,
    COUNT(CASE WHEN ecommerceaction_step IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions

UNION ALL

SELECT
    'ecommerceaction_option' AS column_name,
    COUNT(DISTINCT ecommerceaction_option) AS unique_values,
    COUNT(CASE WHEN ecommerceaction_option IS NULL THEN 1 ELSE NULL END)
FROM
    all_sessions;
```

Post cleaning of the all_sessions dataset we also did a union table check of all uniques and null values too make sure our changes in the cleaning_data.md took effect and they did. That being said, like in all_sessions, we left timeonsite_formatted in despite its presence of NULL values ~3,0000 of a total of ~15,000 rows. We didnt know what to replace them with as its unclear why theyre null in the first place. Below is the check:

```
SELECT
    'fullvisitorid' AS column_name, 
    COUNT(DISTINCT fullvisitorid) AS unique_values,
	COUNT(CASE WHEN fullvisitorid IS NULL THEN 1 ELSE NULL END) AS null_values
FROM
    cleaned_all_sessions 
	
UNION ALL

SELECT
    'channelgrouping' AS column_name,
    COUNT(DISTINCT channelgrouping) AS unique_values,
	COUNT(CASE WHEN channelgrouping IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_all_sessions 
	
UNION ALL

SELECT
    'time_formatted' AS column_name,
    COUNT(DISTINCT time_formatted) AS unique_values,
	COUNT(CASE WHEN time_formatted IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_all_sessions

UNION ALL

SELECT
    'country' AS column_name,
    COUNT(DISTINCT country) AS unique_values,
	COUNT(CASE WHEN country IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_all_sessions

UNION ALL

SELECT
    'productsku' AS column_name,
    COUNT(DISTINCT productsku) AS unique_values,
	COUNT(CASE WHEN productsku IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_all_sessions

UNION ALL

SELECT
    'cleaned_city' AS column_name,
    COUNT(DISTINCT cleaned_city) AS unique_values,
	COUNT(CASE WHEN cleaned_city IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_all_sessions

UNION ALL

SELECT
    'v2productname' AS column_name,
    COUNT(DISTINCT v2productname) AS unique_values,
	COUNT(CASE WHEN v2productname IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_all_sessions

UNION ALL

SELECT
    'v2productcategory' AS column_name,
    COUNT(DISTINCT v2productcategory) AS unique_values,
	COUNT(CASE WHEN v2productcategory IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_all_sessions

UNION ALL
	
SELECT
    'cleaned_productprice' AS column_name,
    COUNT(DISTINCT cleaned_productprice) AS unique_values,
	COUNT(CASE WHEN cleaned_productprice IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_all_sessions

UNION ALL

SELECT
    'currencycode' AS column_name,
    COUNT(DISTINCT currencycode) AS unique_values,
	COUNT(CASE WHEN currencycode IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_all_sessions

UNION ALL

SELECT
    'timeonsite_formatted' AS column_name,
    COUNT(DISTINCT timeonsite_formatted) AS unique_values,
	COUNT(CASE WHEN timeonsite_formatted IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_all_sessions

UNION ALL

SELECT
    'pageviews' AS column_name,
    COUNT(DISTINCT pageviews) AS unique_values,
	COUNT(CASE WHEN pageviews IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_all_sessions

UNION ALL

SELECT
    'date' AS column_name,
    COUNT(DISTINCT date) AS unique_values,
	COUNT(CASE WHEN date IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_all_sessions

UNION ALL

SELECT
    'visitid' AS column_name,
    COUNT(DISTINCT visitid) AS unique_values,
	COUNT(CASE WHEN visitid IS NULL THEN 1 ELSE NULL END)
FROM
    cleaned_all_sessions;
```


