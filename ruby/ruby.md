## `require`,`load`,`include`,`extend`, 
see [http://ionrails.com/2009/09/19/ruby_require-vs-load-vs-include-vs-extend/](http://ionrails.com/2009/09/19/ruby_require-vs-load-vs-include-vs-extend/)
### Require
The require method allows you to load a library and prevents it from being loaded more than once. 

The require method will return ‘false’ if you try to load the same library after the first time. 
The require method only needs to be used if library you are loading is defined in a separate file, 
which is usually the case.

So it keeps track of whether that library was already loaded or not. 
You also don’t need to specify the “.rb” extension of the library file name.

Here’s an example of how to use require. Place the require method at the very top of your “.rb” file:
```ruby
require 'test_library'
```

### Load
The load method is almost like the require method except it doesn’t keep track of whether or not 
that library has been loaded. So it’s possible to load a library multiple times and also when 
using the load method you must specify the “.rb” extension of the library file name.

Most of the time, you’ll want to use require instead of load but load is there if you want a library 
to be loaded each time load is called. For example, if your module changes its state frequently, 
you may want to use load to pick up those changes within classes loaded from.

Here’s an example of how to use load. Place the load method at the very top of your “.rb” file. 
Also the load method takes a path to the file as an argument:
```ruby
load 'test_library.rb'
```

So for example, if the module is defined in a separate .rb file than it’s used, then you can use the
```ruby
# File: log.rb
module Log 
  def class_type
    "This class is of type: #{self.class}"
  end
end
# File: test.rb
load 'log.rb'
 
class TestClass 
  include Log 
  # ... 
end
```

### Include
When you Include a module into your class as shown below, it’s as if you took the code defined within the module and inserted it within the class, where you ‘include’ it. 

It allows the ‘mixin’ behavior. It’s used to DRY up your code to avoid duplication, for instance, if there were multiple classes that would need the same code within the module.

The following assumes that the module Log and class TestClass are defined in the same .rb file. If they were in separate files, then `load` or `require` must be used to let the class know about the module you’ve defined.
```ruby
module Log 
  def class_type
    "This class is of type: #{self.class}"
  end
end
 
class TestClass 
  include Log 
  # ... 
end
 
tc = TestClass.new.class_type
puts tc # “This class is of type: TestClass”
```

### Extend
When using the extend method instead of include, you are adding the module’s methods as class methods instead of as instance methods.

Here is an example of how to use the extend method:
```ruby
module Log 
  def class_type
    "This class is of type: #{self.class}"
  end
end
 
class TestClass 
  extend Log 
  # ... 
end
 
tc = TestClass.class_type
puts tc # “This class is of type: TestClass”
```

When using extend instead of include within the class, if you try to instantiate TestClass and call method class_type on it, as you did in the Include example above, you’ll get a NoMethodError. So, again, the module’s methods become available as class methods.
