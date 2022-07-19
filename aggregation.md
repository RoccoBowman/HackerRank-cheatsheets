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
SELECT (CEIL(AVG(Salary) - AVG(REPLACE(Salary, 0, '')))) AS diff
FROM EMPLOYEES;
```

## Top Earners
-Easy

```sql
SELECT
    (months * salary) AS total,
    COUNT(*) AS count
FROM Employee
GROUP BY total
ORDER BY total DESC
LIMIT 1;
```
