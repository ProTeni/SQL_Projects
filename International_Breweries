
#International Breweries 

**My queries are intended to assist the Brewery in identifying the top-performing sales representative, the most devoted customer, and enhancing business decisions regarding optimal products.**


Write an SQL query to fetch each “SALES_REP” from the breweries

> Select DISTINCT sales_rep FROM  breweries



Write an SQL query to fetch “BRANDS” from breweries table in upper case.

> Select DISTINCT(UPPER(brands)) Brands FROM breweries



Write an SQL query to print all details from the breweries table; sort the QUANTITY column in Ascending order and the COSTS column in Descending order

> Select * FROM breweries
ORDER BY quantity ASC, cost DESC



Write an SQL query to print the profit made from two BRANDS, “trophy” and “eagle” in 
the first quarter of 2019

> Select profit FROM breweries
WHERE brands IN ('trophy', 'eagle')
AND months IN ('January', 'February', 'March')



Write a query that reduces the cost of the trophy brand by 2%

> Select ROUND(cost-(cost*0.02), 0) AS two_percent_discount 
FROM breweries
WHERE brands = 'trophy'



Which sales rep made the highest profit in Ghana in the year 2017?

> Select sales_rep, profit highest_profit
FROM breweries
WHERE years = 2017
ORDER BY profit DESC
LIMIT 1



What region recorded the lowest quantity of goods in the last quarter of every year?

SELECT region, quantity lowest_quantity
FROM breweries
WHERE months IN ('October', 'November', 'December')
ORDER BY 2 
FETCH FIRST 1 row only



The Breweries company has a yearly tradition of promoting the sales_rep who makes the highest profit in the year. Who deserves this promotion in 2019?

> Select sales_rep, profit
FROM breweries
WHERE years = 2019
ORDER BY profit desc
LIMIT 1



Regions with quantities of trophy which are less than 60000, need to be restocked. What regions do we restock with the trophy brand?


SELECT region, brands, quantity
FROM breweries
WHERE brands = 'trophy'
GROUP BY region, brands,quantity
HAVING quantity < 60000

