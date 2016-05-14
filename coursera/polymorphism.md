## [Polymorphism](https://en.wikipedia.org/wiki/Polymorphism_%28computer_science%29#Parametric_polymorphism)
**Polymorphism**(many form) is the provision of a single interface to entities of different types.
A **polymorphic type** is one whose operations can also be applied to values of some other type, or types.

## Ad hoc polymorphism - function overloading
This refer to polymorphic functions that can be applied to arguments of different types, but behave differently
depending on the type of the arguments to which they are applied (also known as function overloading).

```java
class Op {
  String add(String x, String y){ return x+y; }
  int add(Integer x, Integer y) { return x+y; }
}
```

## Parametric polymorphism - gerneric/template
Parametric polymorphism allows a function or data type to be written gernerically, so that it can handle values
uniformly without depending on their type.

The concept of paramtic polymorphism applies to both data types and functions. The former is called *polymorphic data type*,
and the latter is called *polymorphic function*.

Parametic polymorphis is ubiquitous in functional programming.

``` scala
class Stack[T] {
  var elemes: List[T] = Nil
  def push(x:T) {elems = x::elems}
  def pop() {elems=elems.tail}
  def top: T = elmens.head
}

val stack = new Stack[Int]
stack.push(1)
```

## [Subtyping](https://en.wikipedia.org/wiki/Subtyping) - subclass
Subtying (also subtype polymorphism or inclusion polymorphism) is a form of **type polymorphism** in which a 
subtype is a datatype that is related to another datatype (the **supertype**) by some notion of substitutability,
meaning that program elements, typically subroutines or functions, written to operate on elements of the supertype
can also operate on elements of the subtype. 

If `S` is a subtype of `T`, then subtyping relation is often written `S <: T`, to mean that any term of type `S` can
be safely used in a context where a term of type `T` is expected. The precise semantics of subtyping crucially depends
on what *safely used in a context where* means in a given programming language. The type system of a programming language
essentially defines its own subtyping releation.

Due to subtyping relattion, a term may belong to more thatn one type. Subtying is therefore a form of type polymorphism.
In *object-oriented programming* the term **polymorphism** is commonly used to refer soley to this **subtype polymorphism**, 
while the techniques of **parametric polymorphism** would be considered **generic programming**.

```scala
abstract class Animal { def talk:String }
case class Cat(name: String) extends Animal { def talk = "meow" }
case class Dog(name: String) extends Animal { def talk = "wolf" }

def hear(animal: Animal) = animal.talk

hear(Cat("c")) // meow
hear(Dog("d")) // wolf
```
