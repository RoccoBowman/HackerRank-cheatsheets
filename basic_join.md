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

## Contest Leaderboard
-Medium

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
