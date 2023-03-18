
# DVD-Rentals

**1. Question:** 
We have 2 staff members, with staffIDs 1 and 2.
We want to give a bonus to the staff member that handled the most payments. (Most in terms of number of payments 
processed, not total dollar amount).



> SELECT staff_id, COUNT(amount) as "Number of Transactions" <br>
FROM payment
GROUP BY staff_id;


**2. Question:** 
Corporate HQ is conducting a study on the relationship between cost and a movie MPAA rating (e.g. G, PG, R, etc...). 
What is the average replacement cost per MPAA rating? 

> SELECT ROUND (AVG (replacement_cost), 2) as "Average Replacement Cost", rating  <br>
FROM film <br>
GROUP BY rating;


**3. Question:** 
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


**4. Question:** 
We are launching a platinum service for our most loyal customers. We will assign platinum status to customers that have had 40 or more transaction payments. What customer_ids are eligible for platinum status?

> SELECT customer_id, COUNT(amount) <br>
FROM payment <br>
GROUP BY customer_id <br>
HAVING COUNT(amount) > 40 or COUNT(amount) = 40;


**5.  Question:**
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


**6.  Question:**
Return the customer IDs of customers who have spent at least $110 with the staff member who has an ID of 2

> SELECT customer_id, SUM(amount) <br> 
FROM payment <br>
WHERE staff_id = 2 <br>
GROUP BY customer_id <br>
HAVING SUM(amount) >= 110;


**7.  Question:**
How many films begin with the letter J?

> SELECT COUNT(title) AS "No. of Film_Tiles Beginning with J" <br>
FROM film <br>
WHERE title LIKE 'J%';


**8.  Question:**
What customer has the highest customer ID number whose name starts with an 'E' and has an address ID lower than 500?

> SELECT * FROM customer <br>
WHERE first_name LIKE 'E%' AND address_id < 500 <br>
ORDER BY customer_id DESC <br>
LIMIT 1;

**9.  Question:**
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


**10.  Question:**
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
