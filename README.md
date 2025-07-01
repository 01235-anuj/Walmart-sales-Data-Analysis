# Walmart-sales-Data-Analysis:  End-to-End SQL + Python Project 

## Project Overview

This project is an end-to-end data analysis solution designed to extract critical business insights from Walmart sales data. We utilize Python for data processing and analysis, SQL for advanced querying, and structured problem-solving techniques to solve key business questions. The project is ideal for data analysts looking to develop skills in data manipulation, SQL querying, and data pipeline creation.

---

## Project Steps

### 1. Set Up the Environment
   - **Tools Used**: Visual Studio Code (VS Code), Python, SQL (MySQL and PostgreSQL)
   - **Goal**: Create a structured workspace within VS Code and organize project folders for smooth development and data handling.

### 2. Set Up Kaggle API
   - **API Setup**: Obtain your Kaggle API token from [Kaggle](https://www.kaggle.com/) by navigating to your profile settings and downloading the JSON file.
   - **Configure Kaggle**: 
      - Place the downloaded `kaggle.json` file in your local `.kaggle` folder.
      - Use the command `kaggle datasets download -d <dataset-path>` to pull datasets directly into your project.
    
### 3. Download Walmart Sales Data
   - **Data Source**: Use the Kaggle API to download the Walmart sales datasets from Kaggle.
   - **Dataset Link**: [Walmart Sales Dataset](https://www.kaggle.com/najir0123/walmart-10k-sales-datasets)
   - **Storage**: Save the data in the `data/` folder for easy reference and access.

### 4. Install Required Libraries and Load Data
   - **Libraries**: Install necessary Python libraries using:
     ```bash
     pip install pandas numpy sqlalchemy mysql-connector-python psycopg2
     ```
   - **Loading Data**: Read the data into a Pandas DataFrame for initial analysis and transformations.

### 5. Explore the Data
   - **Goal**: Conduct an initial data exploration to understand data distribution, check column names, types, and identify potential issues.
   - **Analysis**: Use functions like `.info()`, `.describe()`, and `.head()` to get a quick overview of the data structure and statistics.

### 6. Data Cleaning
   - **Remove Duplicates**: Identify and remove duplicate entries to avoid skewed results.
   - **Handle Missing Values**: Drop rows or columns with missing values if they are insignificant; fill values where essential.
   - **Fix Data Types**: Ensure all columns have consistent data types (e.g., dates as `datetime`, prices as `float`).
   - **Currency Formatting**: Use `.replace()` to handle and format currency values for analysis.
   - **Validation**: Check for any remaining inconsistencies and verify the cleaned data.

### 7. Feature Engineering
   - **Create New Columns**: Calculate the `Total Amount` for each transaction by multiplying `unit_price` by `quantity` and adding this as a new column.
   - **Enhance Dataset**: Adding this calculated field will streamline further SQL analysis and aggregation tasks.


### 8. Load Data into MySQL and PostgreSQL
   - **Set Up Connections**: Connect to MySQL and PostgreSQL using `sqlalchemy` and load the cleaned data into each database.
   - **Table Creation**: Set up tables in both MySQL and PostgreSQL using Python SQLAlchemy to automate table creation and data insertion.
   - **Verification**: Run initial SQL queries to confirm that the data has been loaded accurately.

### 9. SQL Analysis: Complex Queries and Business Problem Solving

   - **Business Problem-Solving**: Write and execute complex SQL queries to answer critical business questions, such as:
     
   - find the different payment method and number of Transactions and number of qty sold

```sq
select payment_method,
       count(*) as no_payments,
	   sum(quantity) as no_quntity_sold
from walmart 
Group by payment_method
```
   - Identify the Highest_rated Category in each branch Display the branch Category --AVG Rating

```sq
select *
from
(select 
     branch,
	 category,
	 avg(rating) as AVG_Ratings,
	 Rank() over(partition by branch order by avg(rating) desc) as rank
from walmart
group by 1,2)
where rank=1;
```

   - Identify the Busiest day for each Branch based on number of Transactions

```sq
select *
from
(select  branch,
        To_char(To_Date(date,'dd/mm/yy'),'day') as day_name,
        count(*) as no_transactions,
		Rank() over(Partition by branch order by count(*) desc) as rank
		from walmart
		group by 1,2
)
where rank=1
```
   - Calculate the Total quantity of items sold per payment method.List the Payment_method and total_quantity.

```sq
select payment_method,sum(quantity) as no_qty_sold
from walmart
Group By payment_method
```
   - Determine the min,max,avg rating of the category for each city. List the city,avg_rating,min_rating,max_rating.]

```sq
select 
      city,
	  category,
	  max(rating) as max_rating,
	  min(rating) as min_rating,
	  avg(rating) as avg_rating
from walmart
group by 1,2
```

   -  Calculate the total profit for each Category by considering Total_profit as
   -  (unit_price *quantity*profit_margin).List Category and Total_profit ,orderd from highest to lowest profit.

```sq
Select Category,Sum(total) as total_revenue,
        sum(total*profit_margin) as profit
from walmart
Group BY 1;
```
  - Determine the most Common payment method for each Branch. Display branch and the preferred_payment_method

```sq
with cte
as
(select branch, payment_method,
      count(*) as total_trans,
	  rank() over(partition by branch order by count(*) desc) as rank
from walmart
group by 1,2)
select *
from cte
where rank=1
```
 - Categorize sales into three groups morning,evening,afternoon. find out eachof the shift and number of invoices.

```sq
select branch ,
case 
     when extract (hour from(time::time))<12 then 'Morning'
	 when extract(hour from(time::time)) between 12 and 17 then 'Afternoon'
	 else 'Evening'
	 end date_time,
	 count(*)
from walmart 
group by 1,2
order by 1,3 desc
```
   
      
### 10. Project Publishing and Documentation
   - **Documentation**: Maintain well-structured documentation of the entire process in Markdown or a Jupyter Notebook.
   - **Project Publishing**: Publish the completed project on GitHub or any other version control platform, including:
     - The `README.md` file (this document).
     - Jupyter Notebooks (if applicable).
     - SQL query scripts.


    
## Results and Insights

This section will include your analysis findings:
- **Sales Insights**: Key categories, branches with highest sales, and preferred payment methods.
- **Profitability**: Insights into the most profitable product categories and locations.
- **Customer Behavior**: Trends in ratings, payment preferences, and peak shopping hours.


## Future Enhancements

Possible extensions to this project:
- Additional data sources to enhance analysis depth.
- Automation of the data pipeline for real-time data ingestion and analysis.
