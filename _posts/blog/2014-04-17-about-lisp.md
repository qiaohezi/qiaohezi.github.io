---
layout: post
title: About Lisp
description: 《Structure and Interpretaion of Computer Programs》读书笔记
category: blog
---



##《Structure and Interpretaion of Computer Programs》

* ###一个栗子

```lisp
#lang racket
(define (fibonacci n)
  (cond ((= n 1) 1)
        ((= n 0) 0)
        (else (+ (fibonacci (- n 1)) (fibonacci (- n 2))))))
```

```lisp
#lang racket
(define (A x y)
  (cond ((= y 0) 0)
        ((= x 0) (* 2 y))
        ((= y 1) 2)
        (else (A (- x 1)
                 (A x (- y 1))))))
```

```lisp
#lang racket
(define (count-change amount)
  (cc amount 5))

(define (cc amount kinds-of-coins)
  (cond ((= amount 0) 1)
        ((or (< amount 0) (= kinds-of-coins 0)) 0)
        (else (+ (cc amount
                     (- kinds-of-coins 1))
                 (cc (- amount
                        (first-denomination kinds-of-coins))
                     kinds-of-coins)))))
(define (first-denomination kinds-of-coins)
  (cond ((= kinds-of-coins 1) 1)
        ((= kinds-of-coins 2) 5)
        ((= kinds-of-coins 3) 10)
        ((= kinds-of-coins 4) 25)
        ((= kinds-of-coins 5) 50)))
```

##练习1.11
```lisp
#lang racket
(define (f x)
  (if (< x 3) x
        (+ (f (- x 1)) (* 2 (f (- x 2))) (* 3 (f (- x 3)))
           
   )))


(define (f2 n)
  (f2-iter 2 1 0 n))

(define (f2-iter a b c count)
  (if (= count 0) c
      (f2-iter (+ a (* 2 b) (* 3 c)) a b (- count 1))))
```

##求幂
```lisp
#lang racket
;;通用函数
(define (even? n)
  (= (remainder n 2) 0))

(define (square n) (* n n))

;;递归
(define (expt0 b n)
  (if (= n 0)
      1
      (* b (expt0 b (- n 1)))))
;;迭代
(define (expt1 b n )
  (expt-iter1 b n 1))

(define (expt-iter1 b counter product)
  (if (= counter 0) product
      (expt-iter1 b (- counter 1) (* b product))))

;;优化后的迭代
(define (expt b n)
  (cond ((= n 0) 1)
        ((even? n) (square (expt b (/ n 2))))
        (else (* b (expt b (- n 1))))))
```

##加法
```lisp
;;递归 t:o(n) s:o(n)
(define (*0 a b)
  (if (= b 0) 0
      (+ a (* a (- b 1)))))

;;迭代 t:o(n) s:o(1)
(define (* a b)
  (*-iter a b 0))

(define (*-iter a b product)
  (cond ((= b 0) product)
        (else (*-iter a (- b 1) (+ a product)))))

;;迭代优化 t:o(log n) s:o(1)
(define (mul a b)
  (cond ((= b 0) 0)
        ((even? b) (* 2 (mul a (/ b 2))))
        (else (+ a (mul a (- b 1))))))

```
##通过区间折半寻找方程的根
```lisp
(define (average x y) (/ (+ x y) 2))
(define (positive? x) (> x 0))
(define (negative? x) (< x 0))
(define (close-enough? x y)
  (< (abs (- x y)) 0.0001))

;;测试输入函数
(define (t-func x) (- (* x 3) 3))

(define (search f neg-point pos-point)
  (let ((midpoint (average neg-point pos-point)))
    (if (close-enough? neg-point pos-point)
        midpoint
        (let ((test-value (f midpoint)))
        (cond ((positive? test-value)
               (search f neg-point midpoint))
              ((negative? test-value)
               (search f midpoint pos-point))
              (else midpoint))))))
```