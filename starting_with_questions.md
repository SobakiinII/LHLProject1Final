Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

Select country, SUM(cast("totalTransactionRevenue" as integer)) as "sum"
from all_sessions
group by country
order by "sum" desc  Nulls last

Select city, SUM(cast("totalTransactionRevenue" as integer)) as "sum"
from all_sessions
group by city
order by "sum" desc  Nulls last

Answer:
The united states has the most total revenue of any country listed.
Ignoring the "not available entries", the highest city is San Francisco.


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

Select country, AVG(cast("transactions" as integer)) as "avg"
from all_sessions_view
group by country
order by "avg" desc nulls last

Select city, AVG(cast("transactions" as integer)) as "avg"
from all_sessions_view
group by city
order by "avg" desc nulls last

Answer:

I'm not entirely sure what the question is asking of me, but what I think it means is the average number of transactions per visit per country/city. 
The highest average of transactions per country is Isreal, with an average of ~0.016 transaction per visit. The highest average of transactions per city is Nashville, with an average of 1 transaction per visit. 




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

Select country, mode() within group (order by "v2ProductCategory")
From all_sessions_view
group by country

Select country, count("v2ProductCategory")
from all_sessions_view
Where "v2ProductCategory" like 'Home/Shop by Brand/You%'
group by country
order by count("v2ProductCategory") desc

Select city, mode() within group (order by "v2ProductCategory")
From all_sessions_view
group by city

Select city, count("v2ProductCategory")
from all_sessions_view
Where "v2ProductCategory" like 'Home/Sh%'
group by city
order by count("v2ProductCategory") desc

-- the patterns in the Like argument are meant to be interchanged to check different categories.

Answer:

By running the mode and then running a count on some patterns, we can check on the most popular categories in each country and city. For example, home/shop by brand/youtube is the most common category in the U.S. Meanwhile, there are too many cities missing from the demo dataset to get a true feeling for the patterns.



**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

SELECT country, product, cnt
FROM (
   SELECT country, "v2ProductName" as product, cnt,
          RANK() OVER (PARTITION BY country 
                       ORDER BY cnt DESC) AS rn
   FROM (
      SELECT country, "v2ProductName", COUNT("v2ProductName") AS cnt
      FROM all_sessions_view
      GROUP BY country, "v2ProductName" ) t
) s
WHERE s.rn = 1
order by cnt desc

SELECT city, product, cnt
FROM (
   SELECT city, "v2ProductName" as product, cnt,
          RANK() OVER (PARTITION BY city 
                       ORDER BY cnt DESC) AS rn
   FROM (
      SELECT city, "v2ProductName", COUNT("v2ProductName") AS cnt
      FROM all_sessions_view
      GROUP BY city, "v2ProductName" ) t
) s
WHERE s.rn = 1
order by cnt desc

Answer:
We can indeed. There are a good number of ties, for example sunnyvale, an outdoor and an indoor security camera are tied at 6 sales each.
In the united states as a whole, the most popular product is Google Men's 100% Cotton Short Sleeve Hero Tee White at 169 purchases.

**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:

I'm not entirely sure what this question is asking for. "Impact of revenue generated" is incredibly vague, and with this dataset in particular I would not recommend analyzing that anyway. Every column we were given with any relation to revenue was almost nothing but null values, and as such besides the pure numbers the first question asked for, no conclusion regarding revenue drawn from this dataset should be trusted.

