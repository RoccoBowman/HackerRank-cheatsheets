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
