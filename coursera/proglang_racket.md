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


