## L-value vs R-value
L-value has storage address that are programmatically accesible(I C, it means something can be assighned to, e.g, a variable). 
R-value can be L-value or non-L-value(e.g., 4+9).

## Operator Associativity
Left-associative means the operations are grouped from the left (e.g., +-*/). Right-associative means the operations
are grouped from the right(e.g. = in a=b=c, which eq a=(b=c) in C/Java). 

## Infix Operations

The associativity of an operator is determined by the operator’s last character. Operators ending in a colon ‘:’ 
are right-associative. All other operators are left-associative. For example, `++:` is right-associative, which 
means the instance of which the operator belongs to should be on the right of the operator, and the operand should
be on the left of the operator.

```scala
class Foo() {
  def ++:(n:Int) = println(2*n)
  val foo=new Foo()
  123 ++: foo // 246
}
```

## Type can also be infixed
```scala
// Note Pair has been deprecated, here just for demo
val t1:String Pair Int = ("abc",123) // t1: Pair[String,Int] = (abc,123)
```

## Reference
* [scala forum](http://osdir.com/ml/lang.scala/2007-04/msg00222.html)
