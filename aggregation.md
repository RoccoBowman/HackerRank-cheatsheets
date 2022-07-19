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
