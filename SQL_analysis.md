## Analysis using SQL

#### Task 1. What is the number of reported incidents?

<pre>
SELECT COUNT(*) 
FROM fatalities_cleaned
</pre>

#### Task 2. What is the year to year change for the number of fatal incidents?

<pre>
SELECT YEAR(incident_date) AS Year, COUNT(*) AS num_fatalities,
LAG(COUNT(*)) OVER (ORDER BY YEAR(incident_date)) AS previous_year,
ROUND((COUNT(*) - LAG(COUNT(*)) OVER (ORDER BY YEAR(incident_date))) / LAG(COUNT(*)) OVER (ORDER BY YEAR(incident_date)) * 100) AS percentage_change
FROM fatalities_cleaned
WHERE YEAR(incident_date) <> 2022
GROUP BY YEAR(incident_date)
ORDER BY 1
</pre>

#### Task 3.  What is the number of fatalities that received a citation?

<pre>
SELECT citation, COUNT (*) 
FROM fatalities_cleaned
GROUP BY citation
</pre>

#### Task 4. What day of the week has the most fatalities and what is the overall percentage?

<pre>
SELECT day_of_week, COUNT(*),
  ROUND(COUNT(*) * 100 / 14914, 2) AS overall_percentage
FROM fatalities_cleaned
GROUP BY day_of_week
</pre>

#### Task 5. What is the number of fatalities involving welding?

<pre>
SELECT COUNT (*)
FROM fatalities_cleaned
WHERE description LIKE "%weld%"
</pre>

#### Task 6. Select the last 5 from the previous query. The task is to calculates the last 5 fatalities during welding.

<pre>
SELECT *
FROM fatalities_cleaned
WHERE description LIKE "%weld%"
ORDER BY incident_date DESC
LIMIT 5
</pre>

#### Task 7. Select the top 5 states with the most fatal incidents.

<pre>
SELECT state, COUNT (*)
FROM fatalities_cleaned
GROUP BY state
ORDER BY 2 DESC
LIMIT 5
</pre>

#### Task 8. What are the top 5 states that had the most workplace fatalities from stabbings?

<pre>
SELECT state, COUNT (*)
FROM fatalities_cleaned
WHERE description LIKE "%stabb%"
GROUP BY state
ORDER BY 2 DESC
LIMIT 5
</pre>

#### Task 9. What are the top 10 states that had the most workplace fatalities from shootings?

<pre>
SELECT state, COUNT(*)
FROM fatalities_cleaned
WHERE description LIKE "%shot%"
GROUP BY state
ORDER BY 2 DESC
LIMIT 10
</pre>

#### Task 10. What is the total number of shooting deaths per year?

<pre>
SELECT YEAR (incident_date), COUNT (*)
FROM fatalities_cleaned
WHERE description LIKE "%shot%"
GROUP BY YEAR (incident_date)
ORDER BY 2 DESC
</pre>
