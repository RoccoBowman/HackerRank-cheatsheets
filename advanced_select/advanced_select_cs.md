## Type of Triangle

```sql
SELECT
CASE
    WHEN A+B <= C OR B+C <= A OR A+C <= B THEN 'Not A Triangle' -- If sum of two sides is greater than or equal to the third
    WHEN A=B AND B=C THEN 'Equilateral' -- If all sides are equal                  
    WHEN A=B AND A!=C OR B=C AND B!=A OR A=C AND A!=B THEN 'Isosceles' -- If two sides are equal and the third is different
    WHEN A!=B AND B!=C THEN 'Scalene' -- If no sides are equal
END
FROM TRIANGLES;
```
## Occupations

I had no idea with this one. I tried PIVOT and CROSSTAB to no avail. Whilst searching for "Transpose table in MySQL" I found an answer [on stack overflow.](https://stackoverflow.com/questions/71652696/transpose-a-table-in-mysql) accidently and couldn't look away. Why use MAX to wrap CASE WHEN? It's confusing but apparently it turns what would otherwise be diagonal data into vertical data that fits into the columns. Check [this out.](https://stackoverflow.com/questions/71544038/using-max-with-case-when-in-mysql#:~:text=The%20aggregation%20goes%20outside%20%28wraps%20round%29%20the%20CASE,has%20rotated%2090%20degrees%2C%20from%20vertical%20to%20horizontal.)

```sql

WITH -- Uses a CTE to add row numbers to the original table, partitioned by occupation
enumerated AS (
    SELECT name, occupation, ROW_NUMBER() OVER (PARTITION BY occupation ORDER BY name) row_numbers
    FROM OCCUPATIONS
)
SELECT MAX(CASE WHEN occupation='Doctor'    THEN Name END) AS Doctor,    
       MAX(CASE WHEN occupation='Professor' THEN Name END) AS Professor, 
       MAX(CASE WHEN occupation='Singer'     THEN Name END) AS Actor,     
       MAX(CASE WHEN occupation='Actor'    THEN Name END) AS Singer  
FROM enumerated
GROUP BY row_numbers
```


## The PADS
```sql
SELECT CONCAT(Name,'(', LEFT(Occupation,1),')') -- Using LEFT again to grab the first letter of the occupation and concatenating to name with parentheses
FROM OCCUPATIONS
ORDER BY Name; -- This first query has the formatting correct for the data to be summarized now.

SELECT CONCAT('There are a total of ', COUNT(*),' ', LOWER(Occupation),'s.') -- Now we use COUNT to count the number of occupations
FROM OCCUPATIONS
GROUP BY OCCUPATION -- And don't forget to group by when using aggregate functions
ORDER BY COUNT(*) ASC, OCCUPATION ASC;
```

## Binary Tree Nodes
```sql
SELECT
CASE 
    WHEN P IS NULL THEN CONCAT(N,' ','Root') -- The root with have no parent in the "P" column so that's easy to find
    WHEN N NOT IN (SELECT DISTINCT P FROM BST WHERE P IS NOT NULL) THEN CONCAT(N,' ','Leaf') -- Leaf nodes won't be found in the parent column!
    ELSE CONCAT(N,' ','Inner') -- Everything else (most) is thus an inner element
END
FROM BST
ORDER BY N ASC;
```

## New Companies
```sql
SELECT
    company_code,
    founder,
    COUNT(DISTINCT lm_code) AS lmcount, -- Count the unique values of lead manager codes (there are many duplicates due to the joining below)
    COUNT(DISTINCT sm_code) AS smcount, -- rinse and repeat
    COUNT(DISTINCT m_code) AS mcount,
    COUNT(DISTINCT e_code) AS ecount
FROM ( -- Perhaps not elegant, but transparent. join all the tables together so all the data is in one place and choose FROM there!
    SELECT 
        c.company_code,
        c.founder,
        lm.lead_manager_code AS lm_code,
        sm.senior_manager_code AS sm_code,
        m.manager_code AS m_code,
        e.employee_code AS e_code
    FROM company AS c
    LEFT JOIN Lead_Manager AS lm ON (c.company_code = lm.company_code)
    LEFT JOIN Senior_Manager AS sm ON (lm.company_code = sm.company_code)
    LEFT JOIN Manager AS m ON (sm.company_code = m.company_code)
    LEFT JOIN Employee AS e ON (m.company_code = e.company_code)
    ) AS subquery
GROUP BY company_code, founder -- group by the two left identifying columns so each copmany and founder has a tally
ORDER BY company_code;
```
## Contest Leaderboard
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
