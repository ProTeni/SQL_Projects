
# DVD-Rentals

**Business Objectives:**

The objective of this business case is to optimize the DVD rental store's operations by leveraging data analytics. The goal is to increase revenue and profitability, improve customer satisfaction, and reduce operational costs. 


**Data Analysis:**

The store's data will be analyzed to provide insights into customer preferences, movie popularity, and operational efficiency. 

As a data analyst working for the DVD rental company, my task is to analyze the company's data to gain insights that can help improve business operations and increase profitability. By leveraging SQL to analyze the company's database, I can answer a series of questions that will provide valuable information to the management team.

![markus-spiske-Skf7HxARcoc-unsplash](https://user-images.githubusercontent.com/97428597/235983331-438adb86-bba5-4061-a88b-25063eb1bd98.jpg)
-------------------

### **Here are the questions (*and an assessment of my SQL Queries*)**


The store would like to know the movie titles of the movies that generated 'an above average rental income' from the 1st of June to the 1st of December in the year 2005.


> SELECT title, rental_rate  <br>
FROM film  <br>
INNER JOIN inventory  <br>
ON inventory.film_id = film.film_id  <br>
INNER JOIN rental  <br>
ON rental.inventory_id = inventory.inventory_id  <br>
WHERE EXISTS (  <br>
  SELECT 1  <br>
  FROM film  <br>
  WHERE rental_rate > (SELECT AVG(rental_rate) FROM film)  <br>
)  <br>
AND rental_date >= '2005-06-01' AND rental_date <= '2005-12-01';

**OR**

> CTE

```
WITH avg_rental_rate AS ( 
   SELECT AVG(rental_rate) AS avg_rate  
   FROM film  
)
SELECT title, rental_rate  
FROM film   
INNER JOIN inventory  
ON inventory.film_id = film.film_id  
INNER JOIN rental  
ON rental.inventory_id = inventory.inventory_id  
CROSS JOIN avg_rental_rate  
WHERE rental_rate > avg_rental_rate.avg_rate  
AND rental_date >= '2005-06-01' AND rental_date <= '2005-12-01';
```


We have 2 staff members, with staffIDs 1 and 2.
We want to give a bonus to the staff member that handled the most payments. (Most in terms of number of payments 
processed, not total dollar amount).

> SELECT staff_id, COUNT(amount) as "Number of Transactions" <br>
FROM payment  <br>
GROUP BY staff_id;


Corporate HQ is conducting a study on the relationship between cost and a movie MPAA rating (e.g. G, PG, R, etc...). 
What is the average replacement cost per MPAA rating? 

> SELECT ROUND (AVG (replacement_cost), 2) as "Average Replacement Cost", rating  <br>
FROM film <br>
GROUP BY rating;


How many payments occurred on a Monday?

> SELECT COUNT (*) AS "Count of all Payments on Monday"  <br>
FROM payment  <br>
WHERE EXTRACT (dow FROM payment_date) = 1

Return a query that returns the first and last name of customers who made payments greater than $15

> SELECT first_name, last_name  <br>
FROM customer c  <br>
INNER JOIN payment P  <br>
ON c.customer_id = p.customer_id  <br>
AND amount > 10

**OR** 

> SELECT first_name, last_name  <br>
FROM customer AS c  <br>
WHERE EXISTS  <br>
(SELECT * FROM payment AS p  <br>
WHERE c.customer_id = p.customer_id  <br>
AND amount > 10)


Get the percenatge of the rental cost on the replacement cost. 

> SELECT ROUND(rental_rate/replacement_cost, 2) * 100  <br>
percentage_ratio  <br>
FROM film


Write a query that returns the title of each movie along with other movie titles that's got the same movie length with each of the movie.

> SELECT f1.title, f2.title, f1.length <br>
FROM film f1  <br>
JOIN film f2  <br>
ON f1.film_id != f2.film_id  <br>
AND f1.length = f2.length  <br>


If 10% of the replacement cost is the deposit made on each rented movie, how much is the highest deposit charge?

> SELECT DISTINCT ROUND(replacement_cost * 0.1, 1) depost_from_film  <br>
FROM film  <br>
ORDER BY 1 DESC


During which months of the year did payments occur?

> SELECT DISTINCT (TO_CHAR (payment_date, 'Month')) AS "Payment Months"  <br>
FROM payment

Return a query that returns the first and last name of customers who made any payments less than $11

> SELECT first_name, last_name  <br>
FROM customer AS c  <br>
WHERE EXISTS  <br>
(SELECT * FROM payment AS p  <br>
WHERE c.customer_id = p.customer_id  <br>
AND amount < 11)


What Years did all the payment occur?

> SELECT DISTINCT EXTRACT (year FROM payment_date)  <br>
FROM payment



Write a query that categorizes our movies per price. Where movies costing $0.99 are the bargains, movies costing $2.99 are the normal, and movies costing $4.99 are the premium


> SELECT  <br>
 SUM(CASE rental_rate WHEN 0.99 THEN 1 <br>
  ELSE 0  <br>
  END) AS Bargains,  <br>
  SUM(CASE rental_rate WHEN 2.99 THEN 1  <br>
  ELSE 0  <br>
  END) AS Normal,  <br>
  SUM(CASE rental_rate WHEN 4.99 THEN 1  <br>
  ELSE 0  <br>
  END) AS Premium  <br>
FROM film	
	
	

Sum up the different movies we have per Ratings (R, PG-13 and PG).


> SELECT  <br>
	SUM(CASE rating WHEN 'R' THEN 1  <br>
	   ELSE 0  <br>
	   END) AS r,  <br>
	SUM(CASE rating WHEN 'PG' THEN 1  <br>
		ELSE 0  <br>
		END) AS pg,  <br>
	SUM(CASE rating WHEN 'PG-13' THEN 1  <br>
	   ELSE 0  <br>
	   END) AS pg13  <br>
FROM film


 
We are running a promotion to reward our top 5 customers with coupons. What are the customer IDs of the top 5 customers by total spend?

> SELECT customer_id, SUM(amount) <br>
FROM payment <br>
GROUP BY customer_id <br>
ORDER BY SUM(amount) desc <br>
LIMIT 5;

**OR**

> SELECT customer_id, SUM(amount) <br> 
FROM payment <br>
GROUP BY customer_id <br>
ORDER BY 2 desc <br>
LIMIT 5;


We are launching a platinum service for our most loyal customers. We will assign platinum status to customers that have had 40 or more transaction payments. What customer_ids are eligible for platinum status?

> SELECT customer_id, COUNT(amount) <br>
FROM payment <br>
GROUP BY customer_id <br>
HAVING COUNT(amount) > 40 or COUNT(amount) = 40;


What are the customer IDs of customers who have spent more than $100 in payment transations with our staff_id member 2?

> SELECT customer_id, SUM(amount) <br>
FROM payment <br>
GROUP BY customer_id, staff_id <br>
HAVING SUM(amount)>100 AND staff_id = 2;

**OR**

> SELECT customer_id, SUM(amount) <br>
FROM payment <br>
WHERE staff_id = 2 <br>
GROUP BY customer_id <br>
HAVING SUM(amount)>100;


Return the film_id and title of movies rented out between the 29th and 30th of April.

> SELECT title, description, film_id  <br>
FROM film  <br>
WHERE film_id IN  <br>
(SELECT inventory.film_id  <br>
FROM RENTAL  <br>
INNER JOIN inventory  <br>
ON inventory.inventory_id = rental.inventory_id  <br>
WHERE return_date BETWEEN '2005-05-29' AND '2005-05-30')  <br>
ORDER BY title


Return the customer IDs of customers who have spent at least $110 with the staff member who has an ID of 2

> SELECT customer_id, SUM(amount) <br> 
FROM payment <br>
WHERE staff_id = 2 <br>
GROUP BY customer_id <br>
HAVING SUM(amount) >= 110;


How many films begin with the letter J?

> SELECT COUNT(title) AS "No. of Film_Tiles Beginning with J" <br>
FROM film <br>
WHERE title LIKE 'J%';


What customer has the highest customer ID number whose name starts with an 'E' and has an address ID lower than 500?

> SELECT * FROM customer <br>
WHERE first_name LIKE 'E%' AND address_id < 500 <br>
ORDER BY customer_id DESC <br>
LIMIT 1;


California sales tax laws have changed and we need to alert our custimers to this through email. What are the emails of nthe customers who live in California?

> SELECT district, customer_id, first_name, last_name, email <br>
FROM address INNER JOIN customer <br>
ON customer.address_id = address.address_id <br>
WHERE district = 'California';

**OR**

> SELECT district, customer_id, first_name, last_name, email <br>
FROM address, customer <br>
WHERE address.address_id = customer.address_id <br>
and district = 'California';


Write a simple query that retunrs the title and rental_rates of movies above the average rental rate.

> SELECT title, rental_rate  <br>
FROM film  <br>
WHERE rental_rate >  <br>
(SELECT AVG(rental_rate) FROM film)


A customer walks in and is a huge fan of the actor "Nick Wahlberg" and wants to know which movies he is in.
Get a list of all the movies "Nick Wahlberg" has been in.

> SELECT title, description, release_year <br>
FROM film INNER JOIN film_actor <br>
ON film.film_id = film_actor.film_id <br>
INNER JOIN actor <br>
ON film_actor.actor_id = actor.actor_id <br>
WHERE first_name = 'Nick' AND last_name = 'Wahlberg';

**OR**

> SELECT title, description, release_year <br>
FROM film, film_actor, actor <br>
WHERE film.film_id = film_actor.film_id <br>
and film_actor.actor_id = actor.actor_id <br>
and first_name = 'Nick' AND last_name = 'Wahlberg';
