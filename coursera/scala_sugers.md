## [Special symbols and meaning](https://docs.scala-lang.org/tutorials/FAQ/finding-symbols.html)

## Multi-line string
```scala
"""|foo
   |bar""".stripMargin // stripMargin == stripMargin(|) by default
foo
bar

"""#spam
   #eggs""".stripMargin('#')
spam
eggs

"""foo
   bar""".stripMargin
foo
  bar 
```

## [`apply` method](https://blog.matthewrathbone.com/2017/03/06/scala-object-apply-functions.html)
`apply` method allows you to create new instance without using `new`. 
Underhood, when we have a reference `f` to a function object, and write `f(args)` to apply arguments to the represented function, the compiler silently expands `f(args)` to the object method call `f.apply(args)`.
```scala
class Bar{
   def apply() = 0
}
val bar_1 = new Bar
bar_1() // res8: Int = 0

object BarMaker {
   def apply() = new Bar
}
val newBar = BarMaker() // newBar: Bar = Bar@4b78f82

// Function is an object with apply method
object Greet {
 def apply(name: String): String = {
   "Hello %s".format(name)
 }
}

// I can call apply explicitly if I want:
Greet.apply("bob")
// => "Hello bob"

// Or I can call Greet like it is a function:
Greet("bob")
// => "Hello bob"
```
