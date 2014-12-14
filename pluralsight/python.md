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
Everything in Python is an object, and almost everything has attributes and methods. e.g., all functions have a build-in attribute `__doc__`, which return the doc string defined in the function's source code. The `sys` module is an object which has an attribute called `path`.
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

### print()
* similar to printf but with `%` instead of `,` to pass value.
`print('========%s' % word)`
* using pretty print
 ```python
 from pprint import pprint as pp
 m={'a':1,'b':2}
 pp(m) # {'a': 1, 'b': 2}
 
 ```
* redirect output with optional `file` argument. `print("I will be in file", file=sys.stderr)`

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

### Introspect (Reflection)
```python
import words

type(words) # <class 'module'>
dir(words) # show attributes of modules, for retrospection.
type(words.fetch_words)
dir(words.fetch_words)
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

### Dynamic Strong typed

|            |Static            |Dynamic         |
|:---------- |:-----------------|:---------------|
|**Strong**  |Haskell, C++, Java|Python, Ruby    |
|**Weak**    |                  |Javascript, Perl|

* Dynamic Type System: Object types are only resolved at runtime, not compile time
```python
def add(a,b):
   return a+b
add(5,7) # 12
add("hi","yo") # hiyo
add([1,2],[3,4]) #[1,2,3,4]
```
* Strong Type System: not implicit type conversion, except convert to bool in `if` statement
```python
add("hi",42) # TypeError: Can't convert 'int' object to str implicitly
```
* Object references have no type.( a='hi' then reassign a=12 is ok)
* **LEGB** rules: `Scopes` are `contexts` in which named `references can be looked up`. 
 * Local - inside the current function
 * Enclosing - any and all enclosing functions
 * Global - top-level of module
  * main, __name__, sys, urlopen, fetch_words (function names)
 * Build-in - provided by `buildints` module
 * Python name scopes do not correspond to source code blocks demarked by indentation. `for` loop and `with` does not introduce nested scopes. ( see below for `word` variable, it actually live in scope of function scope. The `for` does not introduce new scope)
 ```
def fetch_words(url):
    with urlopen(url) as story:
        story_words = []
        for line in story:
            line_words = line.decode('utf-8').split()
            for word in line_words:
                story_words.append(word)
    print('========%s' % word)
    return story_words
 ```
 * rebind with `global` keyword
 ```python
 count = 0
 def show_count():
     print("count =  ",count)
 def set_count(c):
     global count # rebind local count to global count. Whitout this, the next line create a local variable count. which shadows the global count.
     count = c
 ```

### Objects - summary
* Think of named references to objects rather than varibles
* The garbage collector reclaims unreachable objects
* `id()` returns a unique and constant identifier
* The `is` operato determins equality of identity
* test for equivalence using `==`
* Funciton arguments are passed by object-reference
* Reference is lost if a formal function argument is rebound
* `return` also pass by object-reference
* Function arguments can be specified with defaults
* Default argument expression evaluated once, when `def` is executed.
* Python use dynamic typing (do not need specify type in advance)
* Python use strong typing (type cannot be coerced)
* Names are lookup in four nested scopes LEGB
* Global refernce can be read from a local scope, and can be bind to local scope by using `global` key word
* Everything in Python is an object, includes modules and functions.
* `import` and `def` result in binding to named references
* `type` can be used to determine the type of an object
* `dir()` can be used to introspect an object and get its attributes
* The name of a function or module object can be accessed through its `__name__` attribute.
* The docstring for a function or module object can be accessed through its `__doc__` attribute
* use `len()` to measure the length of a string
* multiply a string by an integer (called "repetition" operation)

## Advanced python data types
* **str** - homogeneous immutable sequence of unicode codepoints (characters)
 * `len(s)` length
 * concantenation with `+, +=`, similar to Java, can degrate the performance
 * use `join` on the `separator` string on a list. `colors=';'.join(["r","g","b"])`
 * use `split` to divide a string into list. `colors.split(';')`. default divides by whitespace
 * use `partition` to divide string into three part tuple around a separator. 
 ```python
 "unforgetable".partition("forget") # ('un','forget','able')
 origin,_,_destination = "seattle-boston".partition('-') # use underscore as dummy name ofr the separator
 ```
 * use `format` to insert values into strings. 
 ```python
 # Integer field names matched with positional arguments.
