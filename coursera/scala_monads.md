## Monad
Data structure with `map` and `flatMap` seem to be quite common. These class of data structures together with some 
algebraic lays that they should have called **monads**

A **monad** `M` is a parametric type `M[T]` with two operations, `flatMap` and `unit`, that have to satisfy some laws:
```scala
trait M[T] {
  def flatMap[U](f: T=>M[U]): M[U]
}
def unit[T](x:T): M[T]
```
IN the literature, `flatMap` is more commonly called **bind**.

