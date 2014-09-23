## Curring

> In computer science, currying, invented by Moses Schönfinkel and Gottlob Frege, is the technique of transforming a function that takes multiple arguments into a function that takes a single argument (the other arguments having been specified by the curry).

What this is saying is that the currying process transforms a function of two parameters into a function of one parameter which returns a function of one parameter which itself returns the result.  In Scala, we can accomplish this like so:
```scala

def add(x:Int, y:Int) = x + y
 
add(1, 2)   // 3
add(7, 3)   // 10
And after currying:

def add(x:Int) = (y:Int) => x + y
 
add(1)(2)   // 3
add(7)(3) 

```
