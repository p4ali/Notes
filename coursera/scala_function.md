## Curring

> In computer science, currying, invented by Moses Schönfinkel and Gottlob Frege, is the technique of transforming a function that takes multiple arguments into a function that takes a single argument (the other arguments having been specified by the curry).

Methods may define multiple parameter lists. When a method is called with a fewer number of parameter lists, 
then this will yield a function taking the missing parameter lists as its arguments.
Here is an example:
```scala
object CurryTest extends Application {

  def filter(xs: List[Int], p: Int => Boolean): List[Int] =
    if (xs.isEmpty) xs
    else if (p(xs.head)) xs.head :: filter(xs.tail, p)
    else filter(xs.tail, p)

  def modN(n: Int)(x: Int) = ((x % n) == 0)

  val nums = List(1, 2, 3, 4, 5, 6, 7, 8)
  println(filter(nums, modN(2))) // List(2,4,6,8)
  println(filter(nums, modN(3))) // List(3,6)
}
```
Note that method modN is partially applied in the two filter calls; i.e. only its first argument is actually applied. 
The term *modN(2)* yields a function of type Int => Boolean and is thus a possible candidate for the second argument 
of function filter.
