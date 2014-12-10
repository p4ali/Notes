# Overview
* Strings and Collections
* Modularity
* Build-in Types and Object Model
* Collection Types
* Handling Exceptions
* Comprehensions, iterables, and generators
* Definen new Types with classes
* Files and resource management
* Shipping working and maintainable code

## Dynamic strong typed language
* ductyping
* Batteries included : no need 3rd libraries
* Significant white space rules
 * prefer 4 spaces
 * Never mix spaces and tabs
 * Be consistent on consecutive lines
 * Only deviate to improve readability

## Zen of Python
* Beautiful is better than ugly
* Explicity is better than implicit
* Simple is better than coplex
* Complex is better than complicated
* Flat is better than nested
* Sparse is better than dense
* Readability counts
* Errors should never pass silently
* If the implementation is hard to explain, it's a bad idea.
* Namespaces are one honking great idea

## Comments
```python
# this is a oneline comment
""" this is multiple 
line comments
"""
```

## Using standard library
```python
import math
math.sqrt(81)

help(math)
help(math.factorial)

from math import factorial
factorial(5)/factoria(3)

from math import factorial as fac
fac(n)/fac(k)

fac(n)//fac(n-1)

n=100
len(str(fac(n)))
```





