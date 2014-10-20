## Currying: (named after Haskell Curry who invent this)
If a function ```fn x*y => z``, then curreing is to have a function take the first comceptual argument ```x``` and return
another function that takes the second conceptual argument ```y``  and return ```z``` so on. The function with only partial conceptual arguments called *partial application*
```SML
val sorted = fn x=> fn y=> fn z=> z>y andalso y>=x
(((sort 4)5)6) = true
(* parenthis is optional *)
sort 4 5 6 = true

(* the following is called partial application *)
sort 4 
sort 4 5

(* alternative way to define currying, separating conceptual arguments by spaces rather than anonymous functions *)
fun sorted x y z = z>y and also y>=x

```

## Closure
A Function value has two pars, the *code* for the function and the *environment that was current when we created the 
function*. These two parts do really form a "pair". When evalates the code part, you will using the environment part.

This "pair" is called a *function closure• or just *closure*. The code itself can have *free variables* (variables that
are nto bound inside the code so they need to be bound by some outer environment), the closure carries with it an 
environment that provides all these bindings. So the closure overall is "closed" -- it has everything it needs to 
produce a function result given a function argument.

## Lexical Scope
The body of a function is evaluated in the environment where the funciton is **defined**, not the environment where
the function is called.

## Returning functions

```SML

fun increment x = x+1
val increment = fn x=>x+1

fun triple_n_times(n,x) = ntimes((fn y=>3*y), n, x))

fun double_or_triple f= 
if f7
then fn x=> 2*x
else fn x=> 3*x
```

