## For-Expressions and Higher-Order Functions
The syntax of **for** is closely related to the higher-order functions **map,flatMap** and **filter**.
### define map, flatMap and filter with for
```scala
def map[]
```

```scala
val books=List(
  Book(title="Structure and Interpretation of COmputer Programs",
      authors=List("Abelson, Harald","Sussman, Gerald J.")),
  Book(title="Introduction ot Functional Programming",
      authors=List("Bird, Richard","Wadler, Phill")),
  Book(title="Effective Java",
      authors=List("Bloch, Joshua")),
  Book(title="Java Pazzlers",
  		authors=List("Bloch, Joshua","Gafter, Neal")),
  Book(title="Programming in Scala",
      authors=List("Odersky, Martin","Spoon, Lex","Venners, Bill")))

for(b<-books;a<-b.authors if a startsWith "Bird,") yield b.title

for(b1<-books;b2<-books;b1!=b2;a1<-b1.authors;a2<-b2.authors;if a1==a2) yield a1

(for(b1<-books;b2<-books;if b1.title<b2.title;a1<-b1.authors;a2<-b2.authors;if a1==a2) yield a1).distinct
```
