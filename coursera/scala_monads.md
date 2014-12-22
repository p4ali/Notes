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
In the literature, `flatMap` is more commonly called **bind**.
* `List` is a monad with `unit(x) = List(x)`
* `Set` is monad with `unit(x) = Set(x)`
* `Option` is a monad with `unit(x)=Some(x)`

`flatMap` is an operation on each of these types, whereas `unit` in scala is different for each monad.

## Monad and map
`map` can be defined for every monad as a combination of `flatMap` and `unit`:
```scala
m map f == m flatMap (x=>unit(f(x))
        == m flatMap (f andThen unit)
```

## Monda Laws
To qualify as a monad, a type has to satisfy three laws:
* Assciativity
` m flatMap f flatMap g == m flatMap(x => f(x) flatMap g)`
* Left Unit
` unit(x) flatMap f == f(x)`
* Right Unit
` m flatMap unit == m`
