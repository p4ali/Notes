## Overview
* List is indexed values, it's different from Array
 * List is **immutable**, Array is mutable by set element
 * List is linked, but Array is flat
 * use `:::` to concat lists, and `::` to prepend element to list
* Set is container for different elements
 * 2 types: mutable or immutable, 
 * use `++` to concatenate two sets
 * use `+`, `-` to add or remote an elemen to set (create a new set)
 * ordered by adding order
* Map are key values
 * entry is `("key1" -> "value1")`
 * use `++` to concatenate two maps
* Tuple can be a group of different type of objects
 * unlike array, or set
 * syntactic sugar: `t=(1,2,3,4)`
 * use `t._1` to access tutple element
* Option is a container of zero or one element
 * two values: `Some[T]` or `None` 
 * to construct `var a:Option[Int] = Some(2)`
 * use `getOrElse` to tetrieve with default value
 * use `isEmpty` to check None or not

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
Folder apply computation to each element in a collection and get a single value result. Mapping builds a new collection by applying a function to all elements of this collection.

```scala
val strs = List("1","2","3","4","5")
val nums = strs.map(s => s.toInt)

nums == List(1,2,3,4,5) // true
// OR using _ to represent any argument
val nums2 = strs.map(_.toInt)

val m=Map('a'->1,'b'->2)
m.map((kv) => kv._2)
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

## Collection Recap

### Hierarchy
* Iterable
 * Seq
  * IndexedSeq
   * Vector
   * ... Array(Java)
   * ... String(Java)
  * LinearSeq
   * List
 * Set
 * Map

### Collection Methods
All collection types share a common set of general methods:
* map
* flatmap
* filter
* foldLeft
* foldRight

```scala
// impl on lists
abstract class List[+T]{
  def map[U](f: T=>U): List[U] = this match{
    case x::xs => f(x) :: xs.map(f)
    case Nil => Nil
  }

  def flatmap[U](f: T=>List[U]): List[U] = this match{
    case x::xs => f(x) ++ xs.flatMap(f)
    case Nil => Nil
  }

}

```

