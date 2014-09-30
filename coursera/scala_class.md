
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

## Orgnization - package
```scala
import week3.Rational // named imports - import only Rational
import week3._ // wildcard imports - import everything from package week3
improt week3.{Rational, Hello} // import both Rational and Hello

// All members of package scala, java.lang, and scala.Predef are automatically imported

// www.scala-lang.org/api/current
```

