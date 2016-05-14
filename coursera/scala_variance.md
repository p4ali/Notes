## Abstract Types
In scala, classes are parameterized with values (the constructor parameters) and with types(if class are generic).
So it is not only possible to have values as object members; types along with values are members of objects too.
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

## Structural type bound
You can define the bound type on the fly with `{...}`. The following defines a structrual bound on A - A can be
an instance of any class as long as it has a *close* method on it. `{...}` also called **structural typing**.
```scala
def using[A <: {def close(): Unit}, B](param: A)(f: A => B): B =  try {f(param)} finally {param.close()}
using(new BufferReader(otherReader)) { reader=> reader.readLine() } // return line
```

## [Variances](https://msdn.microsoft.com/en-us/library/dd799517(v=vs.110).aspx)
In the declareation `trait List[+A]`, the `+` in front of the type parameter A is a *variance annotation* that signals
that `A` is a covariant or *Positive* paramter of `List`. This means that, for instance, `List[Dog]` is considered a 
subtype of `List[Animal]`, assuming `Dog` is a subtype of `Animal`. 

More generally, for all types `X` and `Y`, if `X` is a subtype of `Y`, then `List[X]` is a subtype of `List[Y]`. We 
could leave out the `+` in front of the `A`, which would make `List` **invariant** in that type parameter.

Notice that `Nil` extends `List[Nothing]`. `Nothing` is a subtype of all types, which means that in conjunction with
the *variance annotation*, `Nil` can be considered a `List[Int]`, a `List[double]`, and so on.

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

## Convariance and Contravariance in Generics
**Converance** and **contravariance** are tems that refer to the ability to use less derived (less specific) or 
more derived (more specific) type than originally specified.

Generic type parameters support covaiance and controvariance to provide greater flexibility in assigning and using
generic types.

### Covariance 
Enable you to use a more derived type than the originally specified. e.g., you can assign an instance of `List[Dog]` (List of Dog)
to a variable of `List[+Animal]`(List of Animal).

### Contravariance
Enable you to use a more generic (less specific) type than original specified.  e.g., you can assign an instance of
`List[Animal]` to a variale of `List[-Dog]`.

### Invariance
Means that you can use only the type originally specified. So an invariant generic type parameter is neither covariant
not contravariant. You cannot assign an instance of `List[Dog]` to a variable of `List[Animal]` or vice versa.

In general, a covariant type parameter can be used as the return type of a delegate, and contravariant type parameters 
can be used as parameter types. For an interface, covariant type parameters can be used as the return types of the 
interface's methods, and contravariant type parameters can be used as the parameter types of the interface's methods.
