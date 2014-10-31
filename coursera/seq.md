## Seq
* List
* Vector
* String (from Java, not really subclass of Seq)
* Array (from Java, not really subclass of Seq)

### Sequence Operations
```scala
xs exists p
xs forall p
xs zip ys  // List(1,2,3).zip("Hello") =: List((1,"H"),(2,"e"),(3,"l"))
xs unzip
xs.flatMap f //"Hello".flatMap(s=>List('.',s)) =: String = .H.e.l.l.o
xs.sum
xs.product
xs.max
xs.min
xs.permutations.toList
xs.combinations.toList

// Combinations
(1 to 3) flatMap (x=>(1 to 2) map (y=>(x,y)))
// scalar product
(List(1,2,3) zip List(10,20,30)).map(xy=>xy._1*xy._2).sum // 140
(List(1,2,3) zip List(10,20,30)).map{case (x,y) => x*y}.sum //140
```
