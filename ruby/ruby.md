## `require`,`load`,`include`,`extend`
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
