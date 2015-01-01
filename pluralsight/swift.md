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
* `let` define immutable constant
* `var` define mutable variable
```swift
let immutable = ["one","two","three"]
var mutable = ["one","two","three"]

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
```
