## Populations Census
[Easy]

Given the CITY and COUNTRY tables, query the sum of the populations of all cities where the CONTINENT is 'Asia'.

```sql
SELECT SUM(CITY.POPULATION)
FROM CITY
JOIN COUNTRY ON (CITY.COUNTRYCODE = COUNTRY.CODE)
WHERE CONTINENT = 'Asia';
```
## African Cities
[Easy]

Given the CITY and COUNTRY tables, query the names of all cities where the CONTINENT is 'Africa'.

```sql
SELECT CITY.NAME
FROM CITY
LEFT JOIN COUNTRY ON (CITY.COUNTRYCODE = COUNTRY.CODE)
WHERE CONTINENT = 'Africa';
```

## Average Population of Each Continent
[Easy]

Given the CITY and COUNTRY tables, query the names of all the continents (COUNTRY.Continent) and their respective average city populations (CITY.Population) rounded down to the nearest integer.

```sql
SELECT 
    COUNTRY.CONTINENT, 
    FLOOR(AVG(CITY.POPULATION))
FROM CITY
LEFT JOIN COUNTRY ON (CITY.COUNTRYCODE = COUNTRY.CODE)
WHERE COUNTRY.CONTINENT IS NOT NULL
GROUP BY COUNTRY.CONTINENT;
```
## Top Competitors
[Medium]

Julia just finished conducting a coding contest, and she needs your help assembling the leaderboard! Write a query to print the respective hacker_id and name of hackers who achieved full scores for more than one challenge. Order your output in descending order by the total number of challenges in which the hacker earned a full score. If more than one hacker received full scores in same number of challenges, then sort them by ascending hacker_id.

Strategy: Because we need to find the number of full-score challenges per hacker, we will need a GROUP BY in order to aggregate by hacker and make sure their aggregated value is greater than one indicated more than one full score on challenges. To grab the full scores for aggregation, we need to make sure that the submission scores are the same as the full score as outlined in the difficulty table. To compare these columns, we need to first join all the tables together on appropriate join keys.

```sql
SELECT h.hacker_id, h.name
FROM Submissions AS s -- Started with submissions since it is the table with the most info
LEFT JOIN Challenges AS c ON (s.challenge_id = c.challenge_id)
LEFT JOIN Difficulty AS d ON (c.difficulty_level = d.difficulty_level)
LEFT JOIN Hackers as h ON (s.hacker_id = h.hacker_id) -- Join all the tables together on common columns
WHERE (s.score = d.score) --The score of each submission should be the "full" score depending on its difficulty level
GROUP BY h.hacker_id, h.name -- Group by each participant and their id number
HAVING COUNT(*) > 1 -- Only grab hackers with more than one full score
ORDER BY COUNT(*) DESC, hacker_id ASC;
```

## Ollivander's Inventory
[Medium]
Write a query to print the id, age, coins_needed, and power of the wands that Ron's interested in, sorted in order of descending power. If more than one wand has same power, sort the result in order of descending age.

Strategy: We first need to find the cheapest wands with the same age and power and compare that to the entire stock; in this case, we need a subquery in the conditional WHERE clause to specify which is the cheapest. Because of the subquery's location in the WHERE clause, only one column can be returned using MIN() here. We have to join the tables to match up the age and power based on code but then condition that power in this joined table must be the same as that in the original joined data--same for age. This almost mirrors a GROUP BY statement and aggregates the cheapest cost to the same power and ages. All that is left is the join the original data and specify the columns and order.

```sql
SELECT
    w.id,
    wp.age,
    w.coins_needed,
    w.power
FROM Wands AS w
LEFT JOIN Wands_Property AS wp ON (w.code = wp.code)
WHERE wp.is_evil = 0
    AND w.coins_needed = (SELECT MIN(coins_needed) AS cheapest
                          FROM Wands AS w2
                          LEFT JOIN Wands_Property AS wp2 ON (w2.code = wp2.code)
                          WHERE w2.power = w.power AND wp2.age = wp.age)
ORDER BY w.power DESC, wp.age DESC;
```

## Contest Leaderboard
[Medium]
The total score of a hacker is the sum of their maximum scores for all of the challenges. Write a query to print the hacker_id, name, and total score of the hackers ordered by the descending score. If more than one hacker achieved the same total score, then sort the result by ascending hacker_id. Exclude all hackers with a total score of 0 from your result.

