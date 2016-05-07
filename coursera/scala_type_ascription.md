## Type ascription

Type ascription is just telling the compiler what type you expect out of an expression, from all possible valid types.

A type is valid if it respects existing constraints, such as variance and type declarations, and it is either one of
the types the expression it applies to **"is a"**, or there is a conversion(with **implicit**) that applies in scope.

## Syntax `foo:T`

```
Expr1 ::= ...
        | PostfixExpr Ascription

Ascription ::= ‘:’ InfixType
             | ‘:’ Annotation {Annotation}
             | ‘:’ ‘_’ ‘*’
```
Type ascription can be done by annotate the variable by **:type**. e.g., `"hi":Int` will ascribe a string as Int.

```scala
val s="dog"
val o=s:Object

o.length // error: value length is not a member of java.lang.Object
o.getClass // java.lang.String
s.getClass // java.lang.string
o.asInstanceof[String].length // 3
```

## `foo.asInstanceOf[T]` vs `foo:T`

* foo.asInstanceOf[T] is a **type cast**, which is primarily a runtime operation. It let the
  compiler believe that foo is a type of T. This may result an error ( a ClassCastException ) at runtime.
* foo:T is a **type ascription**, which is entirely a compile-time operation. This help the 
  compiler to understand the meaning of your code, but withouth forcing it to believe anything
  that could possibly be untrue. No runtime failuers can result from the use of type ascription.

```scala
def foo(x:Any) { println("Any") }
def foo(x:String) { println("String") }

def main(args:Array[String]) {
  val a: Any = new Object
  val s = "string"
  
  foo(a)                       // Any
  foo(s)                       // String
  foo(s: Any)                  // Any
  foo(a.asInstanceOf[String])  // compiles, but ClassCastException at runtime
  foo(a:String)                // does not compile, type mismatch
}
```  
  
## Usage of type ascription

### Implicit conversion
type ascription can also be used to trigger implicit conversions. for example, in the following code
ascribing type Int to a String would normally be a compile-time type error, but before giving up
he compiler will search for available implicit conversions to make the problem go away. The implicit 
conversion that will be used in a given context is known at compile time.
  
```scala
  implicit def foo(s:String) = s.length
  
  val a = "hi":Int # a:Int = 2
```

### annotation for compiler 

For example, the following does not compile:
```scala
def prefixesOf(s: String) = s.foldLeft(Nil) { 
  case (head :: tail, char) => (head + char) :: head :: tail
  case (lst, char) => char.toString :: lst
}
```
But this does:
```scala
def prefixesOf(s: String) = s.foldLeft(Nil: List[String]) { 
  case (head :: tail, char) => (head + char) :: head :: tail
  case (lst, char) => char.toString :: lst
}
```

## Prefer pattern `match` to `asInstanceof`

If you find yourself using asInstanceOf, you should probably be using **match** instead.

```scala
// This is NOT good
if(x.isInstaneOf[String]) {
  val s = x.asInstanceOf[String]
  s.length
}else ...

// This is Preferred
def checkFoo(x:Any) = x match {
  case s:String => s.length
  case m:Int => m
  case _ => 0
}
```


