# Aggregation

## Revising Aggregations - The COUNT Function
-Easy

```sql
SELECT COUNT(*)
FROM CITY
WHERE POPULATION > 100000;
```

##Revising Aggregations - The SUM Function
-Easy

```sql
SELECT SUM(POPULATION)
FROM CITY
WHERE DISTRICT = 'California';
```

## Revising Aggregations - Averages
-Easy

```sql
SELECT AVG(POPULATION)
FROM CITY
WHERE DISTRICT = 'California';
```

## Average Population
-Easy

```sql
SELECT ROUND(AVG(POPULATION),0)
FROM CITY;
```

## Japan Population
-Easy

```sql
SELECT SUM(POPULATION)
FROM CITY
WHERE COUNTRYCODE = 'JPN';
```

## Population Density Difference
-Easy

```sql
SELECT MAX(POPULATION) - MIN(POPULATION)
FROM CITY;
```
## The Blunder
-Easy

```sql
SELECT (CEIL(AVG(Salary) - AVG(REPLACE(Salary, 0, '')))) AS diff -- Rounded-up difference between avg salary and avg salaries with zeroes removed
FROM EMPLOYEES;
```

## Top Earners
-Easy

```sql
SELECT
    (months * salary) AS total, -- Total salary for each employee
    COUNT(*) AS count -- Count of rows
FROM Employee
GROUP BY total -- Group by the total salaries
ORDER BY total DESC -- Get largest first
LIMIT 1; -- Limit to the top row to get the highest total along with the count column
```

## Weather Observation Station 2
-Easy

```sql
SELECT
    ROUND(SUM(LAT_N),2) AS latsum,
    ROUND(SUM(LONG_W),2) AS longsum
FROM STATION;
```

## Weather Observation Station 13
-Easy

```sql
SELECT 
    ROUND(SUM(LAT_N),4)
FROM STATION
WHERE LAT_N > 38.7880 
    AND LAT_N < 137.2345;
```

## Weather Observation Station 14
-Easy

```sql
SELECT
    ROUND(MAX(LAT_N),4)
FROM STATION
WHERE LAT_N < 137.2345;
```

## Weather Observation Station 15
-Easy

```sql
SELECT ROUND(LONG_W,4)
FROM STATION
WHERE LAT_N < 137.2345
ORDER BY LAT_N DESC
LIMIT 1; -- Using the ol' lop of the top row ploy
```

## Weather Observation Station 17
-Easy

```sql
SELECT ROUND(LONG_W,4)
FROM STATION
WHERE LAT_N > 38.7780
ORDER BY LAT_N ASC -- Use ASC to choose from the smallest LAT_N
LIMIT 1;
```

## Weather Observation Station 18
-Medium

```sql
SELECT
    ROUND((latdist + longdist),4) as mandist -- Manhattan distance is just the difference of X and Y of two points at a right angle
FROM (
SELECT 
    (SELECT (MAX(LAT_N) - MIN(LAT_N))) AS latdist, -- Calculate the difference between max lat and min lat
    (SELECT (MAX(LONG_W) - MIN(LONG_W))) AS longdist -- Same from longitude
    FROM STATION) AS sq
```
## Weather Observation Station 19
-Medium

```sql
SELECT
    ROUND(SQRT((latdist*latdist) + (longdist * longdist)),4) as eudist -- Euclidean distance = sqrt((x1-x2)^2 + (y1-y2)^2)
FROM (
SELECT
    (SELECT (MAX(LAT_N) - MIN(LAT_N))) AS latdist, -- This equates to x1-x2
    (SELECT (MAX(LONG_W) - MIN(LONG_W))) AS longdist -- This equates to y1-y2
FROM STATION) as sq
```

## Weather Observation Station 20
-Medium

It drives me (and probably everyone else) crazy that there is no 'easy' way to find the median of an array. See the explanation for this code [here.](https://sebhastian.com/mysql-median/)

```sql
SET @row_index := -1;
SELECT ROUND(AVG(sq.LAT_N),4) as median
FROM (
    SELECT @row_index:=@row_index + 1 AS row_index, LAT_N
    FROM STATION
    ORDER BY LAT_N
    ) AS sq
WHERE sq.row_index 
  IN (FLOOR(@row_index / 2) , CEIL(@row_index / 2));     
```

