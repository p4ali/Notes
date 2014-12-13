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
* Sparse is better than dense: two empty lines between top level functions
* Readability counts
* Errors should never pass silently
* If the implementation is hard to explain, it's a bad idea.
* Namespaces are one honking great idea

## Python Excution Model
* When are function s defined?
* What happens when a module is imported?

### Static Lexical Scope
The body of a function is evaluated in the environment where the funciton is defined, not the environment where the function is called.
```python
def foo():
   x=4
   def bar():print(x)
   bar()
   x=5
   bar()
   def bar2():
      x=1
      bar() # get x from the scope where it is defined, i.e. body of foo, not where it was called - bar2
   bar2()

foo()
# output
4
5
5
```
### setting up a `main()` function with a `commandline argument`

### python module, script and program
* python module: convenient import with API
* python script: convenient exceution from command line
* python program: perhaps composed of many modules

## Comments and `__doc__`
```python
# this is a oneline comment
""" this is multiple 
line comments
"""
```
A docstring is a string literal that occurs as the first statement in a module, function, class, or method definition. Such a docstring becomes the `__doc__` special attribute of that object.

### Documenting code using `docstrings`
```python
#!/usr/bin/env python3
# the '#!' pronounced as "shebang". It tells env to run command python3 with this file. The command line will be './workds.py <url>', you need to change 755 surely.
"""Retrieve and print words from a URL.

Usage:

    python3 words.py <URL>

"""
import sys
from urllib.request import urlopen


def fetch_words(url):
    """Fetch a list of words from a URL.

    Args:
        url: The URL of a UTF-8 text document.

    Returns:
        A list of strings containg the words from the document.
    """
    with urlopen(url) as story:
        story_words = []
        for line in story:
            line_words = line.decode('utf-8').split()
            for word in line_words:
                story_words.append(word)
    return story_words


def print_items(items):
    """Print items one per line.

    :param items: An iterable series of printable items
    :return: None
    """
    for item in items:
        print(item)


def main():
    """Print each word from a text document from a URL

    :return: None
    """
    url = 'http://sixty-north.com/c/t.txt'
    print('len=%d' % len(sys.argv))
    if len(sys.argv) > 1:
        url = sys.argv[1] # agrv[0] is the file name of py file.

    words = fetch_words(url)
    print_items(words)


if __name__ == '__main__':
    main()

```


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
    print('n=', n)
    if n>1:
        return n*fib(n-1)
    else:
        print('end of the line')
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

### Augmented assignment operators
```ppytho
n=1
n+=2 # n=3
```

### Requesting test from user with `input()`

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
elif: # flat better than nested else if
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

### functions
A function is a block of organized, reusable code that is used to perform a single, related action. Functions provide better modularity for your application and a high degree of code reusing.
* Function blocks begin with the keyword def followed by the function name and parentheses ( ( ) ).
* Any input parameters or arguments should be placed within these parentheses. You can also define parameters inside these parentheses.
* The first statement of a function can be an optional statement - the documentation string of the function or docstring.
* The code block within every function starts with a colon (:) and is indented.
* The statement return [expression] exits a function, optionally passing back an expression to the caller. A return statement with no arguments is the same as return `None`.
* keyword argument is also used. If keywork arguement is used, then the order of argument does not matter
* **default argument values are evaluated when def is evaluated, and only evaluated once**.
```python
def functionname( parameters ): # paramenters can be value in order, or key-value in any order
   "function_docstring"
   function_suite
   return [expression]
def banner(message, border='-'):
    line = border*len(message)
    print(line)
    print(message)
    print(line)
banner('hello')
banner(border="*", message="hi")
banner('hi', border="+")

# example   
def square(x=10): # 10 is the default value of x. 
    "compute the square of a number"
    return x*x
square(5) # 25
square() # 100
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

## String
* `str` is immutable squences of Unicode codepoints. Single or double quote.
* Python3 has universal newline `\n`. Apply to windows, linux and osx.
* escape string by `\`
* raw string with `r`. e.g. `r'c:\Users\ali\MS Office\docs`, what you type is what you get.
* use `str` constructor to create string. e.g., `str(46)`, `str(1e10)`
* access single char in str by `[]`, e.g., `s="hello",s[1]==e`
* No separate Char type.
* `help(str)`
* ecoded by UTF-8. e.g. `'Vi er s\u00e5'`, `\xe5`, `\345` (== å). 
* adjacent string literal concatenation. e.g. `c="a" "b" # c=="ab"`

