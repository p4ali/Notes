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

## `apply` method
`apply` method allows you to create new instance without using `new`.
```scala
class Bar{
   def apply() = 0
}
val bar = new Bar
bar() // res8: Int = 0

object BarMaker {
   def apply() = new Bar
}
val newBar = BarMaker() // newBar: Bar = Bar@4b78f82
```
