# SML

ML does NOT have subtype polymorphism.

## Basic syntax
### let
```SML
let b1 b2 ... bn in e end
```

## Build-in compound data types
* Records: fixed-size, heterogeneous collections indexed by name
* Tuples: fixed-length, heterogeneous sequences indexed by position
* Lists(singly linked): variable-length, homogeneous sequences with O(1) time to prepend a value and O(n) time to access a value in the list
* Vectors: variable-length, homogeneous sequences with O(1) access time for elements(like arrays)
```SML
(* records *)
val foo={x=3,y=true}
#x(foo)

(* Tuples, i.e. a special form records where key=index *)
val x = (43, "hello")
val firxtX=#1(x)

(* list *)
val y=1::2::3::nil
val z=[1 2 3]
y==z
hd y = hd z
```

## Datatype Bindings
The following defines a datatype:
* A new type **mytype** that we can now use just like any other type
* 3 different *constructors*: **TwoINts, Str,and Pizza**.
```SML
(* define a new type where values have an int*int or a string or nothing(NONE) *)
datatype mytype = TwoInts of ini*int
                | Str of string
                | Pizza
```
A *constructor* is two different things: 
* either a function for creating values of the new type (*TwoInts* and *Str*)
* or a value of type mytype (e.g., *Pizza*) 
ML dos *NOT* provide access to datatype values, you can encouraged to access datatype values by using **Case Expressions**:
```SML
fun f x = (* f has type mytype -> int *)
  case x of
    Pizza => 3
  | TwoInts(i1,i2)=>i1+i2
  | Str s => String.size s
```
### Polymorphic Datatypes
This is the way to definie *generic* data structures.
```SML
(* this binding does NOT introcude a type option. Rather, it make it so that if 't' is a type, then 't option' is a type *)
datatype 'a option = NONE | SOME of 'a

(* this define polymophic datatypes taking multiple types, e.g., a binary tree where internal nodes hold values of type 'a and leaves hold values of type 'b, we then have type like (int,int) tree (in which every node and leaf holds an int) and (String,bool) tree (in which every node holds a string and every leaf holds bool) *)
datatype ('a,'b) tree = Node of 'a * ('a,'b) tree * ('a,'b) tree
                      | Leaf of 'b
```
### Conceptual ways to build new Types
3 ways to create a compound type:
* "Each-of" : A compound type t describes values that contain each of value of type t1,t2,... and tn. e.g. Records, 
        tuples.
* "One-of": A compuond type t describes values that contain a value of one of the types t1,t2,... or tn. e.g. *int option* is a simple example: A value of this type either contains an int or it does not. Java enumerator. (You can retrieve option value by **valOf**, though the better way is to use case expression)
* "Self-reference": A compund type t may refer to itself in its definition in order to describe recusive data structure  like list and tree.

