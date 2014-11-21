## OOP versus Fuctional Decomposition (punchline)
* FP(Functional Programming) approach
 * break programs down into functions that perform some operation
 * define each function to handle each case of data
* OOP(Object Oriented Programming) approach (opposite way to functional approach)
 * break programs down into classes that give ehavior to some kine of data
 * define each data as class to handle each function

|      | eval | toString | hasZero |
|------|------|----------|---------|
| Int  ||||
| Add  ||||
| Negate ||||

* FP(by columns) and OOP(by rows) often doing the same thing in exact oppoiste way - organize the program "by rows" or "by columns".
* Which is the "most natual" may depend on what you are doing (e.g., an interpreter (FP) vs a GUI (OOP)) or personal taste.
* Code layout is important, but there is no perfect way since software has many dimensions of structure
 * tools, IDEs can help with multiple "views" (e.g., rows/columns)
 * 

## Adding Operations or Variables
* FP
 * easier to add function, but not datatype, need to change all functions
* OOP
 * easier to add new datatype (class), hard to add new operation (need to change all class)
* if functions no change, only new datatype will be added, then use OOP approach
* if datatypes are fixed, only functions will be added, then use FP approach
* Plan ahead
 * functions can support new variants
  * use type constructor to make datatypes extensible and have operations take function arguments to give results for the extensions.
 * Objects an support new operations
  * Visitor pattern uses the double-dispatch pattern to allow new operations "on the side"

## Binary methods 
* with functional decomposition
 * Situation is more complicated if an operation is defined over muliple arguments that can have different variants
* with object oriented decomposition
 * double dispatch (NOT the hybrid approach using is_a? instanceof, etc..), better with visitor pattern

