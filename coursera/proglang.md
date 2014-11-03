# SML

## Expression and variable bindings
* An ML program is a sequence of bindings
* Each binding gets *type-checked* and then *evaluated*
* Type binding depends on a *static environment*, which is roughly the **types** of the preceding bindings in the file.
* How a binding is evalauated depends on a *dynamic environment*, which is roughly the **values** of the prededing binding s in the file.

## Function Bindings
* Function arguments are evaluated before being passed to functions
* ML is statically scoped
* Not all functions can be called recusively, only the call by names can
* Functions are first-class expressions
* A function type is **"argument types"->"result type"** where the argument types are separated by **'*'**
* The type of body **e** must have type **t**, i.e., the result type of **x0**.
```SML
(* A function named x0, and body e, and arguments x1...xn, which typed t1...tn*)
fun x0 (x1:t1,...,xn:tn) = e
```
### Tail Recursion and Accumulators
Using accumulatros you can make some functions tail recursive.
Tail recurision has a good feature like *when f makes a recursive all to f, there is nothing more for the caller to do after the callee returns except return the callee's result*. This situation is called a **tail call**. And some
programming languages like ML typically promise an essentail optimization: When a call is a a tail call, the caller's
stack-frame is popped before the call - the callee's statck-frame just replace the caller's. This make sense: the caller was just going to return the callee's result anyway.
By doing this optimization, resusion can sometimes(e.g., tail call) be as efficient as a while-loop, which also does not make the call-stack bigger.
```SML
fun sum1 xs=
  case xs of
    [] => 0
    i::xs' => i + sum xs'
(* using accumulator to make a tail recursive function *)
fun sum2 xs = 
  let fun f (acc,xs) =
    case xs of
      [] => acc
      x::xs' => f(acc+i,xs')
  in
    f(0,xs)
  end
```
### A Precise Definition of Tail Position
A call is a tail call if it is in tail position. Possible tail positions are:
* In ```fun f(x)=e```, e is in tail position
* If an expression is no tin tail position, the none of its subexpressions are in tail position
* If ```if e1 the e2 else e3``` is in tail position, then e2 and e3 are in tail position (but not e1), Case expressions are similar
* If ```let b1 ... bn in e end``` is in tail position, then e is in tail position (but no expressions in the bindings are)
* Function-call arguments are not in tail position.

## Type Inference
Type inference (Figure out types not written down) is a very cool feature of ML. You almost never have to write down type.

## Conceptual ways to build new Types
3 ways to create a compound type:
* "Each-of" : A compound type t describes values that contain each of value of type t1,t2,... and tn. e.g. Records, 
        tuples.
* "One-of": A compuond type t describes values that contain a value of one of the types t1,t2,... or tn. e.g. *int option* is a simple example: A value of this type either contains an int or it does not. Java enumerator.
* "Self-reference": A compund type t may refer to itself in its definition in order to describe recusive data structure  like list and tree.

### Datatype bindings
The following defines a new type of one-fo type. It defines the constructors (TwoInts, Str, Pizza) and constructor arguments (int*int, string and NONE)
```SML
datatype mytype = TwoInts of int*int
                | Str of string
                | Pizza
                
(* above define 3 constructor functions *)
TwoInts = fn int*int -> mytype
Str = fn string -> mytype
Pizza = val mytype
```

## Currying: (named after Haskell Curry who invent this)
If a function ```fn x*y => z``, then curreing is to have a function take the first comceptual argument ```x``` and return
another function that takes the second conceptual argument ```y``  and return ```z``` so on. The function with only partial conceptual arguments called *partial application*
```SML
val sorted = fn x=> fn y=> fn z=> z>y andalso y>=x
(((sort 4)5)6) = true
(* parenthis is optional *)
sort 4 5 6 = true

(* the following is called partial application *)
sort 4 
sort 4 5

(* alternative way to define currying, separating conceptual arguments by spaces rather than anonymous functions *)
fun sorted x y z = z>y and also y>=x

```

### Partial application vs val-binding
```SML
fun longest_string_helper (f:int*int->bool) (cs:string list) =
(*fun longest_string_helper f cs = *)
    List.foldl (fn (s,acc) => if f(String.size(s), String.size(acc)) then s else acc) "" cs

(* val-binding *)
val longest_string3 = fn cs => longest_string_helper (fn (x,y)=>x>y) cs

(* partial application *)
fun longest_string4 cs = longest_string_helper (fn (x,y)=>x>=y) cs
```

## Closure
A Function value has two pars, the *code* for the function and the *environment that was current when we created the 
function*. These two parts do really form a "pair". When evalates the code part, you will using the environment part.

This "pair" is called a *function closure• or just *closure*. The code itself can have *free variables* (variables that
are nto bound inside the code so they need to be bound by some outer environment), the closure carries with it an 
environment that provides all these bindings. So the closure overall is "closed" -- it has everything it needs to 
produce a function result given a function argument.

```SML
val x=1 
fun f y = x+y
val x=2
val y=3
(* val z = 6 *)
val z=f(x+y) 
```

### Lexical Scope
The body of a function is evaluated in the environment where the funciton is **defined**, not the environment where
the function is called.

### Combining functions
```SML
fun sqrt_of_abs i = Math.sqrt(Real.fromInt (abs i))

(* composition with operator "o" is associate-to-right *)
fun sqrt_of_abs i = (Math.sqrt o Real.fromInt o abs) i

(* pipleline oeprator *)
fun sqrt_of_abs i = i |> abs |> Real.fromInt |> Math.sqrt
```

### Using closure for callbacks
In UI libraries often there are listeners to different events, and the library has no idea about what the listener
want to do. It just call the listener, and when called, each listener can response by using their local variables,
which is perfect for closure.

### Abstract data types




## Returning functions
```SML

fun increment x = x+1
val increment = fn x=>x+1

(* following using an anonymous function y=>3*y *)
fun triple_n_times(n,x) = ntimes((fn y=>3*y), n, x))

fun double_or_triple f= 
if f7
then fn x=> 2*x
else fn x=> 3*x
```

## Mutation via ML References
In ML, most things really cannot be mutated. But Mutation is ok in some settings. A key approach in functional 
programming is to use it when "updating the state of something so all users of that state can see a change has
occured" is the natual way to model your computation.

One good way to think about a reference is as a record with one field where that field can be updated with the **:=**
operator.
```SML
val x=ref 0
val y = !x+1 (* y=1 *)
val _ = x := (!x) +2 (* the content of the reference x refers to is now 3 *)
val z = !x+1 (* z=4 *)
```
