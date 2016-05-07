## Type classes

Type classes is not a `class` in object oriented programming. In scala, it is just a `trait` with one or more type 
parameters, and they usaully stateless. Methods in trait define some kind of behavior with type parameters. 
It also define some implicit companion objects, which will provide conversion for diff parameter types.

This kind of reuse code other than from inherit, is kind of like adapter pattern. The implicit value(or member) of 
the typeclass, adapt typa A to type B[A].

```scala
// file NuberLike.scala
object Math{
  trait NumberLike[T]{
    def plus(x:T,y:T):T
    def minus(x:T,y:T):T
  }
  object NumberLike{
    implicit object NumberLikeDouble extends NumberLike[Double] {
      def plus(x:Double,y:Double):Double = x+y
      def minus(x:Double,y:Double):Double = x-y
    }
    implicit object NumberLikeInt extends NumberLike[Int] {
      def plus(x:Int,y:Int):Int = x+y
      def minus(x:Int,y:Int):Int = x-y
    }
  }
}

// file Statistics.scala
object Statistics {
  import Math.NumberLike
  // def median[T:NumberLike](xs: Vector[T]): T = xs(xs.size/2) // this is context bound
  def sum[T](xs: Vector[T])(implicit ev: NumberLike[T]):T = xs.reduce(ev.plus(_,_))
}

// main.scala
median[Int](Vector(1,2,3) // 2
sum[Int](Vector(1,2,3) // 6
```

## Context bound

A context bound `A:B` means that an implicit value of Type `B[A]` must be available. And it
is really equivalent to have a second implict parameter list with a NumberLike[T] in it. i.e.

```scala
// The following two are equivalent, but the first is context bound, and it's syntax sugar.
def sum[T:NumberLike](xs:Vector[T]):T
def sum[T](xs:Vector[T])(implicit ev: NumberLike[T]):T
```

## See also 
* [What is context bounds](http://stackoverflow.com/questions/4465948/what-are-scala-context-and-view-bounds)
* [Type classes](http://danielwestheide.com/blog/2013/02/06/the-neophytes-guide-to-scala-part-12-type-classes.html)
