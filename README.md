# SQL-Based Data Transformation and Analysis for E-commerce Insights

## Project/Goals
The aim of this project was to transform and analyse the 5 datasets recieved (sales_order, sales_by_sku, products, analytics, all_sessions), using SQL.

Across the datasets, was information related to product details including sales and visitor-website interaction. As such, the datasets were cleaned and analysed so as to answer a set of questions and gather important insights.

## Process
### Step 1 - Creating Tables and Importing the Data
Here we created SQL queries to create tables for each dataset and ultimately the database, wherein column names were specified and datatypes too.

## Step 2 - Data Cleaning and Transformation
Here, I examined the data to look for any NULL values, missing or incorrect entries, outliers and ensuring that columns chosen as primary keys were truly unique. The QA process helped to inform on what needed to be cleaned/transformed.

In conclusion, the analytics and all_sessions tables were in need of cleaning and transformation and as such, views were created to incorporate the neccessary changes.

## Step 3 - Used the Data to make Meaningful Insights and Answer Questions
Finally, using the cleaned views, I was able to answer questions.

## Results
Results helped discover finding such as:

- The average time spent on the website, whether by country or city, indicating regional differences in website interaction
- Differences in products ordered by consumers, by country and city
- Differences in the impact of revenue depending on the country where, some countries play a critical role in revenue generation for the website

## Challenges 
Challenges facing this project was the lack of context behind the data and columns' values - in a real world setting dealing with missing entries, NULL values or outliers etc.. may be handled differently where you would reach out to relevant stakeholders/domain experts to verify the information, for example, what may seem like an outlier worth removing could actually be viable data.

Another challenge was time - when presented with lots of data, it's important to really sit, soak in and digest the information so as to properly undestand the information and we were time limited.

## Future Goals
The data could be better understood and there could be a more comprehensive cleaning process.

In addition, more advanced analytics techniques could be incorporated to gain more insight out of the data.
