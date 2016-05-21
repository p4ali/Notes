## Function vs Method
Function is not method. A method can be converted to a function by eta expansion. Function is an object which has `apply` method.
Object can become a function as far as it has `apply` method.

The type of Function and Method are different. The below showing the difference. e.g., function is `String => String = <function1>`,
yet method is `(str: String)String`.

```scala
// function definition. All following should work, and they are different forms of definition
val f1: String=>String = (str: String)=>str+str // String => String = <function1>
val f2 = (str: String)=>str+str 
val f3: String=>String= _*2 // Note, you can not _ + _ , since the Nth _ means Nth arguments. But this function has only 1 argument.
val f4: Function1[String,String] = _*2
val f5: String => String = new Function1[String,String]{
  def apply(str: String): String = {
    str+str
  }
}

// method definition
def f(str: String)=str+str // (str: String)String

// convert method to function by eta expansion ` _`. Yes WHITESPACE followed by underscore.
val g = f _ // eta expansion to conver method to function

// function has apply method
f("1") // 11
f.apply("1") //11
```

## FunctionN
It turns out, that every Function you’ll ever define in Scala, will become an instance of an Implementation which will feature a certain Function Trait. There is a whole bunch of that Function Traits, ranging from `Function1` up to `Function22`. 

A Function with n arguments will end up with FunctionN. 
```scala
trait Function3[-T1, -T2, -T3, +R] extends AnyRef {
    ...
    def apply( v1 :T1, v2 :T2, v3 :T3 ) : R
    ...
}
```

And `String=>String` is just a syntax sugar of `Function1[String,String]`, so the following are same:
```scala
// f1 and f2 are both valid, but f1 != f2, although they have same bahaivor
val f1: String=>String = _*2
val f2: Function1[String,String] = _*2
```

## Tupling a function
Tupling a function is simply adapting `FunctionN[A1, A2, ..., AN, R]` to a `Function1[(A1, A2, ..., AN), R]`, where `An` is input
type, and `R` is return type.

`Function.tupled` will convert the input parameters into tuple instead of list.

```scala
// f and g equally bahaves
val f: ((Int,Int))=>Int = { case (a,b) => a+b } // f: ((Int,Int))=>Int
f(1,2) //3

val g: (Int,Int)=>Int = +_+ // f: (Int, Int)=>Int = <function2>
val h = g.tupled // or Function.tupled(g), h: ((Int,Int)=>Int = <function1> , YES, it's function1
h(1,2) // 3

val k:((Int,Int))=>Int = Function.tupled(_+_) // k: ((Int,Int)=>Int = <function1>
```

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

## Partial functions vs Function
A **function** works for every argument of the defined type. e.g., `f:Int->String` takes any Int and return a String
A **Partial function** is only defined for certain values of the defined type. A partial function (Int)=>String might not accept every Int. `isDefinedAt` is a method on `PartialFunction` that can be used to determin if the PartialFunction will accept a given argument.

A case statement is a subclass of function called a PartialFunciton. Collection of multiple case statements are multiple PartialFunctions composed together.

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

A PartialFunction is a Function, so whenever a Function is required, you can switch with PartialFunction, for example:
```scala
val m:Map[Int,int]=Map(1->2,2->3,3-4)
// the following 2 return same result, note the 2nd with curly {}, not parenthesis ()
m.filter(e=>e._2>2)
m.filter{case (k,v) => v>2}
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
The scala comipler tranlate for-expression in terms of `map`,`flatMap` and a lazy variant of `filter`, i.e., `withFilter`.
```scala
// given
for(x<-e1) yield e2
// the compiler translate to
e1.map(x=>e2)

// given
for(x<-e1 if x;s) yield e2
// the compiler translate to
for(x<-e1.withFilter(x=>f);s) yield e2

// given 
for(x<-e1;y<-e2;s) yield e3
// the compiler translate to, a recursion with the following map until all for is translated
e1.flatMap(x=> for(y<-e2;s) yield e3)
```

The left-hand side of a generator may also be a pattern
```scala
val data:List[JSON]=...
for{
 JObj(bindings) <- data
 JSeq(phones) = bindings("phoneNumbers")
 JObj(phone) <- phones
 JSt(digits) = phone("number")
 if digits startsWith "212"
 
} yield (bindings("firstName"), bindings("lastName"))

// the reason above works is because compiler tranlate
pat <- expr 
// to
x <- expr withFilter {
 case pat => true
 case _ => fasle
} map {
 case pat => x
}
```
