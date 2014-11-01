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
