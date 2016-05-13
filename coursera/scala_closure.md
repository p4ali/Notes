## Closure
A **closure** is a function, whose return value depends on the value of one or more variables declared outside this function.
The binding of variable to the function is done at compile time, i.e., the variable in the visible scope are binded to the function. 
Once binded, redefine the variable has not affect on the function.

```scala
var factor = 3
val multiplier = (i:Int) => i*factor

multiplier(1) // 3

factor=2
multiplier(1) // 2

var factor = 4
multiplier(1) // 2

factor=4
multiplier(1) //2
```

## Function with Side-affect
It's not recommended to change the external variables from within the function, although scala does not prevent you from doing that.
The side-affect of changing external variable is conflict with functional programming, in the following case, same argument will cause
different result.

```scala
var count=1

val multiplier = (i:Int) => { count+=1; i*count}
multiplier(2) // 4
multiplier(2) // 6
```
