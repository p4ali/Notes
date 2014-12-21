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

## `apply` method
In mathematics and computer science, **apply** is a funciton that applies functions to arguments. It serves the purpose of closing the gap between Object-oriented and Functional paradigms in Scala. Every function in scala can be represented as an object. Every function also has an OO type: for instance, a function that takes an `Int` paramter and returns an `Int` will have OO type of **Function[Int,Int]**.  Every function (and everything else) in Scala is an object, and every object can be treated as a funciton, provided it has the `apply` method. Such objects can be used in the function notation:
```scala
object Foo {
  var y = 5
  def apply(x:Int) = x+y
}

printf(Foo(1)) // 6 - using Foo object in function notation
```
When `f` is a reference to function object and write `f(args)` to apply arguments to the represented function, the compiler silently expands `f(args)` tot he object method call `f.apply(args)`.
