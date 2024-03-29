## Say "Hello, World!" with Python

```python
if __name__ == '__main__':
    print "Hello, World!"
```

## Python If-Else

```python
#!/bin/python3

import math
import os
import random
import re
import sys



if __name__ == '__main__':
    n = int(input().strip())
if (n % 2) == 0 and n > 20:
    print('Not Weird')
elif (n % 2) == 0 and n in range(2,5):
    print('Not Weird')
elif (n % 2) == 0 and n in range(6,20):
    print('Weird')
else:
    print('Weird')
```

## Arithmetic Operations

```python
if __name__ == '__main__':
    a = int(input())
    b = int(input())
    print(a + b)
    print(a - b)
    print(a * b)
```

## Python: Division

```python
if __name__ == '__main__':
    a = int(input())
    b = int(input())
    print(a//b)
    print(a/b)
```

## Loops

```python
if __name__ == '__main__':
    n = int(input())
    for i in range(0,n,1): #Use n here, as Python ranges are non-inclusive (if n = 5, will stop at 4)
        print(i**2)
```

## Write a function
[Medium]

```python
def is_leap(year):
    leap = False
    
    if (year % 400) == 0 and (year % 100) == 0: # If the year is divisible by 400 and 100, it is a leap year
        leap = True
    elif (year % 400) == 0 and (year % 100) > 0: # If the year is divisible by 400 but not 100, it is not a leap year
        leap = False
    elif (year % 4) == 0 and (year % 100) == 0: # If the year is divisble by 4 AND 100, it is not a leap year
        leap = False
    elif (year % 4) == 0 and (year % 100) > 0: # If the year is divisible by 4 but not 100, it is a leap year
        leap = True
    return leap

year = int(input())
print(is_leap(year))
```
## Print Function
[Easy]

Strategy: I couldn't figure this one out on my own because I could only do it with string functions or on multiple lines. However, it turns out that the print function in Python has an "end" parameter. This parameter defaults to "\n" or a line return, but if "end" is set to '' or an empty character, the following numbers will print on the same line!

```python
if __name__ == '__main__':
    n = int(input())
    for i in range(1,n+1,1):
        print(i, end='')
```
## Alphabet Rangoli

```python
alphabet = list(map(chr, range(97, 123)))

def print_rangoli(n):
    all_lines=[] # Create an empty set that will store each line created in each iteration in the for loop below
    for i in range(n):
            line_i = "-".join(alphabet[i:n]) # For each iteration, creates a string that contains letters and dash seperators (if n = 3, then i=1 would be a-b-c, i=2 would be a-b ...)
            all_lines.append((line_i[::-1] + line_i[1:]).center(4 * n - 3, "-")) # appends the last letter in the string in the center of a new string, where the length is determined by (4*n-3) and the fill character parameter ('-') when no other characters are specified

    print('\n'.join(all_lines[:0:-1]+all_lines)) # Print a space for each line and print successive lines 
            
print_rangoli(5)
```