Strategy: This problem is another one that relies on aggregation, so first we need to think about how to aggregate maximum scores for each hacker's challenges. We use a subquery in the FROM clause as we only want to sum the maximum scores (one column) from the subquery). The subquery groups hackers by id and name, as well as the unique challenges and selects the maximum score per hacker and challenge. All that is left is to select from this subquery the sum of maximum score and groupb by hacker where the sum score is not zero!

```sql
SELECT
    hacker_id,
    name,
    SUM(max) AS total_score -- We will add the top scores per hacker and challenge
FROM(SELECT -- Similar to the subquery in the new company problem; join hackers to submissions
        h.name,
        h.hacker_id,
        s.challenge_id,
        MAX(score) AS max -- find the max score for each submission
     FROM Hackers AS h
     LEFT JOIN Submissions AS s ON (h.hacker_id = s.hacker_id)
     GROUP BY h.name, h.hacker_id, s.challenge_id) AS subquery -- finish the subquery by grouping by hacker name, id, and challege
                                                               -- this leaves us with all the top scores per hacker, per challenge
GROUP BY hacker_id, name -- Then we just have to group by hacker_id, and name, the sum of top scores per hacker and challenge is appended
HAVING SUM(max) > 0
ORDER BY total_score DESC, hacker_id ASC;
```

## The Report
[Medium]
Ketty gives Eve a task to generate a report containing three columns: Name, Grade and Mark. Ketty doesn't want the NAMES of those students who received a grade lower than 8. The report must be in descending order by grade -- i.e. higher grades are entered first. If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically. Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in descending order. If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order.

Strategy: In this case I didn't use any kind of JOIN since the two tables have no obvious keys between them. I used CASE WHEN to determine grades for each student score and another to make the sub-8 score names "NULL". 

```sql
SELECT     
    CASE WHEN Marks < 70 THEN 'NULL' ELSE NAME END AS Name,
    CASE
        WHEN Marks BETWEEN 0 AND 9 THEN 1
        WHEN Marks BETWEEN 10 AND 19 THEN 2
        WHEN Marks BETWEEN 20 AND 29 THEN 3
        WHEN Marks BETWEEN 30 AND 39 THEN 4
        WHEN Marks BETWEEN 40 AND 49 THEN 5
        WHEN Marks BETWEEN 50 AND 59 THEN 6
        WHEN Marks BETWEEN 60 AND 69 THEN 7
        WHEN Marks BETWEEN 70 AND 79 THEN 8
        WHEN Marks BETWEEN 80 AND 89 THEN 9
        WHEN Marks BETWEEN 90 AND 100 THEN 10
    END AS Grade,
    Marks
FROM Students
ORDER BY Grade DESC, Name ASC;
```
## Challenges
[Medium]
Julia asked her students to create some coding challenges. Write a query to print the hacker_id, name, and the total number of challenges created by each student. Sort your results by the total number of challenges in descending order. If more than one student created the same number of challenges, then sort the result by hacker_id. If more than one student created the same number of challenges and the count is less than the maximum number of challenges created, then exclude those students from the result.

Strategy: Like Ollivander's Inventory, we need to place subqueries in the WHERE or, in this case, HAVING clause because we are ultimately comparing and filtering with aggregated tables. First we need to find the count of challenges per hacker in the first subquery and extract only the unique values for completed challenges so that we do not have duplicates (though still including the maximum ones which we want to keep). The second subquery creates another condition where if the challenge count is the maximum (50) then keep it.

```sql
SELECT 
    h.hacker_id, 
    h.name, 
    COUNT(c.challenge_id) AS challenge_counter
FROM hackers h
JOIN challenges c
    ON h.hacker_id = c.hacker_id
GROUP BY h.hacker_id, h.name

HAVING challenge_counter IN (
    SELECT sq1.counter
    FROM(
        SELECT hacker_id, COUNT(challenge_id) AS counter 
        FROM challenges
        GROUP BY hacker_id
        ORDER BY counter DESC
    ) AS sq1
    GROUP BY sq1.counter 
    HAVING COUNT(sq1.counter) = 1
)

OR

challenge_counter = (
    SELECT MAX(sq2.counter)
    FROM(
        SELECT hacker_id, COUNT(challenge_id) AS counter
        FROM challenges
        GROUP BY hacker_id
        ORDER BY counter DESC
    ) AS sq2)

ORDER BY challenge_counter DESC, h.hacker_id ASC;
```
