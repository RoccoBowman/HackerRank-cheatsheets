## Type of Triangle

```sql
SELECT
CASE
    WHEN A+B <= C OR B+C <= A OR A+C <= B THEN 'Not A Triangle'
    WHEN A=B AND B=C THEN 'Equilateral'
    WHEN A=B AND A!=C OR B=C AND B!=A OR A=C AND A!=B THEN 'Isosceles'
    WHEN A!=B AND B!=C THEN 'Scalene'
END
FROM TRIANGLES;
```
## Occupations

```sql
WITH
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
SELECT CONCAT(Name,'(', LEFT(Occupation,1),')')
FROM OCCUPATIONS
ORDER BY Name;

SELECT CONCAT('There are a total of ', COUNT(*),' ', LOWER(Occupation),'s.')
FROM OCCUPATIONS
GROUP BY OCCUPATION
ORDER BY COUNT(*) ASC, OCCUPATION ASC;
```

## Binary Tree Nodes
```sql
SELECT
CASE 
    WHEN P IS NULL THEN CONCAT(N,' ','Root')
    WHEN N NOT IN (SELECT DISTINCT P FROM BST WHERE P IS NOT NULL) THEN CONCAT(N,' ','Leaf')
    ELSE CONCAT(N,' ','Inner')
END
FROM BST
ORDER BY N ASC;
```

## New Companies
```sql
SELECT
    company_code,
    founder,
    COUNT(DISTINCT w) AS lmcount,
    COUNT(DISTINCT x) AS smcount,
    COUNT(DISTINCT y) AS mcount,
    COUNT(DISTINCT z) AS ecount
FROM (
    SELECT 
        c.company_code,
        c.founder,
        lm.lead_manager_code AS w,
        sm.senior_manager_code AS x,
        m.manager_code AS y,
        e.employee_code AS z
    FROM company AS c
    LEFT JOIN Lead_Manager AS lm ON (c.company_code = lm.company_code)
    LEFT JOIN Senior_Manager AS sm ON (lm.company_code = sm.company_code)
    LEFT JOIN Manager AS m ON (sm.company_code = m.company_code)
    LEFT JOIN Employee AS e ON (m.company_code = e.company_code)
    ) AS subquery
GROUP BY company_code, founder
ORDER BY company_code;
```
## Contest Leaderboard
```sql
SELECT
    hacker_id,
    name,
    SUM(max) AS total_score
FROM(SELECT
        h.name,
        h.hacker_id,
        s.challenge_id,
        MAX(score) AS max
     FROM Hackers AS h
     LEFT JOIN Submissions AS s ON (h.hacker_id = s.hacker_id)
     GROUP BY h.name, h.hacker_id, s.challenge_id) AS subquery
GROUP BY hacker_id, name
HAVING SUM(max) > 0
ORDER BY total_score DESC, hacker_id ASC;
```
