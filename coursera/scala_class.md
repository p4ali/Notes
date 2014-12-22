
## `new` to create a instance of class (case class does not need `new`)
```scala
// Data abstraction

// Type - class
class Rational(x: Int, y: Int) {
  require(y != 0, "denomitor must be nonzero")

  // Multiple constructor
  def this(x:Int) = this(x,1)
  
  private def gcd(x:Int, y:Int): Int = if(y==0) x else gcd(y, x%y)
  private val g = gcd(x,y)

  def numer = x/g
  def denom = y/g
  
  def add(that: Rational, s: Rational): Rational =
    new Rational(this.numer*that.denom + that.numer*this.denom, that.denom*this.denom)
  
  override def toString() = numer + "/' + denom
  
}

// Value - object
val x = new Rational(1,2)
x.numer
x.demon

val y = new Rational(2,3)

x.add(y)
```

## **this** - self reference to the object on which the current method is executed
```scala
trait Generator[+T] {
  self => // an alias for "this"
  def generate: T
  def map[S](f:T=>S): Generator[S] = new Generator[S]{
    def generate = f(self.generate) // self =Geneartor.this
  }
}
```

## **require** - test performed when object is create

## **assert** - check the code of the function itself

## operator

* Any method with one parameter can be used like an **infix** operator
```scala
r add s // r.add(s)
```
* Symbolic + Alphanumeric as identifiers
```scala
x1 * +?%& vector_++ counter_=

def unary_ : Rational = new Rational(-number, denom)
```
## Trait
Both Java and Scala can only have one superclass.

Trait can be used to multi-inherit. You can create a class by **with** multiple trait.

Class, objects and traits can inherit from at most one class but arbitrary many traits.

A **trait** is declared like an astract class, with trait instead of abstract.
```scala
class Square extends Shape with Planar with Movable ...
```

Traits resemble interfaces in Java, in addition, they can contain fields and concrete methods.

On the other hand, traits cannot have (value) parameters, only classes can.

## Orgnization - package
```scala
import week3.Rational // named imports - import only Rational
import week3._ // wildcard imports - import everything from package week3
improt week3.{Rational, Hello} // import both Rational and Hello

// All members of package scala, java.lang, and scala.Predef are automatically imported

// www.scala-lang.org/api/current
```

## Class hierarchy
* Any - the base type of all types. Define methods: '==', '!='
* AnyRef - the base type of all reference types
* AnyVal - the base type of all premitive types
* Nothing - subtype of very other type. There is no value of type Nothing. e.g., an element type of empty collection. set[nothing]. Or throw exception.
* Null - type of null value of reference types.

```scala
if (true) 1 else false // AnyVal = 1
```

## Polymorphism
* subtyping: instances of a subclass can be passed to a base class
* generics: instance of a function or class are created by type parameterization

val is only evaluated at construct time. def evaluated each time is called.
```scala
// Generalize parameter definition using a Type parameter
trait List[T]{
 def isEmpty: Boolean
 def head: T
 def tail: List[T]
}
class Cons[T](val head: T, val tail: List[T]) extends List[T]{
 def isEmpty = false
}
class Nil[T] extends List[T]{
 def isEmpty: Boolean = true
 def head: Nothing = throw new NoSucnElementException("Nil.head")
 def tail: Nothing = throw new NoSuchElementException("Nil.tail")
}

// Generic Functions
def singleton[T](elem: T) = new Cons[T](elem, new Nil[T])
singleton[Int](1)
singleton[Boolean](true)
```
## Diff case class and class
Case classes can be seen as plain and immutable data-holding objects that should exclusively depend on their constructor arguments.

This functional concept allows us to

* use a compact initialisation syntax (Node(1, Leaf(2), None)))
* decompose them using pattern matching
* have equality comparisons implicitly defined

In combination with inheritance, case classes are used to mimic algebraic datatypes.

If an object performs stateful computations on the inside or exhibits other kinds of complex behaviour, it should be an ordinary class.

* case classes automatically generate equals and hashCode methods
* **case classes automatically create a companion object with the same name as the class**, which contains apply and unapply methods. The **apply** method enables constructing instances without prepending with new. The **unapply** extractor method enables the pattern matching.
* Also the compiler optimizes the speed of match-case pattern matching for case classes[

## Case class *copy* method
When you create a case class in Scala, a **copy** method is generated for your case class. 
It lets you make a copy of an object, where a **copy** is different than a clone, because with a copy you can change fields as desired during the copying process. The copy method is important in functional programming, where values (val) are immutable.
```scala
 val fred = Employee("Fred", "Anchorage", "Salesman")
 val joe = fred.copy(name="Joe") // joe: Employee = Employee(Joe,Anchorage,Salesman)
```

## Case class *apply* method
