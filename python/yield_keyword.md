## [The `yield` statement:](https://docs.python.org/2.4/ref/yield.html)

* is only used when define a generator fuction, 
* and is only used in the body of generator function.
* using a a `yield` in a function deinition will cause that definition to create a generatior funciton instead of a normal function

When `yield` is excecuted, the state of generator will be frozen and value of expression_list is returned to the `next()`'s caller.
By`frozen` it means all local states and pc, stack are saved, so that next time `next()` is called, the function can proceed exactly as if 
the `yield` statement were just another external call.

```
# iterable with `[]` - repeatable calls
iter=[x*x for x in range(3)]
for i in iter:
  print(i)
>>
0
1
4
for i in iter:
  print(i)
>>
0
1
4

# Generator with `()` - one time only, no repeatable calls
gen=(x*x for x in range(3))
for i in gen:
  print(i)
>>
0
1
4
for i in gen:
  print(i)
>>

# Yield
def yieldGen():
  for i in range(3):
    yield i*i

mygen = yieldGen() # creat a generator, no repeatable calls
print(mygen) # mygen is an object
for i in mygen:
  print(i)

```
