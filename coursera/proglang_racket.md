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

## function
**lambda** is used to create an anonymous function. e.g., `(lambda (x) e)`
```racket
(define add (lambda (x y) (+ x y)))
(difine cube (lambda (x) (* x x x)))
(define (cube x) (* x x x))
(define (pow x y)
  (if (= y 0)
      1
      (* x (pow x (-y 1)))))
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

# boolean
```racket
#t
#f
````

## Local bindings
```racket
; e2 cannot use x1
(let ([x1 e1] [x2 e2]) e)

; now y can reference x, but no recursion
(let* ([x (+ x 3)]
       [y (+ x 2)])
    (+ x y -8))

; allow recursion
(define (triple x)
  (letrec ([y (+ x 2)]
           [f (lambda (z) (+ z y w x)]
           [w (+ x 7)])
    (f -9)))
```

