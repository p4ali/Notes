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
```
