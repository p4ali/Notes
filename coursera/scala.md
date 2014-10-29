## Programming Paradigms

* Imperative programming - modify mutable variables using assiments and control structures.
 * theory does not define mutationss
  * one or more data types
  * operations on these types
  * laws that describe the relationship between values and operation

* Evaluation
 * substitution model
  * call-by-name and call-by-value
  * if CBV terminates, then CBN terminates, and same value. But the other direction is not true.
  * by default, scala use CBV, and if you want to CBN, put an '=>' in front of the type, e.g.
  ```scala
  def constOne(x: Int, y: => Int) = 1 // YES, there is space between : =>
  ```
* Conditionsals and Value Definitions
```scala
def and(x:Boolean, y: =>Boolean) = if (x) y else false
```

* Persistent data structure 
 * cornerstone of scaling functional prog up to collections. 
 * Older version data structure unchanged
* Dynamic dispatching - subtype
* Type erasure - template
* eta-expansion - replace function type with function value.
* Liskov substituion principle
```
Let q(x) be a property provable about objects x of type T. Then q(y) should be provable for objects y of type S where S is a subtype of T.
```

* Implicit Parameters
```scala
def msort[T](xs: List[T])(implicit ord: Ordering) = ...
  def merge(xs: List[T], ys:List[T]) = 
    ... if (ord.lt(x,y)) ...
    ... merge(msort(fst),msort(snd))

// The calls to msort can avoid the ordering parameters
   msort(nums); msort(fruits)
// The compiler will figure out the right implicity to pass based on the demanded type.
```
 * is marked **implicit**
 * has a type compatible with T
 * is visible at the point of the function call, or is defined in a companion object associated with T
 * if there is a single(most specific) definition, it will be taken, otherwise error
