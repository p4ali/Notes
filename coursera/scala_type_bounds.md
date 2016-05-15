When defining a parameterized type or method, it may be necessary to specify bounds
on the type parameter. For example, a container might assume that certain methods
exist on all types used for the type parameter.

## **Note**:
Type bounds and variance annotations cover unrelated issues:
* A type bound specifies constraints on allowed types that can be used for a
  type parameter in a parameterized type. For example T <: AnyRef
  limits T to be a subtype of AnyRef. 
* A variance annotation specifies when an instance of a subtype of a parameterized 
  type can be substituted where a supertype instance is expected. For example, because
  List[+T] is covariant in T, List[String] is a subtype of List[Any].

## Upper bound `A <: B`
`<:` means any type of A that is a subtype(or same) of B.

## Lower bound `A >: B`
`>:` means any type of A that is a supertype(or same) of B