"hello to {0} from {1}".format('Alex','grace') 

 # And filed names can be omitted if used in sequence. 
 "hello to {} from {}".format('Alex','Grace')
 # Access values through keywords
 "hello to {to} from {from}".format(from="Grace", to="Alex")
 # Access values through indexes
 colors=("r",'g','b')
 "Colors are {c[0]} {c[1]} {c[2]}".format(c=colors)
 # Access attributes using dot
 import math
 "Math constants: pi={m.pi}, e={m.e}".format(m=math)
 # Fomatting and alignment support
 "Math constants: pi={m.pi:.3f}".format(m.math)
```
* **range** - arithmetic progression of integers
 * `range(5)` with stop value 5, start from 0. i.e. range(0,5)
 * `range(startInclusive=0,stopExclusive, step=1)` - half open. i.e., include start but exclude stop, and progress by step.
```python
for i in range(5):
   print(i)
```
 * `enumerate` yield tuples of (index,value), use it to iterate through list with pair.
 ```python
 t=[10,9,8]
 for p in enumerate(t):
   print(p)
 # (0,10)
 # (1,9)
 # (2,8)
 
 # or use tuple unpacking
 for i,v in enumerate(t):
   print("i={}, v={}".format(i,v))
 ```
 * list(range(5,10)) - create a list.
* **list** - herterogeneous mutable sequence of objects
 * 0-based indexing
 * positive index from front
 * negative index from end, the last element is at index -1, avoid `seq[len(seq)-1]`
 * `append` to add new element to the end of list
 * `*` to repat lists. e.g., to initialize alist with constants
 * **repetition is shallow** because it generate multiple references to one instance in the produced list
 * `index(item)` to find elements 
 * `in` `not in` to test membership
 * `del list[index]` to delete element at index. `del` is a global function
 * `list.remove(val)` to remove item whose value is val
 * `list.insert(index, val)` to add value at position of index
 * `join(list)` to concatenate elements
 * `+` to concatenate 2 lists
 * `+=` or `extend()` to modify the assignee in-place.
 * `reverse()` and `sort(keyfun, reverse=True)`
 * `sorted()` build-in funciton sorts any interable series and returns a list
 * `reversed()` build-in function reverses any iterable series and return a reverse iterator
 ```python
 # slicing
 s[1:4]
 s[1:-1] # no first nor last
 s[3:] # from 3rd to end
 s[:3] # 0,1,2
 s[:x]+s[x:]=s
 
 # shallow copy
 full_slice=s[:] # copy all s, important idiom for copying lists.
 s.copy()
 v=list(s)
 
 # repetation
 c=[1,2]
 d=c*2 # d=[1,2,1,2]
 
 # initalization
 e=[0]*2 # [0,0], which is a multiple references to one instance of the constant 0 in the produced list
 
 # finding
 w="the quick brwon fox jumps over the lazy dog".split()
 i=w.index('fox') # 3
 i=w.index('lion') # Value Error: 'lion' is not in list
 w.count('the') #2
 "tiger" in w # False
 
 # concatenate
 m=[1,2]
 n=[3,4]
 m+n # [1,2,3,4]
 m+=n # m=[1,2,3,4]
 m.extend([5,6]) # m=[1,2,3,4,5,6]
 ```
 
* **dict** - unordered mapping from unique, immutable keys (string,number,tuples) to mutable values (object)
 * `dict(names_and_values)` where names_and_values maybe:
  * an iterable series of key-value 2-tuples.
  * keyword arguments - requires keys are valid Python identifiers
  *  ampping, such as another dict
 * `copy()` to copy dictionary
 * `dict(anotherDict)` to copy 
 * `update(anotherDict)` to extend (or merge) with another dictionary in-place. The old value will be replace with new value.
 * iterate with `keys()` for loop, or `values()` for value.
 * `in` and `not in` for membership testing for keys.
 * `del d[key]` to delete item by key
 * `m[newKey]=newValue` to add new key-value pair
 ```python
 names_and_ages=[('Alice',12), ('bob',3)]
 d=dict(names_and_ages) # {'Alice':12, 'bob':3}
 dict(Alice=12,bob=3) # {'Alice':12, 'bob':3}
 dict(anotherDict)
 
 for key,value in d.items():
   print("{key}=>{value}".format(key=key,value=value))
 for key in d.keys():
   print("{key}=>{value}".format(key=key,value=d[key]))
 for value in d.values():
   print(value)
 ```
 
* **tu(ju:)ple** - heterogeneous immutable sequence of objects
 * packing/unpacking vs pattern matching 
 * `a,b=b,a` is the idiomatic Python swap
 * tuple constructor allow converting from list to tuple `tuple([1,2]), tuple("Hi")`
 * membership testing: `1 in (1,2)` or `3 not in (1,2)`.

```python
t=("hi",3.1415926,100)
t[0] # hi
len(t)
for item in t:
   print(item)
