## Iterating
```scala
val names = Array("Dan", "Travis", "Chris")
names.foreach { name =>
  println(name)
}

// or
names.foreach(println)

//
val nums = List(1,2,3,4,5)

var sum = 0
for (n <- nums){
  sum += n
}

```

## Folding
To combine or examine every element in a collection, producing a single value as a result. For example, summing a list. Using **foreach**, we had to make use a mutable variable, and produce the result as a side effect. Since pure functional language should avoid using mutable state, the solution is **"folding"**.

Folding a collection is literally the process of looking at each element in addition to a current accumulatro and returning some value.

```scala
val nums = List(1,2,3,4,5)
val sum = nums.foldLeft(0)(
 (total:Int,n:Int) => total + n 
)

// or
val nums - List(1,2,3,4,5)
val sum = numbers.reduceLeft[Int](_+_)
println("The sum of the numbers one through five if: "+sum)

// or 
(0/:nums){_+_}

```
The corresponding Java code is:
```Java
final int[] numbers = {1,2,3,4,5};
int sum = 0;
for (int num: numbers){
  sum+=num;
}
System.out.println("The sum of the numbers from one to five is "+sum);
```

## Reduce
Reduce similar to fold, however the difference is it does not require and initial value to "prime the sequence". Rather it starts with the very first element in the sequence and moves on the end.
```scala
// FolderLeft
val names = List("Dan", "Nick", "Greg")
val str1 = names.foldLeft("")((acc,n) => acc+","+n)
println(str1) //  str1  : String = ,Dan,Nick,Greg

// Reduce
val str2 = names.reduceLeft[String]((acc,n) => acc+","+n)
println(str2) // str2  : String = Dan,Nick,Greg
```

## Mapping
Folder apply computation to each element in a collection and get a single value result. Mapping will apply each element in a collection in-place (as it were), creating a new collection of the same type with the modified elements.

```scala
val strs = List("1","2","3","4","5")
val nums = strs.map(s => s.toInt)

nums == List(1,2,3,4,5) // true
// OR using _ to represent any argument
val nums2 = strs.map(_.toInt)
```

## Binding
This operation named **flatMap** because the operation may be viewed as a combination of *map* and *flatten* methods. Like map, **flatMap** walks through every element in a collection, and applies a given function which return a same type of enclosing collection with an optional different type parameter. For example, see below signature and example

```scala
// signature
class List[A]{
  def flatMap[B](f:(A)=>List[B]): List[B]
}

// example
val strs = List("1","two","3","four","five")
val nums = strs.flatMap( s=>
  try{
    List(s.toInt)
  } catch {
    case _ => Nil // Nil is the empty List
  }
)
nums == List(1,3) //true
```

## Slice
see [ArrayOps](http://www.scala-lang.org/api/current/index.html#scala.collection.mutable.ArrayOps), the signature and example
```scala
// signature
def slice(from: Int, until: Int): Array[T] // where from <= indexOf(x) < until

# example
val sub=Array("1","2","3","4","5","6").slice(1,4)
sub == Array("2","3","4")
```
