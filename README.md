# Business-Analytics-Nanodegree


I enrolled myself in Udacity Business Analytics Nanodegree because I wanted to learn more about data and how to analyze it in the most optimized way.

I have always been so passionate about data and how useful it can be if it is analyzed in the right way and all the insights that can be extracted from it, that's why I enrolled myself in that nanodegree. Unforuntely I had to drop out of it and when I tried to re-enroll again the nanodegree price was really expensive. :( Therefore, I'll add my projects here without the certificate YET.

The main goals of this nanodegree are; analyzing data using Excel, query a database using SQL and building an interactive dashboards and data visualizations using Tableau. You can check the course syllabus [here](https://d20vrrgs8k4bvw.cloudfront.net/documents/en-US/Business+Analytics+Nanodegree+Program+Syllabus+2.0.pdf).


**First Project -- Analyze NYSE Data**

## Analyze NYSE Data


The main goal of this project is for you to demonstrate your ability to:

    * Interpret the measures of central tendency and spread (mean, median, standard deviation, range)
    * Use a combination of Excel or Google Sheets functions (e.g., IF statements, INDEX and MATCH, calculating descriptive statistics with the IF statement, drop downs, data validation, VLOOKUP).
    * Analyze and forecast financial business metrics using Excel or Google Sheets.
    * Create visualizations of a business metric and use Excel or Google Sheets to create a financial forecast model.



**The following information is included in the Project data NYSE file**:

    * Ticker symbol: Stock symbol
    * Years: Number of years for which data is provided
    * Period ending
    * Total revenue
    * Cost of goods sold
    * Sales, General and Administrative expenses
    * Research and Development expenses
    * Other Operating expense items
    * Global Industry Classification Standard (GICS) Sector: Industry sector the company is categorized under (e.g., American Airlines with the ticker symbol AAL is categorized under Industrials.)
    * GICS Sub Industry: Sub-industry sector the company is categorized under (e.g., AAL is further categorized under the sub-category of Airlines industry.)



### Instructions:

For the final project, you will conduct three tasks:

  1. complete your own data analysis and create a presentation to share your findings,
  2. develop a dashboard for a Profit and Loss Statement, and
  3. create a Financial Forecasting Model using three scenarios.
  
  

## Dynamic Financial Forecast with three scenarios
![image](https://github.com/SalmaH96/Business-Analytics-Nanodegree/blob/main/Screen%20Recording%202022-07-03%20at%2012.55.00.08%20PM.gif)

## Dynamic P&L Statement
![image](https://github.com/SalmaH96/Business-Analytics-Nanodegree/blob/main/Screen%20Recording%202022-07-03%20at%2001.08.11.77%20PM.gif)


## **You can download the project  [here](https://github.com/SalmaH96/Business-Analytics-Nanodegree/blob/main/New%20York%20Stock.xlsx) :star:**

**Second Project -- Query Music Store**

## SQL For Data Analysis

In this project, you will query the Chinook Database. The Chinook Database holds information about a music store. For this project, you will be assisting the Chinook team with understanding the media in their store, their customers and employees, and their invoice information. To assist you in the queries ahead, the schema for the Chinook Database is provided below. You can see the columns that link tables together via the arrows.

 ![image](https://github.com/SalmaH96/Business-Analytics-Nanodegree/blob/main/001.png)


## Queries I used for my Project:

Query 1 - Used to get the sale per unit per genre and percentage of sale. Limited to top 10.
```
  SELECT
    *,
    (SELECT
      ROUND(ROUND((units_sold * 100), 2) / SUM(quantity), 2)
    FROM invoiceline)
    percentage
  FROM (SELECT
    g.name AS genre,
    COUNT(*) AS units_sold
  FROM track t
  JOIN genre g
    ON t.genreid = g.genreid
  JOIN invoiceline il
    ON il.trackid = t.trackid
  GROUP BY 1
  ORDER BY 2 DESC) sub
  LIMIT 10;
```

Query 2 - Used to get the aggregated table of countries with one customers grouped under 'Other' as country name, total number of customers, total number of orders, total value of orders, average value of orders. Countries grouped in others are excluded in the analysis because of its limited data.

```
  WITH all_country_stats
  AS (SELECT
    c.country country_name,
    SUM(i.total) total_order,
    ROUND(AVG(i.total), 2) avg_order,
    COUNT(invoiceid) no_of_orders,
    COUNT(DISTINCT c.customerid) no_of_customers
  FROM invoice i
  JOIN customer c
    ON c.customerid = i.customerid
  GROUP BY 1),

  grouped_country
  AS (SELECT
    CASE
      WHEN no_of_customers = 1 THEN 'Other'
      ELSE country_name
    END AS grouped_country,
    *
  FROM all_country_stats)

  SELECT DISTINCT
    (grouped_country),
    SUM(no_of_customers) no_of_customers,
    SUM(no_of_orders) no_of_orders,
    SUM(total_order) total_value_order,
    ROUND(AVG(avg_order), 2) avg_order
  FROM grouped_country
  WHERE NOT grouped_country = 'Other'
  GROUP BY 1
  ORDER BY 3 DESC;
```


Query 3 - Used to get the percentage of sale per media type
```
  SELECT
    *,
    (SELECT
      ROUND(ROUND((total_qty * 100), 2) / SUM(quantity), 2)
    FROM invoiceline)
    percentage

  FROM (SELECT
    m.name media_type,
    SUM(quantity) AS total_qty
  FROM mediatype m
  JOIN track t
    ON t.mediatypeid = m.mediatypeid
  JOIN invoiceline il
    ON il.trackid = t.trackid
  GROUP BY 1
  ORDER BY 2 DESC) subquery;
```


Query 4 - Used to get all sales made by the sales agent
```
  SELECT
    (CASE
      WHEN e.employeeid = '3' THEN i.total
      ELSE NULL
    END) AS Jane_Peacock,
    (CASE
      WHEN e.employeeid = '4' THEN i.total
      ELSE NULL
    END) AS Margaret_Park,
    (CASE
      WHEN e.employeeid = '5' THEN i.total
      ELSE NULL
    END) AS Steve_Johnson
  FROM employee e
  JOIN customer c
    ON c.supportrepid = e.employeeid
  JOIN invoice i
    ON i.customerid = c.customerid
```


Query 5 - Used to get which agent has the most sales
```
  SELECT
    strftime('%Y-%m',e.hiredate) hire_date,
    e.firstname|| ' ' ||e.lastname rep,
    COUNT(i.invoiceid) number_of_sale,
    ROUND(SUM(i.total), 2) value_of_sale
  FROM employee e
  JOIN customer c
    ON c.supportrepid = e.employeeid
  JOIN invoice i
    ON i.customerid = c.customerid
  WHERE title = 'Sales Support Agent'
  GROUP BY 1,
           2
```


### [You can download it here] (https://github.com/SalmaH96/Business-Analytics-Nanodegree/blob/main/Music%20Store.txt)

**Third Project -- Data Visualization**

## [Data Visualization]

This section takes data visualizations to another level. Use Tableau to analyze and visualize large data sets, and create informative and dynamic data dashboards. This section makes it easier to visualize large data sets in a more readable form in order to present insights to all business areas.

**Project Description - Flight Delays and Cancellations**

In this project, you'll create visualizations to reveal insights from a data set. You will create data visualizations that tell a story or highlight patterns in the data set. Your work should be a reflection of the theory and practice of data visualization, such as visual encodings, design principles, and effective communication.

**1) Flight Delays and Cancellations**

This data comes from a Kaggle dataset, it tracks the on-time performance of US domestic flights operated by large air carriers in 2015. You can find the dataset in supporting materials at the bottom of this page.

The file you must use in creating your data visualizations is the flights.csv file. The other two provided files may be used in conjunction with the flights.csv file, but should not be used alone.

# My Project

### Dashboard 1
![image](https://github.com/SalmaH96/Business-Analytics-Nanodegree/blob/main/Screen%20Recording%202022-07-03%20at%2005.56.10.94%20PM.gif)


### Dashboard 2
![image](https://github.com/SalmaH96/Business-Analytics-Nanodegree/blob/main/Screen%20Recording%202022-07-03%20at%2005.59.36.95%20PM.gif)


### Dashboard 3
![image](https://github.com/SalmaH96/Business-Analytics-Nanodegree/blob/main/Screen%20Recording%202022-07-03%20at%2006.03.13.38%20PM.gif)


**[Link to Tableau public](https://public.tableau.com/app/profile/salma.hussien/viz/FlightsProject_16568617704780/Dashboard2-Routeswithmostflights)**


**[You can also download it here](https://github.com/SalmaH96/Business-Analytics-Nanodegree/blob/main/Flights%20Project.twbx)** :star:



