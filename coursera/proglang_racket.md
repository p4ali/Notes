## Definitions
```racket
(define a 3)
(define b (+ a 2))
```

## function
```racket
(define add (lambda (x y) (+ x y)))
(difine cube (lambda (x) (* x x x)))
(define (cube x) (* x x x))
(define (pow x y)
  (if (= y 0)
      1
      (* x (pow x (-y 1)))))
```

# boolean
```racket
#t
#f
````
