## Summary

You can figure out what object *this* refers to by following a few simple rules:

* By default, *this* refers to the global object.
* When a function is called as a property on a parent object, *this* refers to the parent object inside that function.
* When a function is called with the new operator, *this* refers to the newly created object inside that function.
* When a function is called using call or apply, *this* refers to the first argument passed to call or apply. 
  If the first argument is null or not an object, *this* refers to the global object.

If you understand and follow those four rules, you will always know what this is.

## Reference
[Understanding javascript this](http://unschooled.org/2012/03/understanding-javascript-this/)
