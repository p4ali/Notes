## Pair
```scala
val pair = ("answer", 42) // pair: (String,Int) =(answer,42)
val label=pair._1 // "answer"
val value = pair._2
```

## Tuple
```scala
val tuple = ("a","b","c")
```
### Matching
```scala
(x,y,z) match {
  case (x0::xs,y0::ys,z0::zs) => x0+y0+z0 + add(xs,ys,zs)
}
```

`@` is used to assign the similarity to an variavle.

```scala
case class Address(street: String, city: String, country: String)
case class Person(name: String, age: Int, address: Address)
val alice = Person("Alice", 25, Address("1 Scala Lane", "Chicago", "USA"))
val bob = Person("Bob", 29, Address("2 Java Ave.", "Miami", "USA"))
val charlie = Person("Charlie", 32, Address("3 Python Ct.", "Boston", "USA"))
for (person <- Seq(alice, bob, charlie)) {
  person match {
    case p @ Person("Alice", 25, address) => println(s"Hi Alice! $p")
    case p @ Person("Bob", 29, a @ Address(street, city, country)) =>
    println(s"Hi ${p.name}! age ${p.age}, in ${a.city}")
    case p @ Person(name, age, _) =>
    println(s"Who are you, $age year-old person named $name? $p")
  }
}
```
The p @ … syntax assigns to p the whole Person instance and similarly for a @ … and
an Address



