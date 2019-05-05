## `*list` and `**dict` can unpack list and dict into list arguments and [keyword arguments](https://treyhunner.com/2018/04/keyword-arguments-in-python/) separately.

````
def sub(a, b):
  print(a-b)
  
 sub(1,2) # -1
 sub(2,1) # 1
 sub(a=1,b=2) #-1
 sub(b=2,a=1) #-1
 sub(2,b=1) #1, keyword arg must follow positional, and not override
 
 print('a','b','c', sep=', ') # sep is a keyword arguments, rest are positional
```

### Positional and keyword(named) arguments

You can create a function that accept a number of positioanl args as well as keyworkd-only args by using `*` and then special keyword-only args AFTER `*`.

```
def product(*numbers, initial=1):
  total=initial
  for n in numbers:
    total *=n
  return total

product(4,4) # 16
product(4,4,initial=2) # 32
```

**Note**: `*` syntax capture all positional args into a tuple which numbers varialble points to, `initial` arg above must be spcified as keyword arg.
