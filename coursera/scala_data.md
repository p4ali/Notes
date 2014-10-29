## Pair
```scala
val pair = ("answer", 42) // pair: (String,Int) =(answer,42)
val label=pair._1 // "answer"
val value = pair._2
```

## Tuple
```scala
val tuple = ("a","b","c")
```
### Matching
```scala
(x,y,z) match {
  case (x0::xs,y0::ys,z0::zs) => x0+y0+z0 + add(xs,ys,zs)
}
```


