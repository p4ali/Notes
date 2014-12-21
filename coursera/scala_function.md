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

## Companion object
A companion object is an object with the same name as a class or trait and is defined in the same source file as the associated file or trait. A companioin object differs from other objects as it has access rights to the class/trait that other objects do not. In particular it can access methods and fields that are private in the class/trait.

An analog to a companion object in Java is having a class with static methods. In scala you would move the static methods to a Companion object. The object will have `apply` methods wich will return an instance of the object.
```scala
class MyString(val jString:String) {
  private var extraData = ""
  override def toString = jString+extraData
}
object MyString {
  def apply(base:String, extras:String) = {
    val s = new MyString(base)
    s.extraData = extras
    s
  }
  def apply(base:String) = new MyString(base)
}
println(MyString("hello"," world"))
println(MyString("hello"))
```

## extractors and `case` class
When defining a match such as `case Tuple2(one, two)` the methods `Tuple2.unapply` and `Tuple2.unapplySeq` are called to see if that case can match the input. If one of methods return a `Some(...)` object then the case is considered to be a match. These methods are called **Extractor** methods because they essentially decompose the object into several parameters.

## Partial functions
```scala
val f: String => String = {case "ping" => "pong" }
f("ping") // pong
f("abc") // MatchError

// rewrite in partial function
val f: PartialFunction[String, String] = {case "ping" => "pong" }
f.isDefinedAt("ping") // true
f.isDefinedAt("pong") // false

// In scala, the following partial function trait is defined 
trait PartialFunction[-A, +R] extends Function[-A,+R] {
  def apply(x:A): R
  def isDefinedAt(x:A): Boolean
}
```
If the expedted type is a PartialFunction, the Scala compiler will expand  `{case "ping"=>"pong"}` as follows
```scala
new PartialFunction[String, String] {
 def apply(x:String) = x match {
  case "ping" => "pong"
 }
 def isDefinedAt(x:String)=x match{
  case "ping" => true
  case _ => false
}
```

## Collection Recap

### Hierarchy:
* Itarable
 * Seq
  * IndexedSeq
   * Vector
   * ...Array(Java)
   * ...String(Java)
  * LinearSeq
   * List
 * Set
 * Map

### Core methods
All collection types share a common set of general methods
* map
* flatMap
* filter
* foldLeft
* foldRight

Idealized implmentation on Lists:
```scala
abstract class List[+T]{
 def map[U](f: T=>U): List[U] = this match {
  case x::xs => f(x)::xs.map(f)
  case Nil => Nil
 }
 def flatMap[U](f: T=>List[U]): List[U] = this match {
  case x::xs => f(x)++xs.flatMap(f)
  case Nil => Nil
 }
 def filter(p: T=>Boolean): List[T] = this match {
  case x::xs => if(p(x)) x::xs.filter(p) else xs.filter(p)
  case Nil => Nil
 }
}
```
In practice, the implementation and type of these methods different in order to
* make them apply to arbitrary collections, not just lists
* make them tail-recursive on lists (otherwise, stack overflow for too large list)

### For-expression
The `for` expression simplify the comibination of core methods `map`,`flatMap`,`filter`.
```scala
// instead of 
(1 until n) flatMap(i=>
 (1 until i) filter (j=>isPrime(i+j)) map(j=>(i,j)))
// using for
for{
 i<- 1 until n
 j<- 1 until i
 if isPrime(i+j)
}yield(i,j)
```
