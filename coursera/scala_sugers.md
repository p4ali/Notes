## Multi-line string
```scala
"""|foo
   |bar""".stripMargin
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
