## Important Skill
* looking up library functions even in languages new to you is an important skill

## Dr Racket
``` racket
#lang racket ; tell interpreter this is racket code

(provide (all-defined-out)) ; make top-level definitions defined in this file (also module) externally visible.
(define s "Hello")
...
```

## Racket syntax
Racket has amazing simple syntax
* A **term** (anything in the language) is either
 * An **atom**, e.g., #t,#f,34 "hi", null, 4.0 x,...
 * A **special form**, e.g., *define*, *lambda*, *if*
  * Macros will let us define our own
 * A **sequence* of **terms** in parens: `(t1 t2 ... tn)`
  * if `t1` a *special form*, sematics of sequence is special
  * Else a *function call*

## Parentheses
Parentheses `(` and `[` are equal in Racket, and they make parsing the program text to semantics tree extremely easy.
Don's complain about parentheses, they behave like exactly as the tags in html.
See also [http://xkcd.com/297](http://xkcd.com/297)
* in most places `(e)` means call `e` with zero arguments
* so `((e))` means call `e` with zero arguemtns and call the result with zero arguments

## Comments
```racket
; single line comment
#; single block comment
#|
multiple blocks/lines comment
bla
bla
|#
```
## Definitions
```racket
(define a 3)
(define b (+ a 2))
```
## Symbol
A *symbol* is an atomic value that prints like an identifier proceded with `'`. Symbol `eq?` is much faster than string comparision.
```racket
'a ; a is a symbol
(eq? 'a 'a) ;#t
(eq? 'a (string->symbol "a"); #t
(string-> "one two"); '|one |
```
## boolean
Anything other than *#f* is *#t*.
```racket
#t
#f
(if null 14 15); 14
(if #f 14 15); 15
```

## if block
```racket
if e1 e2 e3 ; check e1, if #t e2, if #f e3, not else then needed
```

## cond
`cond` can be used to avoid nested if-expressions, it's one of SUGAR for other
Think it as a *switch* in Java
```racket
;syntax
(cond [e1a e1b]
      [e2a e2b]
      ...
      [eNa eNb]) ; eNa should be #t for Good style

;; example
(define (sum2 xs)
  (cond [(null? xs) 0]
        [(number? (car xs)) (+ (car xs) (sum2 (cdr xs)))]
        [(list? (car xs)) (+ (sum2 (car xs)) (sum2 (cdr xs)))]
        [#t (sum2 (cdr xs))]))
```

## function
**lambda** (like fun in scala, or fn in ML) is used to create an function. e.g., `(lambda (x) e)`
```racket
(define add (lambda (x y) (+ x y)))
(difine cube (lambda (x) (* x x x)))
(define (cube x) (* x x x))
(define (pow x y)
  (if (= y 0)
      1
      (* x (pow x (-y 1)))))

;; + is a function also
(+ 1 2)
(* 1 2 3 4 5)

;; you can also define function withouth lambda
(define (add x y) (+ x y))
```
### currying
```racket
(define pow
  (lambda (x)
    (lambda (y)
      if (= y 0)
         1
         (*x ((pow x) (-y 1)))
    )
  )
)
(define three-to-the (pow 3))
(define eightyone (three-to-the 4))
(define sisteen ((pow 2) 4))
```

## List
```racket
null; empty list. 
'();  empty list
cons; constructor
(list e1 ... en); constructo a list
car; access head of list
cdr; access tail of list
null?; check for empty
```

## sequence
Like ML, **list** in racket is readonly.
```racket
empty ; '()
'(1 2 3)
'(t1 t2 ... tn)
(cons 1 (cons 2 (cons 3 null))) ; (1 2 3)
(cons 1 (cons 2 (cons 3 4))) ; (1 2 3 . 4) where '.' means not a list, or imiproper list.
pair? (cons 1 2); #t
length (cons 1 2); throw runtime error
```

## Local bindings
```racket
; syntax, additional ([]) comparing with cond
;; form 1: all expressions are evaluated in the environment from before the let-expression, diff from ML
(let ([x1 e1]
      [x2 e2]
      ...
      [xn en])
     body)
; e2 cannot use x1
(define (double1 x)
  (let ([x (+ x 3)] ; x = x + 3
        [y (+ x 2)]) ; y = x + 2
    (+ x y -5))) ; x + 3 + x + 2 - 5 = 2x

; form 2: all expressions are evaluated in the environment produced from the previous bindings, same as ML
(define (double3 x)
  (let* ([x (+ x 3)] ; x = x+3
         [y (+ x 2)]); y = (x+3)+2 = x+5
    (+ x y -8))) ; x+3 + x+5 -8 = 2x

; form 3: all expression are evaluated in the environment that includes all the bindings, allow recursion
(define (triple x)
  (letrec ([y (+ x 2)] ; y = x+2
           [f (lambda (z) (+ z y w x)] ; f (z) = z+y+w+x
           [w (+ x 7)]) ; w = x+7
    (f -9))) ; f (-9) = -9+y+w+x = -9 + x+2 + x + 7+x =3x

; local defines are preferred racket style
(define (mod2 x)
  (define (even? x) (if (zero? x) #t (odd? (- x 1))))
  (define (odd? x) (if (zero? x) #f (even? (- x 1))))
  (if (even? x) 0 1))
; compare with the letrec impl
(define (mod2 x)
  (letrec
    ([even? (lambda (x) (if (zero? x) #t (odd? (- x 1))))]
     [odd? (lambda (x) (if (zero? x) #f (even? (- x 1))))]
    )
    (if (even? x) 0 1)
  )
)

; compare with/out local for max
(define (max xs)
  (cond [(null? xs) (error "given empty list")]
         [(null? (cdr xs)) (car xs)]
         [#t (if (> (car xs) (max (cdr xs))) (car xs) (max (cdr xs)))]
         ))
(define (max2 xs)
  (cond [(null? xs) error "given empty list"]
        [(null? (cdr xs)) (car xs)]
        [#t (let ([h (car xs)]
                  [t (max2 (cdr xs))])
                  (if (> h t) h t))]))
```

## Toplvel bindings
The bindings in a file work like local defines, i.e., `letrec`
* like ML, you can refer the earlier bindings
* unlike ML, you can also refer to later bindings, but refer to later bindings ONLY in function bodies
 * because bindings are evaluated in order
 * an error to use an undefined variable
* unlike ML, cannot define the same variable twice in module
Each file is imiplicityly a module. An moduel can shadow bidings from other modules it uses, so we could redefine `+` or any other funciton, but poor style and only shadoes in our module.

## Dynamic typing
With dynamic typing
* Frustrating not to catch "little errors" like `(n * x)` until you test your function -- this make testing more important
* But can use very flexible data structures and code without convincing a type checker that it make sense
Example:
* A list that can contain numbers or nested list of numbers
* Assuming *list or numbers "all the way down"*, sum all the numbers ...
* You need to check the type by yourself in the code, but it's hard to document the arguments :-(
```racket
(define xs (list 3 4 5 (list "hi" 1)))
(define ys (list (list 4 (list 5 0)) 6 7 (list 8) 9 2 3 (list 0 1)))
(define (sum1 xs) 
  (if (null? xs) 
      0 
      (if (number? (car xs)) 
          (+ (car xs) (sum1 (cdr xs))) 
          (if (list? (car xs))
              (+ (sum1 (car xs)) (sum1 (cdr xs)))
              (sum1 (cdr xs))))))
(sum1 xs);13
(sum1 ys);45 

(define (sum2 xs)
  (cond [(null? xs) 0]
        [(number? (car xs)) (+ (car xs) (sum2 (cdr xs)))]
        [(list? (car xs)) (+ (sum2 (car xs)) (sum2 (cdr xs)))]
        [#t (sum2 (cdr xs))]))
```

## Mutation with `set!` (pronounce *set bang*)
Unlike ML, racket does has assignment statement, but use *only-when-really-appropriate*.
* environment for closure is determined when funciton is defined, but body is evaluated when function is called.
* once an expression produces a value, it is irrelavant how the value was produced
* `set!` only change identifier (e.g. variables), not function like (car x)
```racket
;; syntax
(set! x e); like x=e in Java
;; sequence
(begin e1 e2 ... en); return value of en
(define x (begin
            (+ 5 3)
            (* 3 8)
            (- 3 2))); x=1

(define b 3); b=3
(define f (lambda (x) (* 1 (+ x b)))); if in ML, b always =3, but in racket, still cloure, but refer to b.
(define c (+ b 4)); c=7
(set! b 5); b=5
(define z (f 4)) ; z=9
(define w c) ; w=7
```
If something you need not to change might change, make a local copy of it. e.g.
```racket
(define b 3)
(define f 
  (let ([b b] ; define local inner b as a copy of outer b, which won't change with out b
        [* *]
        [+ +])
    (lambda (x) (* 1 (+ x b)))))
(set! b 10); 
(f 2); 5
```
Do not allow mutation, mutable top-level bindings a highly dubious idea.

## Cons Cell are immutable, but there is `mcons`
`cons` just makes a pair.
* often called a *cons cell*
* by convention and standar library, lists are nested pairs that eventually end with null
```racket
(define pr (cons 1 (cons #t "hi"))); '(1 #t . "hi")
(define lst (cons 1 (cons #t null))); '(1 #t)
car ; == ML #1
cdr ; == ML #2
(define lst (cons 1 (cons #t (cons "hi" null)))); '(1 #t "hi")
(caddr lst); "hi"
(list? pr); #f
(list? lst); #t
(pair? pr); #t
(pair? lst); #t YES true
(and (pair? lst) (list? lst)); #t
```

## mulatable pairs
*cons cell* are immutable. set! cannot mutate *cons cell*.
* `mcons` makes a mutable pair `(define x (mcons 14 null)))`
* `mcar` return the first component of mutable pair `(mcar x)`
* `mcdr` return the second component of a mutable pari `(mcdr x)`
* `mpair?` returns #t if givena mutable pair
* `set_mcar!` take a mutable pair and and expression and change the second component to be result the expression `(set_mcar x e)`
* `set_mcdr!` takes a mutable pair and an expression andn change the second component to be the result of the expression.
```racket
(set-car! x 45); only available for scheme
(define mpr (mcons 1 (mcons 2 3))); mpr=(mcons 1 (mcons 2 3))
(set-mcdr! mpr 47); mpr=(mcons 1 47)

lenth mpr; does NOT work
```

## Delayed Evaluation and Thunks
A key semantic issue for a language constructs is *When are its subexpression evaluated*.
* function arguments are eager (call-by-value)
* conditional branches are not eager (`if e bt bf`)
### Evaluated arguments in advance
* given (e1 e2 ... en) we evaluate the function arguments e2,...,en once before the evaluate the function body 
* given a function (lambda (...) ...) we do not evaluate the body until the function is called.

### Thunks
By using function, we can delay the evaluation. 
* A zero arugument function used to delay evaluation is called a **thunk**. e.g., "thunk the argument" means use `(lambda () e)` instead of `e`.
```racket
;; syntax
(lambda () e) ; define a thunked e
(e) ; evaluate e to some thunk and then call the thunk

; bad impl of if, which will force all of x y z to be evaluated before call my-if-bad
(define (my-if-bad x y z) (if x y z)) ; this will evaluate x, y, z before calling my-if-bad, which may cause recursive problem.
; a better imple of my-if, which delay the evaluation of y z by using thunk
(define (my-if x y z) 
  (if x (y) (z)))
(define (fact n)
  (my-if (= n 0)
      (lambda() 1)
      (lambda() (* n (fact (- n 1))))))
(fact 3); 6
```

## Lazy evaluation with Delay and Force
Lazy evaluation or call-by-need, or promises. The idea is to use mutation to remember the result from the first time we use the thunk so that we do not need to use the thunk again. i.e., thunks + multable pairs
* call-by-need: If an argument is never used it is never evaluated, else it ie evaluated only once
* call-by-value: aruguments are fully evluated before the call is made
```racket
;; create a pair holding the evaluated value
(define (my-delay th)
  (mcons #f th))
;; evalulate the value or return the evaluated value
(define (my-force p)
  (if (mcar p)
      (mcdr p)
      (begin (set-mcar! p #t)
             (set-mcdr! p ((mcdr p)))
             (mcdr p))))
;; example call, make best of both world of mutation and thunk
(define (my-mult x y-thunk)
  (cond [(= x 0) 0]
        [(= x 1) (y-thunk)]
        [#t (+ (y-thunk) (my-mult (- x 1) y-thunk))]))
(my-mult 3 (let ([p (my-delay (lambda () 777))])
               (lambda () (my-force p))))             
```

## Streams
A stream is a thunk that when called returns a pair `'(next-answer . next-thunk)`. Streams represent a infinite sequence of values. It can be implemented by writing two parts of codes:
* One part knows how to produce the infinite sequence 
* and other code that knows how to ask for however much of the squence it needs.
key ieda to implement:
```racket
; stream of ones
(define ones 
  (lambda ()
    (cons 1 ones)))

; natural integer sequence
(define nats (letrec ([f (lambda (x) (cons x (lambda () (f (+ x 1)))))])
              (lambda () (f 1))))
;; or
(define (f n)
  (cons n (lambda () (f (+ n 1)))))
(define nats (lambda () (f 1)))

; to access
(car ((cdr (nats)))); 2

; power of 2s
(define (powers-of-two)
  (letrec ([f (lambda (x) 
                (cons x (lambda () (f (* x 2)))))])
    (f 1)))

(define (number-until stream tester)
  (letrec ([f (lambda (stream acc)
                (let ([pr (stream)])
                  (if (tester (car pr))
                      acc
                      (f (cdr pr) (+ acc 1)))))])
    (f stream 1)))
(number-until powers-of-two (lambda (x) (= x 16))); 5
```

## Memoization(缓存)
An idiom related to lzay evaluation that does not actually use thunks is *memoization*. It based on following assumption:
* given the same arguments a function will always return the same result and have no side-effects.
We can lookup what the answer was the first time we called the function with the arguments. 

To implement memoization, we use mutation. Notice the below `assoc` and `memo` as cache, and `set!` for updating cache.
```racket
(define fibonacci 
  (letrec ([memo null] ; cache, i.e., list of pairs (x, fibonacci(x))
           [ f (lambda (x) ; local function
                 (let ([ans (assoc x memo)]) ; cache lookup
                   (if ans
                       (cdr ans)
                       (let ([new-ans (if (or (= x 1) (= x 2))
                                          1
                                          (+ (f (- x 1)) (f (- x 2))))])
                         (begin
                           (set! memo (cons (cons x new-ans) memo)) ; update the cache
                           new-ans)))))])
    f))
```

## Recursive datatypes via Racket's `struct`
A struct defines as this:
`(struct foo (bar baz quux) #:transparent #:mutable)`
* defines a new *struct* called *foo* that is like an ML constructor
* It adds the environment functions like:
 * *foo* is a function that takes three arguments and return a value that is a foo field holding the first argument, bar field hoding the second argument, and quux filed holding the third argument
 * *foo?* is a function that return #t for values created bt calling *foo* and #f for everything else
 * *foo*-**field** are functions that takes a foo and returns the contents of the *field*, raising an error if passed anything other thatn a foo. e.g. foo-bar, foo-baz, and foo-quux
 * *set-foo*-**field** are funtions that mutate the *field* to some special value. The *set-* functions are only available if *#:mutable* is appear
 * the *#:transparent* meks the fileds and accesor function s visible even outside the module that defines the struct.
```racket
(struct const (int) #:transparent)
(struct negate (e) #:transparent)
(struct add (e1 e2) #:transparent)
(struct multiply (e1 e2) #:transparent)

(define (eval-exp e)
  (cond [(const? e) e]
        [(negate? e) (const (- (const-int (eval-exp (negate-e e)))))]
        [(add? e) (let ([v1 (const-int (eval-exp(add-e1 e)))]
                        [v2 (const-int (eval-exp(add-e2 e)))])
                    (const (+ v1 v2)))]
        [(multiply? e) (let ([v1 (const-int (eval-exp (multiply-e1 e)))]
                             [v2 (const-int (eval-exp (multiply-e2 e)))])
                         (const (* v1 v2)))]
        [#t (error "eval-exp expected an exp")]))
```

## Implementing a Programming Language in general
We can describe a typical workflow for a language **A** implementation as follows:
* Input: a string holding the *concrete syntax* of a program in the language. Typically, this string would be the contents of one or more files.
* The *parser* take the input and produce a *tree* (*abstract-syntax-tree* or AST in short) that represents the program if the input is sysntactically well-formed. Otherwise, the *parser* gives error.
* For language including *type-checking* rules, the *type-checker* will use this AST to either report error or not.
* The AST then passed to the rest of the implementation:
 * either an interpreter(writen in another language **B**) which take programs in **A** and produces answers. Also named *evaluater* or *excutor* for **A**
 * or a compiler(writen in another lanugage **B**) which take program in **A** and produces equivalent programs in some other language **C** and then uses some pre-existing implmenetation for **C**. Also named *translator*. Language **A** called *source language* and **C** the *target language*.
For either the interpreter and the compiler approach, we call **B** the language which we are writing the implementation of **A**, the *metalanguage*.

*Interpreter versus compiler is a feature of a particular programming-language implementation, not a feature of the programming language*. You can write an interpreter for *C* although it was so called (incorrectly) as "compiled language".

### INterpreters for languages with Varialbes need evironments
Ideally a hash map, also called *symbol table*. In Racket, a simple association list holding pairs of strings and values can suffice.

### Implementing closures
To implement a language with function closures and lexical scope, the interpreter needs to "remember" the environment that "was current" when the function was defined, so that it can use this environment *instead of* the caller's environment when the function is called. To do that:
* Create a small data structure called a *closure* that includes the environment along with (+) the function itself.
* Using above pair (closure=env+code) to interpre a function. In other word, a function is not a value, *a closure is*.
So the evaluation of a function produces a closure that "remember" the environment from when we evaluated(or defined) the function.

Only the *free variables* (variables used in the function body whose definition are outside the function body) should be stored in the einvironment of closure.

### ML vs. Racket
Think ML is a subset of Racket, which 
* reject bug-likely programs, by type-checking. 
* On the othersid, reject Raket-like programs that are not bugs as well.
```Racket
; good reject case
(define (f y) (+ y (car y)))

; bad reject case
(define (f x) (if (> x 0) #t (list 1 2)))
(define xs (list 1 #t "hi"))
(define y (f (car xs)))
```
Raket accepts a superset of programs, some of which are erors and some of which of no. Racket is just ML where *every experession is part of one big datatype*. 

## Static checking vs dynamic checking
**static checking** is anything done to reject a program *after* it (successfully) parses but *before* it runs. This is different from "syntax error" or "parsing error" which fails the parsing. *static checking* typically means a "type error", which include:
* undefined variables
* using a number instead of a pair
*static checking* is "compile-time checking" which was done without any input to the program identified. It is irrelevant whether the language implementation using a comipler or interfperter after static checking succeeds.

*Dynamic cheching* (i.e., runt-time checking) can be done by taging each value and then do a similar static checking.

## Correctness: Soundness, Completeness, Undecidability
Given X
*soundness* if it never accept a program that when run with some input, does X (prevent *false negative*, also if)
*completeness* if it never reject a program that run with any input, does not do X (prevent *false positive*, also only if)
Similar to iff.
In mordern languages, type system are sound(reject what they claim to), but not complete (reject program which need NOT reject)

Type systems are *not compete* because for almost anything you might like to check statically, it is *impossible* to implement a static checker that given any program in your languate
* (a) always terminates
* (b) is sound
* (c) is complete
Since we have to give up one, (c) seems like the best option (programmers do not like compilers that may not terminate)

The impssibility result is exactly the idea of *undecidability* at the heart of the study of the theory of computation.

## Weakly typed vs strongly typed
It is programmer's fault if some error happens and the language definition doe not have to tyoe check.

*weakly typed* - if a language has program where a legal implementaion is allowed to set the computer on fire, we call the languate *weekly typed*. such as C/C++. 
 * People favor of weakly typing is by saying "Strong types for weak minds". The idea is that any storngly typed language either rejecting program statically or performing unnecessary tests dynamically, so a human should be able to "overrule" the check in places where he knows they are unnecessary.

*strongly typed* - language where the behavior of buggy programs is more limited are called *strongly typed*. such as Java/ML/Scala. 
 * The implementation of strongly type check not only cost-perfoming, also because it has to keep around extra data (like tags on values) to do the check. 
 * Howevever, in reality, humans are extreamly error-prone and favor strongly typed lanugage, and moreever, type system have gotten more expressive over time, and lanugage implmenations are gotten bettern at optimizing away unnecessary checkes.

## Advantage and disadvantage of static checking
* Is static or dynamic typing more Convenient
 * dynamic is more convenient since it callow to mix-and-match different knid s of data such as numbers, strings, and pairs without having to declare new type definitions or "clutter" code with pattern-matching. 
 ```racket
 ; in racket, a function can return different types
 (define (f y) (if (> y 0) (+ y y) "hei"))
 (let ([ans (f x)] (if (number? ans) (number->string ans) ans))
 ```
 * static typing is more convient becuase it assume data has a certain type, and less error-prone.
* Does static typing prevent useful programs?
 * dynamic typing does not reject programs that make perfect sense
 * static typing become more expressive on the other hand
* Is static typing's early bug-detection important?
 * bugs are easier to fix if discoverd sooner.
 * static typing catch bug by type-check
 * dynamic typing catch bug with testing, since you need to test your program, the additional value of catching some bugs before you run the test is reduced. So, for dynamic typing, you need a more complete testing than static typing.
* Does static or Dynamic typing lead to better performance?
 * static typing can lead to faster code since it does not need to perforce type tests at run time.
 * dynamic typing can optimize the check, and static typing sometime has to work around type-system limitation which erode the supposed performance advantages.
* Does static or dynamic typing make code resuse easier?
 * Dynamic typing arguably make it easier to reuse library fucntions. e.g., if you build lots kinds of data out of cons cells, you can just keep using car, cdr, cadr, etc. to get the pieces out rather than defining lots of different getter functions for each data structure. On the other hand, this can mask bugs.
 * This is really and interesting design issue more general than just static vs dynamic typing.
* Is static or dynamic typing better for prototyping?
 * early in a software project, you are develop prototype, often at the same time you are changing your views on what the software will do and how the impl will approach doing it. 
 * Dynamic typing is often considered better for prototyping since you do not need to expend energy definign the types of variables, functions, and data structures when those decidions are in flux. 
 * Static typing argue that it is never too early to document the types in your software design even it they are unclear and changing. Moreover, commeing our code or adding stubs like patter-match branches of form `_=>raise Unimplemented` is often easy and documents what parts of the program are known not to work.
* Is Static or dynamic typing better for code evolution?
 * Dynamic typing is sometimes more convenient for code evolution becuase we can change code to be more permissive (accept arguments of more types) without have to change any of the pre-exsiting clients of the code.
 ```racket
 ;; old
 (define (f x) (* x 2))
 ;; new, it will keep old callers work, and also new caller pass in strings. However, this also means old caller will not type-check.
 (define (f x) (if (number? x) (* x 2) (string-append x x )))
 ```
 * on the other hand, static type-checking is very useful when evolving code to catch bugs that evolution introduced. When we change the type of a function, all callers no longer type-check, which means the type-chekcer gives us an invaluable "to-do" list of all the call-sites that need to change. however, it may also be frustrating that the program will not run until all items on the "to-do" list are addressed.

## **eval** and **quote**
The **eval** premitive take a piece of Raket code, and at run-time, evaluate it. **quote** will treat everything under it as symbols, numbers, lists, etc, not as functions to be called. **eval** and **quote** are inverse.
```racket
(define (make-some-code y)
  (if y 
     (quote (begin (print "hi") (+ 4 2)))
     (quote (+ 5 3))))

;; quasiquote == interpolation   
(quasiquote (1 2 (unquote (+ 1 2)) (unquote (- 5 1)))) ; '(1 2 3 4)

```

### Interpolation
The ability to embed expression evaluation inside a string (which one might or might not then call **eval** on, just as one might or might not use a Racket quote expression to build something for **eval**). This feature is called **interpolation** in scripting language, but is it jsut *quasiquoting* in Racket.
```ruby
puts "#{1+2+3}"
```
 

## Reference
* [Racket guide](http://docs.racket-lang.org/guide)
