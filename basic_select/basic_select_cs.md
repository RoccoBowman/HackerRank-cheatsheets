# HackerRank

## SQL Basic Select Cheat Sheet

### Revising the Select Query I
```{sql eval=FALSE}
SELECT *
FROM CITY
WHERE population > 100000 
  AND CountryCode = 'USA';
```

### Revising the Select Query II
```{sql eval=FALSE}
SELECT NAME
FROM CITY
WHERE population > 120000 
    AND CountryCode = 'USA';
```

### Select All
```{sql eval=FALSE}
SELECT *
FROM CITY;
```

### Select By ID
```{sql eval=FALSE}
SELECT *
FROM CITY
WHERE ID = 1661;
```

### Japanese Cities' Attributes
```{sql eval=FALSE}
SELECT *
FROM CITY
WHERE COUNTRYCODE = 'JPN';
```

### Japanese Cities' Names
```{sql eval=FALSE}
SELECT NAME
FROM CITY
WHERE COUNTRYCODE = 'JPN';
```

### Weather Observation Station 1
```{sql eval=FALSE}
SELECT CITY, STATE
FROM STATION;
```

### Weather Observation Station 3 
```{sql eval=FALSE}
SELECT DISTINCT CITY -- DISTINCT looks for unique values only
FROM STATION
WHERE ID % 2=0; -- % 2=0 is the code for looking for even only
```

### Weather Observation Station 4
```{sql eval=FALSE}
SELECT COUNT(CITY) - COUNT(DISTINCT CITY)
FROM STATION;
```

### Weather Observation Station 5
```{sql eval=FALSE}
SELECT CITY, length(CITY) -- Use length to find, duh, length; not len()
FROM STATION
ORDER BY length(CITY) ASC, CITY ASC -- Sort length ascending to find smallest
LIMIT 1; -- Limit 1 to find the smallest in the list

SELECT CITY, length(CITY)
FROM STATION
ORDER BY length(CITY) DESC, CITY ASC -- Sort length descending to find largest
LIMIT 1; -- Limit 1 to find largest in the list
```

### Weather Observation Station 6
```{sql eval=FALSE}
SELECT DISTINCT CITY
FROM STATION
WHERE (CITY LIKE 'a%' -- use a % to denote "anything else"
    OR CITY LIKE 'e%' -- so this would be starting with "e" plus anything else after
    OR CITY LIKE 'i%'
    OR CITY LIKE 'o%'
    OR CITY LIKE 'u%'); -- caveman style :)
```

### Weather Observation Station 7
```{sql eval=FALSE}
SELECT DISTINCT CITY
FROM STATION
WHERE (CITY LIKE '%a' -- reverse the % placement to find anything before plus X
       OR CITY LIKE '%e'
       OR CITY LIKE '%i'
       OR CITY LIKE '%o'
       OR CITY LIKE '%u');
```

### Weather Observation Station 8
```{sql eval=FALSE}
SELECT DISTINCT CITY
FROM STATION
WHERE (LEFT (CITY, 1) IN ('a','e','i','o','u') -- more elegant!; had to look this up
    AND RIGHT (CITY, 1) IN ('a','e','i','o','u')); -- left(column, position) gets first position
                                                  -- "IN" lets us specify a search list
```

### Weather Observation Station 9
```{sql eval=FALSE}
SELECT DISTINCT CITY
FROM STATION
WHERE LEFT(CITY, 1) NOT IN ('a','e','i','o','u'); -- use "NOT IN" to reverse the previous query
```

### Weather Observation Station 10
```{sql eval=FALSE}
SELECT DISTINCT CITY
FROM STATION
WHERE RIGHT(CITY, 1) NOT IN ('a','e','i','o','u');
```

### Weather Observation Station 11
```{sql eval=FALSE}
SELECT DISTINCT CITY
FROM STATION
WHERE LEFT(CITY,1) NOT IN ('a','e','i','o','u')
    OR RIGHT(CITY,1) NOT IN ('a','e','i','o','u'); -- Just subbing OR for AND here
```

### Weather Observation Station 12
```{sql eval=FALSE}
SELECT DISTINCT CITY
FROM STATION
WHERE LEFT(CITY,1) NOT IN ('a','e','i','o','u')
    AND RIGHT(CITY,1) NOT IN ('a','e','i','o','u'); -- Including AND for neither 
```

### Higher Than 75 Marks
```{sql eval=FALSE}
SELECT Name 
FROM STUDENTS
WHERE Marks > 75
ORDER BY RIGHT(Name,3) ASC, ID ASC; -- Using right again here but with "3" to denote last three letters
```

### Employee Names
```{sql eval=FALSE}
SELECT name
FROM Employee
ORDER BY name ASC; -- Easy as frick
```

### Employee Salaries
```{sql eval=FALSE}
SELECT name
FROM Employee
WHERE salary > 2000
    AND months < 10
ORDER BY employee_id ASC;
```
