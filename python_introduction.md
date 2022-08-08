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
