## Abstract Types
In scala, classes are parameterized with values (the constructor parameters) and with types(if class are generic).
So it is not only possible to have values as object members; types alonw with values are members of objects too.
Furthermore, both forms of memebers can be concreate and abstract. The following example defines both a deferred
value definition and an **abstract type** definition as members of class Buffer:

```scala
trait Buffer {
  type T   // absract type 
  val element:T // value definition
}
```
**Abstract types** are types whose identity is not precisely know. In above example, we only know that each object of
class Buffer has a type member `T`, but the definion of class Buffer does not reveal to what concrete type the member
type `T` corresponds. You need to override type definition in subclasses.
```scala
class IntBuffer extends Buffer {
  type T = Int
}
```

## Polymorphic methods
Methods in scala can be parameterized with both values and types. Value parameters are enclosed in a pair of
parentheses **(x:T, n:Int)**, type parameters are enclosed in a pair of brackets, **[T,B]**.

```scala
def dup[T](x:T,n:Int): List[T] = if (n==0) Nil else x::dup(x,n-1)

dup[Int](3,4) // 3333
dup("Thred",2) // ThreeThree
```

## Upper Type Bounds
**Upper type bounds** `T <: A` declares that type variable `T` refers to a subtype of type `A`.

## Lower Type Bounds
**Lower type bounds** declare a type to be a supertype of another type. The term `T >: A` express that the type
pareameter `T` refer to a supertupe of type `A`.

```scala
case class ListNode[+T](h:T, t:ListNode[T]) {
  def head: T = h
  def tail: ListNode[T] = t
  def prepend[U >: T](elem: U): ListNode[U] = ListNode(elem, this)
}
```

## Variances
Variance annotaions can be added to type parameters of generic classes.
```scala
class Stack[+A]{
  def push[B >:A](elem:B):Stack[B] = new Stack[B]{...}
  ...
}
```
The annotaion `+T` declares type `T` to be used only in covariant positions. Similarly, `-T` would declare T to 
be used on in contravariant positions. For convariant type parameters we get a convariant subtype relationship
regarding ths type parameter. For example, `Stack[T]` is a subtype of `Stack[S]` if T is a subtype of S.

Remember, *Function subtyping contravariant in arguments and convariant in results*. So we have to use supertype `>:`
to make the class Stack coveriant.