t+(333)
a=((1,2),(3,))
len(a) # 2

h=(1,) # This is an single element tuple 
h=(1) # this is an integer

t=() # empty tuple

# tuple packing/unpacking
def minmax(items):
   return min(items), max(items) # return tuple - auto-packing
lower, upper = minmax([2,1,4,3]) # lower=1,upper=4 - auto unpacking, delimiting parentheses are optional for one or more elements

# (similar to pattern matching) tuple unpacking works with arbitrary nested tuples
(a,(b,(c,d))) = (4,(3,(2,1))) # a=4,b=3...

a,b = b,a # a=3,b=4
```
* **set** - unordered collection of unique, immutable objects
 * `{1,2}` delimited by `{` and `}`, use `set()` for empty. since `{}` is for dict.
 * can be constructed from a list with duplicates discarded.
 * iterate witn `for x in {1,2}`
 * `in` and `not in` for membership testing
 * `add(item)` to add new item, duplicate will be ignored silently
 * `update(iterable)` to update multiple elements
 * `remove(elemnt)` to remove element with error if no element
 * `discard(element)` to remove without throw error
 * `copy()` shallow
 * set algebra: `union(s)`,`intersection(s)`, `difference(s)`, `symmetric_difference(t)`, `issubset(s)`, `issuperset(t)`, `isdisjoint(t)`
 ```python
 s=set()
 s=set([1,2,1]) # {1,2}
 ```
 
* **protocols** (interface)
 * to implement a protocol, objects must support certain oeprations
 * `iterable`,`sequence` and `container`

## Exception handling
A mechanishm for stopping "normal" program flow and continue at some surrounding context or code block.
* Raise an exception to interrupte the program flow
* Handle an exception to resume control
* Unhandled exceptions will terminate the program
* Exception objects contains information about the exceptional event
* Exceptions for **programmer errors**, should nonormally being catch
 * IndentationError, SyntaxError, NameErrors
* Exception can be stringified by `str(e)`.
* Multiple exceptions can be catched together `except(ValueError,TypeError) as e:`
* exception can be raised by `raise exObj`
* Callers need to know that exceptions to expect and when
* Standard exceptions are often the best choice, use common or existing exception types when possible.
 * IndexError, ValueError, KeyError
 * Avoid catch TypeError
* Python perfer **It's Easier to Ask Forgiveness than Permission** to *Permission to Look Befor You Leap*. i.e., do it and handle exception, rather than test everything befor action
* Platform-specific modules are implemented with try catch ImportError (EAFP)
```python
# Syntax
try:
except(ErrorType1,ErrorType2..) as e:
finally:

# exceptional.py
"""A module for demonstrating exceptions."""

def convert(s):
    """"convert to an integer"""
    try:
        return int(s)
    except (ValueError, TypeError) as e:
        print("Conversion error: {}".format(str(e)), file=sys.stderr)
        raise
    except ZeroDivisionError as e:
        print(e, file=sys.stderr)
        raise ValueError("failed")
    finally:
        print("call end")

from exceptional import convert
convert("33")
convert('hi') # -1

import os
def make_at(path,dir_name):
  original_path=os.getcwd()
  try:
    os.chdir(path)
    os.mkdir(dir_name)
  finally:
    os.chdir(original_path)
```

## Comprehensions, Iterarbles and Generators
### Comprehensions
* list comprehensions
 * format: `[expr(item) for item in iterable]`, e.g. `[len(word) for word in words]`, evaluate expr for each element.
 ```python
words = "hello world!".split()

f=[len(word) for word in words] 
type(f) # <class 'list'>
# same as 
lengths=[]
for word in words:
   lengths.append(len(word))
 ```
* set comprehensions
 * format: `{expr(item) for item in iterable }
* dictionary comprehensions
 



## Reference
* [Dive into Python](http://www.diveintopython.net/toc/index.html)


