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
### Vaiable binding
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
def f(args: String*) = ... //varargs parameter, use as an Array[String]
val list = List("a", "b", "c")
f(list : _*)
```
It can also be used to explode an array. The following create a Map from a List:
```scala
Map(List(Pair(1, 2), Pair(3, 4)): _*) // Map(1 -> 2, 3 -> 4)
```

## References
* [Twitters Scala School](https://twitter.github.io/scala_school/)
* [Scala Tour](http://www.scala-lang.org/old/node/104)
* [This week in scala blogs](http://www.cakesolutions.net/teamblogs)
* [Typesafe blog and newsletter](https://typesafe.com/company/news)