```python
str
"It's a good thing"
'hello'

# multi-line string by """ or ''' or embeded \n inside the line or use print

# escape string by \
"This is a \" in a string"
```
## Bytes
* Immutable sequences of bytes.
* Converting between strings and byte: call `mystring.encode('gb2312')` to convert a string to bytes, and call `mybytes.decode('gb2312')` to convert bytes to a string.
* Think convertion between string and bytes in case of networking. before sending string, you need encode it, and after recieved, decode to string.
```python
b'data'
b"data"

d=b'some bytes'
d.split() # [b'some', b'bytes']

mystring="hello world"
b = mystring.encode('utf-8)
type(b) # ensure bytes are type of bytes
b.decode('utf-8')

```

## List
* mutable sequences of objects (can be different types)
* `[1,2] a[1]==2, a[1]=100`
* `append` to the end of list
* use `list` constructor to create a new list. e.g.,`list("hi") # ['h','i']`
```python
# lists
b=[1,2,3]
b.append('hello') # b=[1,2,3,'hello']

# can be spread in multiple lines. neat
c = ['bear',
     'giraffe',
     'elephant',
     'catepillar',]
```

## dictionary
* mutable mappings of keys to values `{k1:v1,k2:v2}`
* constructor `dict`

```python

d={'a':1,'b':2}
d['a']==1
d['a']=100 # {'a':100,'b':2}
e={}
e['a']=222
```

## loop
```python
'''for item in iterable:
    body'''
colors={'red':0xff0000,'green':0x00ff00,'blue':0x0000ff}
for color in colors:
   print(color,colors[color])
    
"""while True:
    body # may contain 'break'"""
```

## with-statement
`with expression [as variable]  BLOCK` statement is supposed to replace `try ... catch ...`, the `expression` should result in an Context Manager object, which has `__enter__()` and `__exit()__` methods, the following will be executed in order:
* evaluate `expression`, get contextManager
* variable=contextManager.__enter__(), similar to Java `try`
* execute BLOCK
* contextManager.__exit__(), similar to Java `catch ... finally ...`

```python
from urllib.request import urlopen
with urlopen('http://sixty-north.com/c/t.txt') as story: # binds response to variable story
    story_words = []
    for line in story:
        line_words = line.decode('utf-8').split()
        for word in line_words:
            story_words.append(word)

```

## Modularity
* Python code is placed in `*.py` files called **module**s.
* Modules can be executed directly with `python3 module_name.py`
* You can bring module into the REPL or other modules with `import module_name`
* Named functions defined with `def` keyword `def function_name(arg1,argn):`
* Use `__name__` to determine how the module is being used
 * if `__name__=="__main__"` the module is being executed directly as a program
* mudule can **use** other modules as far as no cycle dependencies.
* To user module, use `import mymodule`, where `mymodule` is the basename of the file `mymodule.py`.
* module code only be executed once on the first import.
* Command line arguments are accessible through `sys.argv`, the script file name is in `sys.argv[0]`
```python
# mymodule.py
def square(x):
    return x*x
print(__name__)
if(__name__ == '__main__'): # True when run in REPL, false when import into script file.
    square(5)
    
# on REPL or another .py file
import mymodule
# or 
from words import *

mymodule.square(5) # 25
```

### special attributes in Python
* delimited by **double underscores**, e.g., `__name___`

## Finding and browsing with `help()`

## Object Model
* Variable is named referrence to object (integer is immutable?)
* Variable assignment actually assign object reference
* `id()` identifier of object. `is` test identity. e.g., `a is b`
* Value equality vs. identity
```python
   p=[1,2]
   q=[1,2]
   p==q # True for value equlity
   p is q # False for identity equality
```
* function call by object reference

|:---------- |:-----------------|:..............-|
|            |Static            |Dynamic         |
|Strong      |Haskell, C++, Java|Python, Ruby    |
|Weak        |                  |Javascript, Perl|


## Reference
* [Dive into Python](http://www.diveintopython.net/toc/index.html)


