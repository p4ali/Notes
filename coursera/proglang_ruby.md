## Ruby logistics
* http://www.ruby-lang.org
* http://ruby-doc.org
* Programming Ruby 1.9 The progmatic Programmers' Guide

## Ruby
* Puer object-oriented: everything is an object (even numbers)
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
* all values are reference to objects, even *nil*
* Evaluationg an expression in Ruby is an object
* all code is methods. Top-level methods are added to Object class, so will be available to any sub class.
* Object communicate each other via *method calls*, also know as *messages*
* each object has class
* evey object is an instance of class
* an object's class determine the object behavior
* Unlike OOP language that make "what fields an object has" a fixed part of the class definition, in Ruby, different instances of same class can have different instance variables.
 * In Ruby, class definition are dynamic, i.e., Ruby programs can add/change/replace methods while a program is running 
```ruby
# create object
ClassName.new
# method call
e.m

# change class definition dynamically
class FixNum
def double
 self+self
end
end
3.couble # 6

# add m method to Object class
def m
42
end
3.m # 42
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
* Ruby interpreter call **missing_method** when method is missing, and the default impl will print undefined message.
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
## Reflection
```ruby
#run irb
3.methods
(3.class).class
3.methods - nil.methods
```

## duck typing
"If it walks like a duck and quacks like a duck, it's a duck".
* pros: resuse code
* cons: almost nothing is equivalent

## Array
```ruby
a=[1,2,3,4]
a[1] # 2
a[1]=1
a[4] #nil
a.size # 4
a[-1] # 4
a[-4] # 1
a[6] = 14 # [1,2,3,4,nil,6]
a[5]="hi" # [1,2,3,4,"hi",6]
b=a+[tru,false] # [1,2,3,4,"hi",6,true,false]
c=[3,2,3] | [1,2] # [1,2,3]
tuple=[false,"hi", 1 2]

x=2
y = Array.new(x) # [nil,nil]
z = Array.new(x) {0} # [0,0]
w = Array.new(x+1){|i| -i} # [0,-1,-2]

# Array can be a stack or queue
a.push 7 # append to right-most
a.pop # remove the right-most
a.shift # remove the left-most
a.unshift 9 # prepend 

# slice
f=[2,3,4,5,6,7]
f[2,4] # get 4 element start from offset 2: [4,5,6,7]
f[2,4] = [1j,1] # replace element 2,3,4,5 with [1,1] => [2,3,1,1]

[1,2,3,4].each {|i| puts (i*i)} # 1,4,9,16
```

## Style
* indentation does not affect semantics
* newline matter
* semicolon is encourage if more in same line

## Sugars
### expression interpolation (Racket's quasiquote or eval_exp)
```ruby
"#{@num}#{if @den==1 then "" else "/"+@den.to_s end}"
```
### *nil* and *false* are false
```ruby
y=if 4> 3 then nil else 32 end
if y thne puts "A" else puts "B" end # B
```


