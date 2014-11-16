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
* Unlike OOP language that make "what fields an object has" a fixed part of the class definition, in Ruby, different instances of same class can have different instance variables.
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
* **attr_accessor** or **attr_reader**
```ruby
def foo
  @foo
end

def foo= x
  @foo = x
end
e.foo = 42 # yes , whitepace works!

attr_reader :foo, :bar,... # define foo only
attr_accessor :foo, :bar... # define foo, and foo=
```
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
* The **initialize** method will be called on **ClassName.new** and before **new** returned.
* The arguments passed to **new** will be passed to **intialize**
* usually good style to create instance variables in **intialize**
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
* instance variables e.g. @foo
* class variables e.g. @@foo. Can be accessed from both instance method or class methods.
* class constances e.g. Foo. Visible outside class C::Foo
 
## Methods
* Calss methods (Java/C# static methods)
```
class C
  def self.method_name (args) # self. define a class method
   ...
  end
end

C.method_name(args) # use class name to call class method
```
* Visibiliy
 * private: only available to object itself. You can only call m(args), bu NOT call via self.m, 
 * protected: available only to code in the class or subclass
 * public: availble to all code, *default*
```ruby
class Foo
# by default methods public
  ...
protected
# now methods will be protected until next visibility keyworkd
  ...
public
  ...
private
  ...
end
```

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
