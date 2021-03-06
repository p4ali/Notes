# Iterable
## Seq
* List
* Vector
* String (from Java, not really subclass of Seq)
* Array (from Java, not really subclass of Seq)

### Sequence Operations
```scala
xs exists p
xs forall p // def isPrime(n:Int)=(2 until n) forall (d=>n%d!=0)
xs zip ys  // List(1,2,3).zip("Hello") =: List((1,"H"),(2,"e"),(3,"l"))
xs unzip
xs.flatMap f //"Hello".flatMap(s=>List('.',s)) =: String = .H.e.l.l.o
xs.sum
xs.product
xs.max
xs.min
xs.permutations.toList
xs.combinations.toList

val v=(4 to 0 by -1)
val lines=for(i<- v.reverse) yield Vector.fill(v.length)("* ").updated(i,"X ").mkString
"\n"+lines.mkString("\n")

// Combinations
(1 to 3) flatMap (x=>(1 to 2) map (y=>(x,y)))
// scalar product
(List(1,2,3) zip List(10,20,30)).map(xy=>xy._1*xy._2).sum // 140
(List(1,2,3) zip List(10,20,30)).map{case (x,y) => x*y}.sum //140
```
## Set
* unique elements
* ```contains``` vs inde in Seq
```scala
val s = (1 to 6).toSet
val fruit = Set("apple","Banana","Pear")
s map (_+2)
fruit.filter(_.startsWith == "app")
s.nonEmpty
```

## Map
