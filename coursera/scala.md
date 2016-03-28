## Programming Paradigms

* Imperative programming - modify mutable variables using assiments and control structures.
 * theory does not define mutationss
  * one or more data types
  * operations on these types
  * laws that describe the relationship between values and operation

* Evaluation
 * substitution model
  * call-by-name and call-by-value
  * if CBV terminates, then CBN terminates, and same value. But the other direction is not true.
  * by default, scala use CBV, and if you want to CBN, put an '=>' in front of the type, e.g.
  ```scala
  def constOne(x: Int, y: => Int) = 1 // YES, there is space between : =>
  ```
* Conditionsals and Value Definitions
```scala
def and(x:Boolean, y: =>Boolean) = if (x) y else false
```

* Persistent data structure 
 * cornerstone of scaling functional prog up to collections. 
 * Older version data structure unchanged
* Dynamic dispatching - subtype
* Type erasure - template
* eta-expansion - replace function type with function value.
* Liskov substituion principle
```
Let q(x) be a property provable about objects x of type T. Then q(y) should be provable for objects y of type S where S is a subtype of T.
```
* Natural Induction (to show a property P(n) for all the integers n>=b)
 * show that we have P(b) (base case)
 * for all integers n>=b show the induction step: *if one has P(n), then on also has P(n+1)*
* Structure induction
* Referential transparency

