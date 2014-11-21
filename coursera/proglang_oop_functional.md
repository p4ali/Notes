## OOP versus Fuctional Decomposition (punchline)
* FP(Functional Programming) approach
 * break programs down into functions that perform some operation
 * define each function to handle each case of data
* OOP(Object Oriented Programming) approach (opposite way to functional approach)
 * break programs down into classes that give ehavior to some kine of data
 * define each data as class to handle each function

|      | eval | toString | hasZero |
|------|------|----------|---------|
| Int  ||||
| Add  ||||
| Negate ||||

* FP(by columns) and OOP(by rows) often doing the same thing in exact oppoiste way - organize the program "by rows" or "by columns".
* Which is the "most natual" may depend on what you are doing (e.g., an interpreter (FP) vs a GUI (OOP)) or personal taste.
* Code layout is important, but there is no perfect way since software has many dimensions of structure
 * tools, IDEs can help with multiple "views" (e.g., rows/columns)
 * 

## Adding Operations or Variables
* FP
 * easier to add function, but not datatype, need to change all functions
* OOP
 * easier to add new datatype (class), hard to add new operation (need to change all class)
* if functions no change, only new datatype will be added, then use OOP approach
* if datatypes are fixed, only functions will be added, then use FP approach
* Plan ahead
 * functions can support new variants
  * use type constructor to make datatypes extensible and have operations take function arguments to give results for the extensions.
 * Objects an support new operations
  * Visitor pattern uses the double-dispatch pattern to allow new operations "on the side"

## Binary methods 
* with functional decomposition
 * Situation is more complicated if an operation is defined over muliple arguments that can have different variants
* with object oriented decomposition
 * double dispatch (NOT the hybrid approach using is_a? instanceof, etc..), better with visitor pattern

## MultiMethods
* Allow multiple methods with same name
* Indicate which ones take instances of which classes
* Use dynamic dispatch on arguments in addtion to reciever to pick which method is called
* Java does not support dynamic mutimethod, it support static overloading only:

## Multiple Inheritance
* tree, diag, diamond inherit patterns

## Mixin / Trait
A *mixin* is a collection of methods
* A class can include number of mixins
* include a mixin makes its methods part of the class
* we use mixin to define certain methods in terms of other methods already dfined in the class
```ruby
module Doubler # mixin
  def double
    self+self
  end
end
class Pt
  include Doubler
  def + other
    and = Pt.new
    ans.x = self.x + other.x
    ans.y = self.y + other.y
    ans
  end
end
class String
  include Doubler
end

p = Pt.new(2,3)
p.double # @x=4,@y=6

'hi'.double # hihi
```

### lookup rules
When looking for receiver `obj`'s method `m`
* look in `obj`'s class
* look mixin that class include
* look `obj`'s superclass
* look superclass's mixin, etc...
* methods is ordered in mixin and class, later will shadow earler.
* mix method can access instance variable, but bad style.
* BUT mixin should not be the full replacement of inheritance, i.e., in some cases, it does not make sense to define a mixin semancically. However we have no way to guarantee those methods were actually defined in the class.

#### Two most popular/useful mixins in Ruby
* Comparable Defines `<,>,==,!=,>=,<=` in terms of `<=>`
* Enumerable: Defines many iterators(e.g., `map, find`) interms of each

## Interfaces
* Interface is *statically-typed oop*. Make sure the methods exists and type-checks. 
* interface is type
 * only method signature, not body
 * interfaces have little use in a dynamically typed language
### subtype
* if c is a subclass of D, then c is subtype of D
* if c implement interface I, then c is subtype of I

## Abstract/virtual methods
Help a statically typed OOP language to suport "required overriding'?
* Ruby: by adding comment, which is *error-causing implementation*
* C++/Java: Use static checking to check by using abstract method in abstract class
 * abstract methods similar to caller passed high-order function as call argument
* any langugage have multiple inheritance and abstract methods, do not need interfaces
* Only expect interfaces if in statically typed OOP without multiple inheritance
 * Not Ruby (dynamically typed)
 * Not C++ ( support mutiple inheritance already)

## Subtyping
Make a record the subtype of a partial record. This can be done by adding following type check rules:
```
t1 <: t2 if t1 is a subtype of t2
if e has type t1 and t1 <: t2
then e (also) has type t2
```
### Subtype rules
* A supertype can have a subset of fields with the same types
* Permutation subtyping: A supertype can have the same set of fields withe the same types in a different order
* Transitivity: if t1<: t2, and t2 <: t3, then t1 <: t3
* Reflexivity: every types is a subtype of itself
* depth subtyping - replace field with subtype will not change the type (Won't work! break the soundness). This is similar to Java tmplate.
 * Subtyping cannot change the type of fields.
 * if fields are immutable, the depth subtyping is sound!
 * choose two of three: setters, depth subtyping, soundness
```
if ta <: tb, then {f1:t1, ..., f:ta, ... } <:
                  {f1:t1, ..., f:tb, ... }
# example
fun setToOrigin (c:{center:{x:real,y:real},r:real})= c.center = {x=0,y=0}
val spere:{center:{x:real,y:real,z:real}, r:real} = {center={x=3.0,y=4.0,z=5.0},r=1.0}
val _=setToOrigin(spere)
val _=spere.center.z (* kaboom! (no z field) *)
```

### Java/C Arrays
Arrays in Java: `if t1 <: t2, then t1[] <: t2[]`, and this code type-checks (BUT should not, JAVA!)
```Java
class Point {...}
class ColorPoint extends Point{..}
void m1(Point[] pt_attr) {
  pt_arr[0]=new Point(3,4);
}
// The following code should NOT pass type-checking, but it does, and throw runtime exception.
String m2(int x) {
  ColorPoint[] cpt_att=new ColorPoint[x];
  for(int i=0;j<x;i++)
     cpt_arr[i] = new ColorPoint(0,0,"green");
  m1(cpt_arr);
  return cpt_arr[0].color;// Exception!!!
}
```
