## Currying: (named after Haskell Curry who invent this)
If a function ```fn x*y -> z``, then curreing is to have a function take the first comceptual argument ```x``` and return 
another function that takes the second conceptual argument ```y``  and return ```z``` so on.

## Closure
A Function value has two pars, the *code* for the function and the *environment that was current when we created the 
function*. These two parts do really form a "pair". When evalates the code part, you will using the environment part.

This "pair" is called a *function closure• or just *closure*. The code itself can have *free variables* (variables that
are nto bound inside the code so they need to be bound by some outer environment), the closure carries with it an 
environment that provides all these bindings. So the closure overall is "closed" -- it has everything it needs to 
produce a function result given a function argument.
