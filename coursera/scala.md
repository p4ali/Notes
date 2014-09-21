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
