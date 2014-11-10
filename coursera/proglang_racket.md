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

## Mutation with `set
Unlike ML, racket does has assignment statement, but use *only-when-really-appropriate*.
* environment for closure is determined when funciton is defined, but body is evaluated when function is called.
* once an expression produces a value, it is irrelavant how the value was produced
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

## Cons Cell are immutable, but there is `mcons`
* `mcons` makes a mutable pair `(define x (mcons 14 null)))`
* `mcar` return the first component of mutable pair `(mcar x)`
* `mcdr` return the second component of a mutable pari `(mcdr x)`
* `mpair?` returns #t if givena mutable pair
* `set_mcar!` take a mutable pair and and expression and change the second component to be result the expression `(set_mcar x e)`
* `set_mcdr!` takes a mutable pair and an expression andn change the second component to be the result of the expression.

## Delayed Evaluation and Thunks
A key semantic issue for a language constructs is *When are its subexpression evaluated*.

### Evaluated arguments in advance
* given (e1 e2 ... en) we evaluate the function arguments e2,...,en once before the evaluate the function body 
* given a function (lambda (...) ...) we do not evaluate the body until the function is called.

### Thunks
By using function, we can delay the evaluation. When we use a zero arugument function to delay evaluation, we call the function a **thunk**. e.g., "thunk the argument" means use `(lambda () e)` instead of `e`.
```racket
(define (my-if-bad x y z) (if x y z)) ; this will evaluate x, y, z before calling my-if-bad, which may cause recursive problem.
(my-if e1 (lambda () e2) (lambda () e3)) ; this delay the evaluation to e2, e3
```

## Lazy evaluation with Delay and Force
Lazy evaluation or call-by-need, or promises. The idea is to use mutation to remember the result from the first time we use the thunk so that we do not need to use the thunk again.

* call-by-need: If an argument is never used it is never evaluated, else it ie evaluated only once
* call-by-value: aruguments are fully evluated before the call is made

## Streams
Streams is a infinite sequence of values. It can be implemented by writing two parts of code
* One part knows how to produce the infinite sequence 
* and other code that knows how to ask for however much of the squence it needs.
```racket
; natural integer sequence
(define nats (letrec ([f (lambda (x) (cons x lambda() (f (+ x 1))))])
              (lambda () (f 1))))
; to access
(car ((cdr (nats))))
```

## Memoization(缓存)
An idiom related to lzay evaluation that does not actually use thunks is *memoization*. It based on following assumption:
* given the same arguments a function will always return the same result and have no side-effects.
We can lookup what the answer was the first time we called the function with the arguments. 

To implement memoization, we use mutation. 
 
## Reference
* [Racket guide](http://docs.racket-lang.org/guide)
