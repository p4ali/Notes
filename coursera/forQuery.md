```scala
case class Book(title:String, authors:List[String])

val books=List{
  Book(title="Structure and Interpretation of COmputer Programs",
      authos=List("Abelson, Harald","Sussman, Gerald J.")),
  Book(title="Introduction ot Functional Programming",
      authors=List("Bird, Richard","Wadler, Phill")),
  Book(title="Effective Java",
      authors=List("Bloch, Joshua","Gafter, Neal")),
  Book(title="Programming in Scala",
      authors=List("Odersky, Martin","Spoon, Lex","Venners, Bill"))
}

for(b-books;a<-b.authors if a startsWith "Bird,") yield b.title
```
