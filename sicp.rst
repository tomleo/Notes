=================================================
Structure and Interpretation of Computer Programs
=================================================

normal-order evaluation -> fully expand and then reduce
applicative-order -> evaluate the arguments and then apply

Conditionals
------------

.. code-block:: lisp

    (cond (<p1> <e1>)
          (<p2> <e2>)
          ...
          (<p^n> <e^n>))

    (cond (<p1> <e1>) (else <e2>))

    (if <predicate> <consequent> <alternative>)


    (and <e1> … <en>)
    (or <e1> … <en>)
    (not <e>)

Exercise 1.1
------------

.. code-block:: lisp

    10  ;;10
    (+ 5 3 4) ;;12
    (- 9 1) ;;8
    (/ 6 2) ;;3
    (+ (* 2 4) (- 4 6)) ;;6
    (define a 3)    ;;a=3
    (define b (+ a 1))  ;;b=4
    (+ a b (* a b)) ;;19
    (= a b) ;;false
    (if (and (> b a) (< b (* a b)))
        b
        a) ;;4
    (cond ((= a 4) 6)
          ((= b 4) (+ 6 7 a)) ;;16
          (else 25))
    (+ 2 (if (> b a) b a)) ;;6
    (* (cond ((> a b) a)
             ((< a b) b)
             (else -1))
       (+ a 1)) ;;16

Exercise 1.2
------------

.. code-block:: lisp

    (/
        (+ 5 4 (-2 (- 3 (+ 6 (4/5)))))
        (* 3 (- 6 2)(- 2 7))
    )

Exercise 1.3
------------

.. code-block:: lisp

    (define (square a) (* a a))
    (define (sum-of-squares a b) (+ (square a) (square b)))

    (define (sum-of-greatest-squares a b c)
      (cond ((and (> a b) (> b c)) (sum-of-squares a b))
            ((and (< a b) (< b c)) (sum-of-squares b c))
            ((and (> a b) (< b c)) (sum-of-squares a c))))

    ;;TEST
    (=
     (sum-of-greatest-squares 1 2 3)
     (sum-of-squares 2 3))

Exercise 1.4
------------

.. code-block:: lisp

    (define (a-plus-abs-b a b)
        ((if (> b 0) + -) a b))

The easiest way for me to describe the behavior of the list function is to
translate it into python, so here is the same function written in python.

.. code-block:: python

    def a-plus-abs-b(a, b):
        if b>0:
            return a+b
        else:
            return a-b

.. code-block:: java

    public int a-plus-abs-b(int a, int b) {
        if (b>0)
            return a+b;
        else
            return a-b;
   }

.. block:: c++

    function int a-plus-abs-b(int a, int b)
    {
        if (b>0) { return a+b; }
        else { return a-b; }
    }

Exercise 1.5
------------

.. code-block:: lisp
    (define (p) (p))
    (define (test x y) (if (= x 0) 0 y))
    (test 0 (p))


applicative-order evaluation::

    ;;subsitution followed by reduction
    (test 0 (p))
    (define (test 0 (p)) (if (= 0 0) 0 (p)))
    (if (= 0 0) 0 (p))
    0


normal-order evaluation?::

    ;;expansion followed by reduction
    (test   0   (p))
    (if (= 0 0) 0 (p))
    0