## Monad
[Monad is Elephant](http://james-iry.blogspot.com/2007/09/monads-are-elephants-part-1.html)
[Introduction to Monads in Scala](http://www.slideshare.net/stasimus/introduction-to-monads-in-scala-1)
## Some idioms
### Implicit Parameters
```scala
def msort[T](xs: List[T])(implicit ord: Ordering) = ...
  def merge(xs: List[T], ys:List[T]) = 
    ... if (ord.lt(x,y)) ...
    ... merge(msort(fst),msort(snd))

// The calls to msort can avoid the ordering parameters
   msort(nums); msort(fruits)
// The compiler will figure out the right implicity to pass based on the demanded type.
// * is marked **implicit**
// * has a type compatible with T
// * is visible at the point of the function call, or is defined in a companion object associated with T
// * if there is a single(most specific) definition, it will be taken, otherwise error
```
### Pattern Matching from
 * *constructor*, e.g, NUmber, Sum
 * *variables*, e.g., n,e1,e2
 * *wildcard pattern*, _,
 * *constants*, e.g. 1, true
 * Generally, teh function value {case p1=>e1 ... case pn=>pn} is equivalent to x=>x match {case p1=>e1...case pn=>en}
```scala
(List(1,2,3) zip List(10,20,30)).map{case (x,y) => x*y}.sum

trait Expr
case class Number(n:Int) extends Expr
case class Sum(e1:expr, e2:expr) extends Expr

def eval(e:Expr):Int = e match{
  case Number(n) => n
  case Sum(e1,e2) => eval(e1)+eval(e2)
}

def describe(x:Any) = x match{
  case 5 => "Five" // variable/constant patterns
  case true => "truth"
  case "hello" => "Hi!"
  case Number(0) => "0" // Constructor patterns
  case Nil => "The empty list" // constant patterns
  case (a,b,c) => "Tuple" // tuple patterns
  case List(0,_*) => "A sequence start with 0" // sequence patterns
  case s:String => s.length // typed patterns
  case m:Map[_,_] => m.size // typed patterns
  case _=> "something else"
}
```
### Variable binding
```scala
expr match{
  case UnOp("abs",e@Unop("abs",_)) => e // e=Unop("abs",_)
  case _=>
```
### Named parameters
When calling methods and functions, you can use the name of the variables expliclty in the call, like map:
```scala
def printName(first:String,last:String)={
  println(first+", "+last)
}
printName(first="ALex", last="Li") // eq. printName("Alex","Li")
```

### By name parameter
A by-name parameter is formed by putting ```=>``` between the name and type, i.e., ```name: => Type```
```
def runTwice(body: => Unit)={
  body
  body
}
```

### `_` as default value
```scala
class Reference[T]{
 private var contents: T=_ // default value is 0 for numeric type, false for boolean type, () for Unit and null for other
}
```

### For comprehensive
```scala
val pairs = for (x<-0 until 4; y<-0 until 4) yield (x,y) # seq[(Int, Int)]
```

### Iterate Collection by `case`
```scala
  def sum(nums: Iterable[Int]): Int = nums match {
    case x :: xs => x + sum(xs)
    case Nil => 0
  }
  sum(Seq(1,2,3,4,5))
```

### Syntax sugar: _* for treating Seq as method parameters
Generally the **:** notation is used for type ascription, forcing compiler to see a value as some particular
type.
```scala
val b=1:Byte
val f=1:Float
```
It can also ascribes the special varargs type. It can mirrors the asterisk notation used for declaring
a varargs parameter and can be used on a vaiable of any type that subclasses **Seq[T]**:
```scala
def f(args: String*) = ... //varargs parameter, use as an Array[String], * called repeatent parameter
val list = List("a", "b", "c")
f(list : _*)
```
It can also be used to explode an array. The following create a Map from a List:
```scala
Map(List(Pair(1, 2), Pair(3, 4)): _*) // Map(1 -> 2, 3 -> 4)
```
### String interpolation
Interpolcated strings are preceded with an `s` character, and can contain `$` symbols with arbitruary identifiers.
```
val magic=7
val myMagicNumber=s"My Magic Number is $magic"
```
### `apply` - [default method](https://www.safaribooksonline.com/library/view/learning-scala/9781449368814/ch08.html#apply_method_section)
Scala compiler will converts `f(a)` into `f.apply(a)`, where `f` could be either a method name or class constructor. That's why the following code works (think apply as `method_missing` in ruby):
```
var seq=Seq(1,2,3)
trait Seq[+A] extends .. {
 ...
 def apply[A](elems: A*): CC[A] = {
    if (elems.isEmpty) empty[A]
    else {
      val b = newBuilder[A]
      b ++= elems
      b.result
    }
  }
 }
```

### [sealed and final](http://underscore.io/blog/posts/2015/06/02/everything-about-sealed.html)
`sealed` basically tells compiler to perform exhaustiveness checking for pattern matches within the file in which it defined.

A `sealed` trait/[class](http://naildrivin5.com/scalatour/wiki_pages/SealedClasses/) can only be extended in the defining file. 
A `final` class cannot be extended anywhere(however, a final class does NOT get exhaustiveness checking).

## Genericity
```scala
// file Reference.scala
class Reference[T] {
 private var contents: T = _
 def set(value: T) { contents = value }
 def get: T = contents
}

// file IntegerReference.scala
object IntegerReference {
 def main(args: Array[String]) {
  val cell = new Reference[Int]
  cell.set(13)
  println("Reference contains the half of " + (cell.get * 2))
 }
}
```

## [Partial function](http://blog.bruchez.name/2011/10/scala-partial-functions-without-phd.html)
scala's collection's [collect](http://ochafik.com/blog/?p=393) use Partial function.
```scala
val fraction = new PartialFunction[Int, Int] {
  def apply(d: Int) = 42 / d
  def isDefinedAt(d: Int) = d != 0
}

// collect
val pf: PartialFunction[A, B] = ...
someCollection.collect(pf)
//Is roughly equivalent to :
someCollection.filter(pf.isDefinedAt _).map(pf)
```

## References
* [Twitters Scala School](https://twitter.github.io/scala_school/)
* [Scala Tour](http://www.scala-lang.org/old/node/104)
* [This week in scala blogs](http://www.cakesolutions.net/teamblogs)
* [Typesafe blog and newsletter](https://typesafe.com/company/news)
* [Language spec](http://www.scala-lang.org/old/sites/default/files/linuxsoft_archives/docu/files/ScalaReference.pdf)
* [Scala 20 pages overview](http://www.scala-lang.org/old/sites/default/files/linuxsoft_archives/docu/files/ScalaOverview.pdf)
