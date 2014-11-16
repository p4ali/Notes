## Ruby logistics
* http://www.ruby-lang.org
* http://ruby-doc.org
* Programming Ruby 1.9 The progmatic Programmers' Guide

## Ruby
* Puer object-oriented: all values are objects (even numbers)
* Class-based: every object has a class that determines behavior 
 * like java, unlike javascript
 * Mixins
* Dynamically typed
* Convenient reflection: Run-time inspection objects
* Very dynamicd: can change calsses during execution
* Blocks and libraries encourage lots of closure idioms
* Syntax, scopint rules, semantics of a "scription language"
 * Variables "sprint to life" on use
 * very flexible arrays
* String support and regular expressions support
* Popular for server-sice web app
* many ways to do same thing

|        | dynamically typed | statically typed|
|:-------|:-------------------|:-----------------|
| functional | Racket | SML|
|object-oriented (OOP)| Ruby |Java,etc|

## Class and Objects
* all values are reference to objects
* Object communicate each other via *method calls*, also know as *messages*
* each object has class
* evey object is an instance of class
* an object's class determine the object behavior
```ruby
# create object
ClassName.new
# method call
e.m
```
### Object state (attribute/field)
* all state are private
* only accessible from object's methods
* consistes of *instance variables*
 * using one not in state is not an error - will produce **nil** object
* **attr_accessor** or **attr_getter**
* starts with an **@**, e.g. *@foo*
```ruby
class A
  def m1
    @foo = 0 # create an instance variable foo
    self
  end
  def m2 x
    @foo+=x
  end
  def foo
    @foo
  end
end

puts A.new.m1.m2(3).foo # 3
```

### *initialize* method
The **initialize** method will be called after **ClassName.new** and before the object is returned.
```ruby
class A
  def initialize(f=0)
    @foo=f
  end
  def foo
    @foo
  end
end

puts A.new(3).foo # 3
```

## Variables
* lcoal variable, not declartion needed, mutable.

## self
* refer to "the current object", i.e., the object whose method is executing
* can call method on "same obejct" with `self.m(..)`
* can also pass/return/store "the whole object" with just **self**. good for chaininig method call.
```ruby
class C
  def m1
    prinf "hi"
    self
  end
  
  def m2
    print "bye"
    self
  end
end
c=C.new
c.m1.m2 # "hi bye"
```

## Style
* indentation does not affect semantics
* newline matter
* semicolon is encourage if more in same line
