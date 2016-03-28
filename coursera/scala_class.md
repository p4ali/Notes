## [calling scala from java](http://lampwww.epfl.ch/~michelou/scala/using-scala-from-java.html)

## Everything is an object

## `class` and `object`
A `class` is a definition, a description. It defines a type in terms of methods and composition of other types.

An `object` is a singleton -- an instance of a class which is guaranteed to be unique. For every object in the code, an anonymous class is created, which inherits from whatever classes you declared object to implement. This class cannot be seen from Scala source code -- though you can get at it through reflection. 
```scala
object A extends B with C
```
This will declare an anonymous class which extends B with the trait C and create a single instance of this class **named A**.

On the other hand, Most singleton objects do not standalone, but instead are associated with a class of the same name. When
this happens, hte singleton object is called the *companion object* of the class, and the class is called the *companion class*
of the object.

### Constructor
In Scala the primary constructor is the class’ body and it’s parameter list comes right after the class name.
```scala
class Greeter(message: String) {
    println("A greeter is being instantiated")
    
    def SayHi() = println(message)
}

val greeter = new Greeter("Hello world!")
greeter.SayHi()

// output:
// A greeter is being instantiated 
// Hello world!
```


### Companion object
An object is said to be the **companion-object** of a class/trait if they share the same name in the same source file. 
A companion object difffers from other objects as it has access rights to the class/trait that other objects do not.
In particilar it can acess methods and fields that are private in the class/trait.

An analog to a companion object in Java is having a class with static methods. In scala, you would move the static methods
to a companion object. You can use companion object to define factory methods for class.

```scala
  // file Companion.scala
  1 class Companion{
  2   def hello() {println("Hello class")} // [1]
  3 }
  4
  5 object Companion{
  6   def hallo() {println("Hallo object")} // [2]
  7   def hello() {println("Hello object")} // [3]
  8 }
  9
  
  // file TestCompanion.java
  1 public class TestCompanion{
  2   public static void main(String[] args){
  3     new Companion().hello(); // [1]
  4     Companion.hallo(); // [2]
  5     Companion$.MODULE$.hello(); // [3] hidden static
  6   }
  7 }  
  
```

## [style guide](http://docs.scala-lang.org/style/files.html)
Basically, similar to Java .java file, Scala .scala file should be named as same as the logical Unit with CamelCase.
e.g., *Sets.scala*. And should be located in the package folder similar to Java.

A little exception for multi-unit file, which contain serveral logical units, which should be named CamelCase with
a lower-case first letter, e.g., *optionCollections.scala*.

## Automatically generate getter for constructor's parameters
Use **val** keyword in constructor to make paramter publicly accessible:
```scala
class Foo(val bar:String) {}

object Main{
  def main(args: Array[String]){
    println(new Foo("hello").bar)
  }
}
```
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

## methods without arguments

```scala
// file methodsNoArgs.scala
  1 class Complex(real: Double, imaginary: Double){
  2   def re=real
  3   def im=imaginary
  4 }
  5
  6 object Main {
  7   def main(args: Array[String]){
  8     val c = new Complex(1.2,3.4)
  9     println("imaginary part: "+c.im)
 10   }
 11 }
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
