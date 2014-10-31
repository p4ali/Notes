## For-Expressions and Higher-Order Functions
The syntax of **for** is closely related to the higher-order functions **map,flatMap** and **filter**.
### define map, flatMap and filter with for
```scala
def mapFun[T,U](xs:List[T],f:T=>U):List[U]= for(x<-xs) yield f(x)
def flatMap[T,U](xs:List[T],f:T=>Iterable[U]):List[U]= for(x<-xs;y<-f(x)) yield y
def filter[T](xs:List[T],p:T=>Boolean):List[T]= for(x<-xs if p(x)) yield x
```
### translate for with map, flatmap and filter
```scala
for(x<-e1) yield e2
e1.map(x=>e2)

for(x<-e1 if f;s) yield e2
for(x<-e1.withFilter(x=>f);s) yield e2

for(x<-e1;y<-e2;s) yields e3
e1.flatMap(x=>for(y<-e2;s) yield e3)
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
