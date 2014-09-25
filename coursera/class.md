
```scala

// Type - class
class Rational(x: Int, y: Int) {
  def numer = x
  def denom = y
  
  def add(that: Rational, s: Rational): Rational =
    new Rational(numer*that.denom + that.numer*denom, that.denom*denom)
  
  override def toString() = numer + "/' + denom
  
}

// Value - object
val x = new Rational(1,2)
x.numer
x.demon

val y = new Rational(2,3)

x.add(y)
```
