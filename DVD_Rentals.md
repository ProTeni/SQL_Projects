
# DVD-Rentals

**Question:**
The store would like to know the movie titles of the movies that generated 'an above average rental income' from the 1st of June to the 1st of December in the year 2005.

> WITH avg_rental_rate AS (
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

**OR**

SELECT title, rental_rate 
FROM film 
INNER JOIN inventory
ON inventory.film_id = film.film_id
INNER JOIN rental
ON rental.inventory_id = inventory.inventory_id
WHERE EXISTS (
  SELECT 1
  FROM film
  WHERE rental_rate > (SELECT AVG(rental_rate) FROM film)
)
AND rental_date >= '2005-06-01' AND rental_date <= '2005-12-01';


**Question:** 
We have 2 staff members, with staffIDs 1 and 2.
We want to give a bonus to the staff member that handled the most payments. (Most in terms of number of payments 
processed, not total dollar amount).

> SELECT staff_id, COUNT(amount) as "Number of Transactions" <br>
FROM payment
GROUP BY staff_id;


**Question:** 
Corporate HQ is conducting a study on the relationship between cost and a movie MPAA rating (e.g. G, PG, R, etc...). 
What is the average replacement cost per MPAA rating? 

> SELECT ROUND (AVG (replacement_cost), 2) as "Average Replacement Cost", rating  <br>
FROM film <br>
GROUP BY rating;


**Question:**
How many payments occurred on a Monday?

> SELECT COUNT (*) AS "Count of all Payments on Monday"
FROM payment
WHERE EXTRACT (dow FROM payment_date) = 1

**Question:**
Return a query that returns the first and last name of customers who made payments greater than $15

> SELECT first_name, last_name
FROM customer c
INNER JOIN payment P
ON c.customer_id = p.customer_id
AND amount > 10

**OR** 

> SELECT first_name, last_name
FROM customer AS c
WHERE EXISTS
(SELECT * FROM payment AS p
WHERE c.customer_id = p.customer_id
AND amount > 10)


**Question:**
Get the percenatge of the rental cost on the replacement cost. 

> SELECT ROUND(rental_rate/replacement_cost, 2) * 100
percentage_ratio
FROM film


**Question:**
If 10% of the replacement cost is the deposit made on each rented movie, how much is the highest deposit charge?

> SELECT DISTINCT ROUND(replacement_cost * 0.1, 1) depost_from_film
FROM film
ORDER BY 1 DESC


**Question:**
During which months of the year did payments occur?

> SELECT DISTINCT (TO_CHAR (payment_date, 'Month')) AS "Payment Months"
FROM payment

**Question:**
Return a query that returns the first and last name of customers who made any payments less than $11

> SELECT first_name, last_name
FROM customer AS c
WHERE EXISTS
(SELECT * FROM payment AS p
WHERE c.customer_id = p.customer_id
AND amount < 11)


**Question:**
What Years did all the payment occur?

> SELECT DISTINCT EXTRACT (year FROM payment_date) 
FROM payment


**Question:** 
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


**Question:** 
We are launching a platinum service for our most loyal customers. We will assign platinum status to customers that have had 40 or more transaction payments. What customer_ids are eligible for platinum status?

> SELECT customer_id, COUNT(amount) <br>
FROM payment <br>
GROUP BY customer_id <br>
HAVING COUNT(amount) > 40 or COUNT(amount) = 40;


**Question:**
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


**Question:**
Return the film_id and title of movies rented out between the 29th and 30th of April.

> SELECT title, description, film_id
FROM film
WHERE film_id IN
(SELECT inventory.film_id
FROM RENTAL 
INNER JOIN inventory
ON inventory.inventory_id = rental.inventory_id
WHERE return_date BETWEEN '2005-05-29' AND '2005-05-30')
ORDER BY title


**Question:**
Return the customer IDs of customers who have spent at least $110 with the staff member who has an ID of 2

> SELECT customer_id, SUM(amount) <br> 
FROM payment <br>
WHERE staff_id = 2 <br>
GROUP BY customer_id <br>
HAVING SUM(amount) >= 110;


**Question:**
How many films begin with the letter J?

> SELECT COUNT(title) AS "No. of Film_Tiles Beginning with J" <br>
FROM film <br>
WHERE title LIKE 'J%';


**Question:**
What customer has the highest customer ID number whose name starts with an 'E' and has an address ID lower than 500?

> SELECT * FROM customer <br>
WHERE first_name LIKE 'E%' AND address_id < 500 <br>
ORDER BY customer_id DESC <br>
LIMIT 1;


**Question:**
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


**Question:**
Write a simple query that retunrs the title and rental_rates of movies above the average rental rate.

> SELECT title, rental_rate 
FROM film
WHERE rental_rate > 
(SELECT AVG(rental_rate) FROM film)

**Question:**
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
