# Blinkit Sales Analysis SQL Project

## Project Overview

**Project Title**: Blinkit Sales Analysis  
**Level**: Beginner  
**Database**: `blinkit_sales`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and normalize inconsistent data entries across the dataset.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a table named `blinkit_sales` to store the sales data.
- **Table Creation**: The table structure includes columns for item fat content, item identifier, item type, establishment year, outlet identifier, outlet location type, outlet size, outlet type, item visibility, item weight, total sales, and rating.

```sql
DROP TABLE IF EXISTS blinkit_sales;

CREATE TABLE blinkit_sales (
	item_fat_content VARCHAR(50),
	item_identifier VARCHAR(50),
	item_type VARCHAR(50),
	establishment_year INT,
	outlet_identifier VARCHAR(50),
	outlet_location_type VARCHAR(50),
	outlet_size VARCHAR(50),
	outlet_type VARCHAR(50),
	item_visibility NUMERIC(10,2),
	item_weight NUMERIC(10,2),
	total_sales NUMERIC(1000,2),
	rating NUMERIC(10,2)
);

SELECT * FROM blinkit_sales;

```

### 2. Data Exploration & Cleaning

* **Record Count**: Determine the total number of records in the dataset.
* **Customer Count**: Find out how many unique outlets are in the dataset.
* **Category Count**: Identify all unique product categories in the dataset.
* **Data Uniformity**: Identify inconsistent data classifications and clean them up to ensure uniformity.

```sql
UPDATE blinkit_sales
SET item_fat_content =
CASE
WHEN item_fat_content IN('LF', 'low fat') THEN 'Low Fat'
WHEN item_fat_content = 'reg' THEN 'Regular'
ELSE item_fat_content
END;

```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to display all details (all columns) for items that belong to the 'Fruits and Vegetables' category**:

```sql
SELECT * FROM blinkit_sales
WHERE item_type = 'Fruits and Vegetables';

```

2. **Write a query to find the total number of unique outlets**:

```sql
SELECT COUNT(DISTINCT outlet_identifier) AS total_unique_outlets
FROM blinkit_sales;

```

3. **Write a query to list the top 5 highest-selling transactions**:

```sql
SELECT item_identifier, item_type, total_sales
FROM blinkit_sales
ORDER BY total_sales DESC
LIMIT 5;

```

4. **Write a query to extract all information for outlets located in a 'Tier 1' region that are also classified as 'Medium' in Outlet Size**:

```sql
SELECT * FROM blinkit_sales
WHERE outlet_location_type = 'Tier 1' AND outlet_size = 'Medium';

```

5. **Write a query to calculate the average Rating across all records in the dataset**:

```sql
SELECT ROUND(AVG(rating),2) AS average_rating
FROM blinkit_sales;

```

6. **Write a query to find all details for items where the Item Visibility is exactly 0**:

```sql
SELECT * FROM blinkit_sales
WHERE item_visibility = 0;

```

7. **Write a query to fetch a distinct list of all product categories (Item Type) available in the dataset, sorted alphabetically**:

```sql
SELECT DISTINCT item_type
FROM blinkit_sales
ORDER BY item_type ASC;

```

8. **Write a query to find all unique Outlet Identifier codes that have an Outlet Size of 'Small'**:

```sql
SELECT COUNT(outlet_identifier) AS unique_outlet_codes
FROM blinkit_sales
WHERE outlet_size = 'Small';

```

9. **Write a query to select the Item Identifier, Item Type, and Rating for all items that have a perfect Rating of 5**:

```sql
SELECT item_identifier, item_type, rating
FROM blinkit_sales
WHERE rating = 5;

```

10. **Write a query to find the combined sum of Total Sales generated across the entire dataset**:

```sql
SELECT SUM(total_sales) AS total_sales 
FROM blinkit_sales;

```

11. **Write a query to find the total sales and the average item visibility for each Outlet Type. Sort the results in descending order of total sales**:

```sql
SELECT outlet_type, SUM(total_sales) AS total_sales, ROUND(AVG(item_visibility),2) AS average_item_visibility
FROM blinkit_sales
GROUP BY outlet_type
ORDER BY total_sales DESC;

```

12. **The Item Fat Content column contains inconsistent entry types (e.g., 'Low Fat', 'low fat', 'LF' and 'Regular', 'reg'). Write a query using a conditional statement to normalize these into two distinct groups—'Low Fat' and 'Regular'— and find the total sales for each group**:

```sql
UPDATE BLINKIT_SALES
SET ITEM_FAT_CONTENT = 
	CASE
		WHEN ITEM_FAT_CONTENT IN ('LF', 'low fat') THEN 'Low Fat'
		WHEN ITEM_FAT_CONTENT = 'reg' THEN 'Regular'
		ELSE ITEM_FAT_CONTENT
	END,
	SUM(TOTAL_SALES) AS TOTAL_SALES
FROM BLINKIT_SALES
GROUP BY
	CASE
		WHEN ITEM_FAT_CONTENT IN ('LF', 'low fat') THEN 'Low Fat'
		WHEN ITEM_FAT_CONTENT = 'reg' THEN 'Regular'
		ELSE ITEM_FAT_CONTENT
	END
;

```

13. **Write a query to identify the Item Type categories that have an average Rating greater than 4.0. Display the category name and its corresponding average rating**:

```sql
SELECT item_type, ROUND(AVG(rating),2) AS average_rating
FROM blinkit_sales
GROUP BY item_type
HAVING AVG(rating) > 4.0;

```

14. **Some records are missing values in the Item Weight column. Write a query to count how many records have a missing Item Weight, grouped by their Outlet Establishment Year**:

