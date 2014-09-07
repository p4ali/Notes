## Javascript does not have block scope
## Variable Scopes
Javascript does not have block scope. The work around is to create a new funciton and immediately invoking them.

### **var** vs **let**
**var** is scoped to the nearest function block (or global if outside a function block).
**let** is scoped to the nearest enclosing block (or global if outside any block), which can be smaller than
a function block.
```javascript
//Glocal 
let me = 'go'; //globally scoped
var i = 'able'; //globally scoped

// Function block - identical
function ingWithinEstablishedParameters() {
    let terOfRecommendation = 'awesome worker!'; //function block scoped
    var sityCheerleading = 'go!'; //function block scoped
};

// Block
// let only visible in the for() loop
function allyIlliterate() {
    //tuce is *not* visible out here

    for( let tuce = 0; tuce < 5; tuce++ ) {
        //tuce is only visible in here (and in the for() parentheses)
    };

    //tuce is *not* visible out here
};

// var is visible to the whole function
function byE40() {
    //nish *is* visible out here

    for( var nish = 0; nish < 5; nish++ ) {
        //nish is visible to the whole function
    };

    //nish *is* visible out here
};

// let with its own closing block
function conjunctionJunctionWhatsYour() {
    //sNotGetCrazy is *not* visible out here

    let( sNotGetCrazy = 'now' ) {
        //sNotGetCrazy is only visible in here
    };

    //sNotGetCrazy is *not* visible out here
};
```


### Global vs Local Variables
```javascript
// local variable start with var
var myLocal = "this is my local variable, and it start with var";
myGlobal = "This is a global variable, which start with nothing";


```

### Local variable will override globla variable anyway
```javascript
var foo = "Hello!";
 
function doSomethingClever() {
    alert(foo); // undefined
 
    var foo = "Good Bye!";
    alert(foo); // Good Bye!
}
 
doSomethingClever();
```
The output of above code is *undefined* and *Good Bye!*. This is because When JavaScript encounters a function, 
one of the first things it does is scan the full body of the code for any declared variables. When it encounters 
them, it initializes them by default with undefined. Because the doSomethingClever function is declaring a 
variable called **foo**, before the first alert even hits, an entry for **foo** is created with a value of *undefined*. 
Eventually, when the code hits ```var foo = "Good Bye!"```, the value of **foo** is properly initialized. 
That doesn't help our first alert function, but that does help the second one that follows the re-declaration of foo.

Keep this little quirk in mind if you ever run into a situation where you are re-declaring variables into a 
local scope like this simple example highlighted.

## Function Pullup (Declaration form vs Expression form)
Function declarations are hoisted to the top of the context (either
the function in which the declaration occurs or the global scope) when the
code is executed. That means you can actually define a function after it is
used in code without generating an error.
