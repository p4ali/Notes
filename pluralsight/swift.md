## Swift
### What Swift is
* The future of Apple development
* modern language
* strongly-typed
* terse
* square-bracket free (syntax)
* LLVM-compiled & optimized
* Swift can call objectC and vice versa

### What Swift isn't
* C-based
* A scripting language
* dynamically-typed
* message-based
* on other platform

### Cornerstones of swift's philosophy
* easy to learn
* modern
* productive
* safe

## Data Types in Swift

### Noun of the language
* classes
* protocols
* extensions
* structs
* tuples
* funtions
* closures
* enumerations

### Mutablity in Swift
* `let immutable = ["one","two"]` define immutable constant
* `var mutable = ["one","two"]` define mutable variable
```swift
class Person {
  var firstName: String
  let lastName: String
  
  init(firstName: String, lastName: String) {
    self.firstName = firstName
    self.lastName = lastName
  }
}
let me = Person(firstName: "Alex", lastName: "Li")
me.firstName = "chengdong" // ok
me.lastName = "other" // error: Cannot assign to 'lastName' in 'me'


var mutable = ["one","two"]
mutable.append("three")

let immutable = ["one","two"]
immutable.append("three") // Error: Immutable value of type "[String]" only has mutating members named 'append'

```
