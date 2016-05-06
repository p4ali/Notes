## Stream
Stream is very similar to List, except it uses call-by-name for tail. i.e., A Scala Stream is the head of the stream and a function that can construct the rest of the Stream. So if you want to avoid computing the tail of a 
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

// and infinite random stream
def make : Stream[Int] = Stream.cons(util.Random.nextInt(10), make)
```
## Lazy Evaluation
Scala use strict evaluation by default, but allows lazy evaluaiton of value definitions with the lazy val form.
```Scala
// lazy val
lazy val y= {print "y";2}
// calll by name parameter
def lazyFun(p:=>Int) { // => indicate call-by-name, argument is not evaluated at the point of function application, but instead is evaluated at each use within the function.
  print p
}
```

## Computing with ininite sequences
Avoid filter which require the whole seq and then remove the undesired elements. Try to generate the desireable 
elements from begining.
```scala
// define all natural numbers
// a #:: b means the stream which is element a put in front of the stream b
def from(n:Int):Stream[Int]= n #:: from(n+1)
val nats=from(0)
```
