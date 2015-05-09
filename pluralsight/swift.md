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

# keywords
|       |       |       |       |       |       |       |       |       |       |       |
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|
|class  |deinit |enum|extension |func  |import  |init   |let    |private|protocol|public|
|static |struct |subscript|typealias|var|break|case|continue|default    |do     |else   |
|fallthrough|for|if     |in     |return |switch |where  |while||||

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
// cannot change immutable property
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

// cannot change immutable
var mutable = ["one","two"]
mutable.append("three") // OK
let immutable = ["one","two"]
immutable.append("three") // Error: Immutable value of type "[String]" only has mutating members named 'append'
```

### Optionals
* Be something or be nothing.
* Part of type-safety
* complier will catch these
* unwrap to access with `!`
* use optional binding
* any type can be optional

```swift
var s1: String // a non-optional string
var s2: String? // an optional string: can be either a string or nil
var s3: String! // implicilty unwrapping optional. The bang ! is an force unwrapping operator

let fm = NSFileManager.defaultManager()
let path = "/Users/ali/.gitignore"

// let a = b also return a, so it can be used to check nil or not
if let gitignore = fm.contentsAtPath(path) {
  // TODO: parse contents
}

// Use the nil-coalescing operator to use an optional's value or an alternative
let gitignore = (path ?? "no gitignore)"

// force unwrapping
println("name is \(name!)") // if name is nil, will cause fatal error
println("job is \(job!)")
```

## REPL
To run REPL:
* Maverics `$ xcrun swift`
* Yosemite `$ swift`
