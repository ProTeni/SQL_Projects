You want to print out a list of all of the facilities and their cost to members. How would you retrieve a list of
only facility names and costs?

> SELECT name, membercost <br>
FROM cd.facilities <br>



How can you produce a list of facilities that charge a fee to members?

> SELECT * FROM cd.facilities <br>
WHERE membercost <> 0



How can you produce a list of facilities that charge a fee to members, and that fee is less than 1/50th of the monthly maintenance cost? 
Return the facid, facility name, member cost, and monthly maintenance of the facilities in question.

> SELECT facid, name, membercost <br>
FROM cd.facilities <br>
WHERE membercost < (0.02 * monthlymaintenance) <br>
AND membercost > 0



How can you produce a list of all facilities with the word 'Tennis' in their name?

> SELECT * FROM  cd.facilities <br>
WHERE name LIKE '%Tennis%'



How can you retrieve the details of facilities with ID 1 and 5? *without using the OR operator*


> ELECT * FROM cd.facilities <br>
WHERE facid IN (1, 5)



How can you produce a list of members who joined after the start of September 2012? Return the memid, surname, firstname, 
and joindate of the members in question.

> SELECT memid, surname, firstname, joindate <br>
FROM cd.members <br>
WHERE joindate > '2012-09-01'



How can you produce an ordered list of the first 10 surnames in the members table? The list must not contain duplicates.

 SELECT DISTINCT surname <br>
FROM cd.members <br>
ORDER BY 1 <br>
LIMIT 10



You'd like to get the signup date of your last member. How can you retrieve this information?

> SELECT Max (joindate) latest_signup_date <br>
FROM cd.members



Produce a count of the number of facilities that have a cost to guests of 10 or more.

> SELECT COUNT(*) Guestcost_count <br>
FROM cd.facilities <br>
WHERE guestcost >= 10



Produce a list of the total number of slots booked per facility in the month of September 2012. Produce an output table consisting of facility id
and slots, sorted by the number of slots.

> SELECT facid, SUM(slots) total_slots <br>
FROM cd.bookings <br>
WHERE starttime >= '2012-09-01' <br>
AND starttime < '2012-10-01' <br>
GROUP BY facid <br>
ORDER BY 1



Produce a list of facilities with more than 1000 slots booked. Produce an output table consisting of facility id and total slots, 
sorted by facility id.

> SELECT facid, SUM(Slots) total_slots <br>
FROM cd.bookings <br>
GROUP BY facid v
HAVING SUM(Slots) > 1000 <br>
ORDER BY 1



How can you produce a list of the start times for bookings for tennis courts, for the date '2012-09-21'? Return a list of start time and facility name pairings,
ordered by the time.

> SELECT starttime AS start, name <br>
FROM cd.bookings b, cd.facilities f <br>
WHERE b.facid = f.facid <br>
AND f.name ILIKE 'tennis%' <br>
AND b.starttime >= '2012-09-21' <br>
AND b.starttime < '2012-09-22' <br>

** OR**

> SELECT starttime AS start, name <br>
FROM cd.bookings <br>
INNER JOIN cd.facilities <br> 
ON cd.facilities.facid = cd.bookings.facid <br>
WHERE name LIKE 'Tennis%' <br>
AND cd.bookings.starttime >= '2012-09-21' <br>
AND cd.bookings.starttime < '2012-09-22' <br>



How can you produce a list of the start times for bookings by members named 'David Farrell'?

> SELECT starttime <br>
FROM cd.members <br>
INNER JOIN cd.bookings <br>
ON cd.bookings.memid = cd.members.memid <br>
WHERE firstname LIKE 'David'<br>
AND surname LIKE 'Farrell'