## Exception binding and Handling
```SML
fun hd xs=
  case xs of
    [] => raise List.Empty (* List.Empty is a type of exception*)
  | x::_ => x 
```
You can create your own exception binding:
```SML
(* MyException has type int*int->exn *)
exception MyException of int*int (* You can call MyException(3,9) *)
```
The kinds exceptions are a lot like constructors of a datatype binding. Indeed, they are functions (if they carry values) or values (if they don't) that create values of type ```exn``` rather than the type of a datatype. So List.Empty, MyException(3,9) are all values of type ```exn```, whereas MyException has type of int***int->exn** 

### Raise and handling exception
```SML
exception Baz; (* no arguments *)
excpetion Foo of string; (* Foo("bar") *)
raise Foo("bar");

(* if raised exception is not handled, computation always stops *)
exception OutOfRange of int*int*string;
fun save_comb(n,m) =
  if n<=0 then raise OutOfRange(n,m,"")
  else if m<0 raise OutOfRange(n,m,"m must be greater than 0")
  else if m>n then raise OutOfRange(n,m,"n must be greater than m")
  else if m=0 orelse m=n then 1
  else safe_comb(n-1,m)+safe_comb(n-1,m-1)
fun comb(n,m) = safe_comb(n,m)
  handle
    OutOfRange(0,0,msg) => 1
  | OutOfRange(n,m,msg) =>
     ( print("Out of range: n="); print(n)
       print(" m="); print(m);
       print("\n"); print(msg); print("\n");
       0
     );
     
(* More examples *)
fun inverse x=1.0/x
  handle Div=>(print("divide by zero produces 'infinite' value: "); 
               real(expo(10,10)))
```

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
ML has no language constructs for creating mutable data. In order to mutate, you have to use **ref**.

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
## Type Inference
Type inference (Figure out types not written down) is a very cool feature of ML. You almost never have to write down type.
In Java:
```Java
int a=5
float b=1.0
String d="hello"
```
In ML:
```
a=5

(* alternatively, you can ascribe the type to the value like this *)
val a:int = 5;
val a = 5:int;
```

### statically typed language vs dynamically typed language
* *statically typed language* meaning every binding has a type that is determined "at compile-time", i.e., before any part of the program run. The type-checker is a compile-time procedure that either accepts or rejects a program. Such language like Java,C and ML.
* *dynamically typed language* meaning the type of a binding is NOT determinedd ahead of time and computations like bindng 42 to x and then treating x as a string WILL result in runtime ERRORs, language like Racket, Ruby, and Python.
* Unlike Java and C, ML is *implicitly typed*, meaning programmers rarely need to write down the type of binding. The type-checker mus to be more sophiscated to *infer* (i.e., figure out) what the *type annotations* "would have been" had programmers writen all of them.

## Modules and Signature
### Modules
Modules are used for namespace management for large program. It can be used to separate bindings into different *namespace*. 
In ML, we can use **structure** to define modules that contains a collection of findings.
```SML
(* bindings is any list of bindings, and Name is the name of your structure *)
structure Name = struct 
  bindings 
end
```
### Signature
signature defines the public types for modules. It prvovide strict interfaces that code outside the module must obey.
```SML
signature MATHLIB = 
sig
  val fact : int -> int
  val half_pi : real
  val doubler : int -> int
end

structure MyMathLib :> MATHLIB = 
struct
  fun fact x = if x=0 then 1 else x*fact(x-1)
  val half_pi = Math.pi/2.0
  fun doubler y = dbl y
  
  (* Hidden types (also abstract type, meaning private to structure only) *)
  fun dbl x=x*2
end
```
ML will type check signature when compiling MyMathLib (because of the ```:> MATHLIB```)

### Rules for signature matching
The following rule defines whether a sturcture *match* a signature. If a structure does not match a signature assigned to it, then module does not type-check. A tructure *Name* matches a signature *BLAH* if:
* for every val-binding in BLAH, Name must have a binding with that type or a more general type (e.g., the implementation can be polymorphic even if the signature says it is not). This binding could be provided via a val-binding, a fun-binding, or a datatype-binding.
* for every non-abstract-binding in BLAH, Name must have the same type binding
* for every abstract type-binding in BLAH, Name must have some binding that creates that type.
### abstract types
Inside structure, the definitions of type compoments may or may not be exported (defined in signature); type components whose definitions are hidden are *abstract types*.

### functor
A functor is a function from structures to structures; that is, a functor accepts one or more arguments, which are usually structures of a given signature, and produces a struacture (of the same signature) as its result. see also [Functor(函子), Applicative, and Monad(单子, or 一价原子)](http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html) and [category thoery(范畴论), morphism(态射), objects(物件)](http://zh.wikipedia.org/wiki/%E8%8C%83%E7%95%B4%E8%AE%BA)

### Monad(单子)
an abstract datay type in functional programming. It use to present computation not the data. It contains 2 opration **bind** (```>=``` in Haskell) and **return**, and a type constructor **M**. See also [Monads for functional programming](http://homepages.inf.ed.ac.uk/wadler/papers/marktoberdorf/baastad.pdf)

## Sugars
```SML
(* #? to reference tuple *)
fun sum_triple (triple : int * int * int) =
#1 triple + #2 triple + #3 triple

(* o or |> to compose function *)
val f= fn x=> x+1
val g=fn x=>x*2
val h= f o g (* f(g(x)) *)
val i= g |> f (* f(g(x)) *)

(* ^ for string concatenation *)
fun partial_name {first=x, middle=y, last=z} = x ^ " " ^ z

(* hd and tl to get head or tail of list *)
hd [1,2,3] = 1
tl [1,2,3] = [2,3]

(* valOf to get option value *)
int option = NONE | SOME of int
valOf (SOME 10) (* 10 *)

(* record *)
(* foo has filed x and y *)
val foo = {x=3, y=true}
(* access x of foo =3*)
#x(foo);

```