```sql
SELECT COUNT(item_weight) AS count 
FROM blinkit_sales
WHERE item_weight IS NULL
GROUP BY establishment_year;

```

15. **Write a query to calculate the maximum and minimum sales for every combination of Outlet Location Type and Outlet Size**:

```sql
SELECT DISTINCT outlet_location_type, outlet_size, MAX(total_Sales) AS max_sales, 
	MIN(total_sales) AS min_sales
FROM blinkit_sales
GROUP BY outlet_location_type, outlet_size;

```

16. **Write a query to find the average Item Weight for each Item Type. Exclude any rows where the weight is missing (NULL), and sort the results from highest weight to lowest weight**:

```sql
SELECT item_type, ROUND(AVG(item_weight),2) AS average_item_weight
FROM blinkit_sales
WHERE item_weight IS NOT NULL
GROUP BY item_type
ORDER BY average_item_weight DESC;

```

17. **Write a query to count how many unique outlets were established in each Outlet Establishment Year**:

```sql
SELECT establishment_year, COUNT(DISTINCT outlet_identifier) AS unique_outlets
FROM blinkit_sales
GROUP BY establishment_year
ORDER BY establishment_year;

```

18. **Write a query to find the Outlet Location Type regions that have generated a cumulative total sales of more than 50,000**:

```sql
SELECT outlet_location_type, SUM(total_sales) AS total_sales
FROM blinkit_sales
GROUP BY outlet_location_type
HAVING SUM(total_sales) > 50000;

```

19. **Write a query to categorize every row's Rating into performance tiers: 'High' (Rating of 4 or 5), 'Average' (Rating of 3), and 'Low' (Rating below 3). Display the tier name alongside the total count of items in that tier**:

```sql
SELECT
	CASE
		WHEN rating >=4 THEN 'High'
		WHEN rating = 3 THEN 'Average'
		ELSE 'Low'
	END AS rating_tier,
	COUNT(*) AS total_items
FROM blinkit_sales
GROUP BY 
	CASE
		WHEN rating >=4 THEN 'High'
		WHEN rating = 3 THEN 'Average'
		ELSE 'Low'
	END;

```

20. **Write a query to count how many items exist for each combination of a normalized fat content (use 'Low Fat' or 'Regular') and Outlet Size**:

```sql
SELECT item_fat_content, outlet_size, COUNT(*) AS total_items
FROM blinkit_sales
GROUP BY item_fat_content, outlet_size;

```

21. **Write a query to find which specific product category (Item Type) generated the highest total sales inside each separate Outlet Type**:

```sql
WITH rank_categories AS (SELECT outlet_type, item_type, SUM(total_sales) AS total_sales,
	RANK() OVER(PARTITION BY item_type ORDER BY SUM(total_sales) DESC) AS rank
FROM blinkit_sales
GROUP BY outlet_type, item_type)
SELECT outlet_type, item_type, total_sales
FROM rank_categories
WHERE rank = 1;

```

22. **Write a subquery or CTE-based query to display all details of individual transaction entries whose Total Sales value is higher than the overall average transaction sales across the entire dataset, but filter the final output to only show entries belonging to outlets established before the year 2015**:

```sql
SELECT * FROM blinkit_sales
WHERE total_sales > (SELECT  AVG(total_sales) AS average_sales FROM blinkit_sales)
	AND establishment_year > 2015;

```

23. **Write a query to rank the outlets (Outlet Identifier) within each Outlet Location Type based on their cumulative Total Sales. Use an appropriate window function (such as RANK() or DENSE_RANK()) so that the ranking resets for each location tier**:

```sql
SELECT outlet_location_type, outlet_identifier,
	SUM(total_sales) AS total_Sales,
	RANK() OVER(PARTITION BY outlet_location_type ORDER BY SUM(total_sales) DESC)
FROM blinkit_sales
GROUP BY outlet_location_type, outlet_identifier;

```

24. **Write a query to find all items whose individual Item Visibility is strictly greater than the average Item Visibility of items belonging to that specific item's Item Type**:

```sql
SELECT item_identifier, item_type, item_visibility
FROM blinkit_sales AS main
WHERE
item_visibility > 
(SELECT AVG(item_visibility)
FROM blinkit_sales AS sub
WHERE sub.item_type = main.item_type);

```

25. **Write an analytical query to calculate the running (cumulative) total of Total Sales across the years, ordered chronologically by the Outlet Establishment Year**:

```sql
SELECT DISTINCT establishment_year, SUM(total_sales) AS total_Sales,
	SUM(SUM(total_sales)) OVER(ORDER BY establishment_year) AS running_total
FROM blinkit_sales
GROUP BY establishment_year
ORDER BY establishment_year;

```

## Findings

* **Customer Demographics**: The dataset includes items from various categories, with sales distributed across multiple establishment years and localized regional outlets.
* **High-Value Transactions**: Several transactions and specific outlet structures generated high sales volume, indicating premium distribution hubs.
* **Sales Trends**: Historical and chronological running totals show progressive variations in overall business sales expansion.
* **Customer Insights**: The analysis highlights key item visibility points, perfect product ratings, and tier-specific regional operational performance.

## Reports

* **Sales Summary**: A detailed report summarizing total sales, outlet classifications, and product tier metrics.
* **Trend Analysis**: Insights into distribution performance across different establishment years and outlet regional sizes.
* **Customer Insights**: Reports on product categories, normalized item fat structures, and operational visibility levels.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `Blinkit Sales Project.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `Blinkit Sales Project.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

## Author - Mayank Padia
This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!

Thank you for your support, and I look forward to connecting with you!

```

```
