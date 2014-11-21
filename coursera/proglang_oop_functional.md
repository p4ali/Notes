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
