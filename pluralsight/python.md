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

## Comparing with other programming languages
Everything in Python is an object, and almost everything has attributes and methods. e.g., all functions have a build-in attribute `__doc__`, which return the doc string defined in the function's source code. The `sys` module is an object which ahs an attribute called `path`.
* Statically typed language - A language in which types are fixed at compile time, e.g. Java and C
* Dynamically typed languate - A language in which types are discovered at execution time. Javascript, VBScript and Pythong/Ruby/Racket are dynamically typed, because they figure out what type a variable is when you first assign it a value.
* Strongly typed language - A language in which types are always enforced. Java and Python are strongly typed. If you have an integer, you can't treat it like a string whithout explicityly converting it.
* Weaky typed language - A language in which types may be ignored. Javascrip, VBScript are weakly typed. In VBScript, you can concatenate the string '12' and integer 3 to get the string '123. Then treat that as the integer 123, all without any explicity conversion.

### Python is both Dynamically typed and strongly typed language (because once a variable has datatype, it matters)
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

## Comments and `__doc__`
```python
# this is a oneline comment
""" this is multiple 
line comments
"""
```
A docstring is a string literal that occurs as the first statement in a module, function, class, or method definition. Such a docstring becomes the `__doc__` special attribute of that object.

## Indenting Code
Python functions have no explicit `begin` or `end`, and no curly braces to mark where the function code starts and stops. The only delimiter is a colon (`:`) and the indentation of the code block itself.
```python
def buildConnectionString(params):
    """Build a connnection string from a dictionary of parameters.
    returns string."""
    return ";".join(["%s=%s" % (k,v) for k,v in params.items()])
```
**Code blocks are defined by their indentation.** Here the "code blodk", means *functions*, `if` statements, `for` loops, `while` loops, and so forth. Indenting starts a block and unindenting ends it. There is no explicit braces, brackets, or keyworkds. This means **whitespace is significant, and must be consistent**. 

### Compare with Java
* Python use `:` and indentation to separate code blocks.
* Java use `{` and `}` and semicolons to separate code blocks.
* Python `:` plus indentation equal to Java `{}`
* Python carriage return equal to Java `;`
```python
"""This is python code for fibinacci"""
def fib(n):
    print 'n=', n
    if n>1:
        return n*fib(n-1)
    else:
        print 'end of the line'
        return 1
```
```Java
// This is Java code for fibinacci
int fib(int n){
    System.out.println("n="+n);
    if(n>1){
        return n*fib(n-1);
    }else{
        System.out.println("end of the line");
        return 1;
    }
}
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

5//2 # 2
5/2 # 2.5
```

## Fundamentals 
### Scalar types and Values
```python
# int - unlimited precision signed integer
0b10 # 2
0o10 # 8
0x10 : 16
int(3.5)
int(-3.5)
int("456") # 456
int("10000",3) # 81 3-based

# folat - double precision (64-bit), 53 bits of binary precision, 15 to 16 bits of decimal precision
3.125
3e8 # 300000000.0
1.616e-35
float(7)
float("1.345")
float("nan")
float("inf")
float)"-inf")

# None - the sole value of NoneType, represent the absence of a value. Not displayed by REPL.
a = None
a is None # True

# bool - True or False, yes capitialized
True
False
bool(0) # False
bool(0.0) # False
bool(-1) # True
bool([]) # False
bool("") # False
bool("False") # True
```

### Relational Operators
```python
==
!=
<
>
<=
>=
```

### Conditional Statements
```python
if expr:  # equal to bool(expr)
    print("expr is True")
elif: 
else:
    print("expr is False")
```

### Python while loop
```python
while expr:
    print("loop while expr is True")
    break
c=5
while c != 0
    print(c)
    c -= 1
    if(c==0) break
```


## Reference
* [Dive into Python](http://www.diveintopython.net/toc/index.html)


