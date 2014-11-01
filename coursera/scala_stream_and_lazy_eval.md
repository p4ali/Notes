## Stream
Stream is very similar to List, except it uses call-by-name for tail. So if you want to avoid computing the tail of a 
sequence until it is needed or the evaluation result (which might be never)
The tail is evaluated only on demand.
```scala
val xs=Stream.cons(1,Stream.cons(2,Stream.empty))
Stream(1,2,3)
x#::xs == Stream.cons(x,xs) // #:: can be used in expression as well as patterns

object Stream{
  def cons[T](hd: T, tl: => Stream[T]) = new Stream[T]{
    def isEmpty = false
    def head = hd
    lazy val tail = tl
  }
  val empty = new Stream[Nothing]{
    def isEMpty = true
    def head = throw new NoSUchElementException("Empty head")
    def taili = throw new NoSuchElementException("Empty tail")
  }
}
```
## Lazy Evaluation
Scala use strict evaluation by default, but allows lazy evaluaiton of value definitions with the lazy val form.
```Scala
// lazy val
lazy val y= {print "y";2}
// calll by name parameter
def lazyFun(p:=>Int) {
  print p
}
```

## COmputing with ininite sequences
Avoid filter which require the whole seq and then remove the undesired elements. Try to generate the desireable 
elements from begining.
```scala
// define all natural numbers
def from(n:Int):Stream[Int]= n#:::from(n+1)
val nats=from(0
)
```
