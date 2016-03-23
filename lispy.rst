The Joy of Clojure
==================

pg 16
-----

- Lisp can be likened to a self-licking lollipop—more for mally defined as
  homoiconicity. 
- First-class functions can be stored, passed, and returned just like any other
  piece of data.


pg19
----

::
    Polymorphism is the ability of a function or method to perform different actions
    depending on the type of its arguments or a target object. Clojure provides polymor-
    phism via protocols,13 which let you attach a set of behaviors to any number of existing
    types and classes; they’re similar to what are sometimes called mix-ins, traits, or inter-
    faces in other languages and are more open and extensible than polymorphism in
    many languages.

Scalar Data Types
=================

Integers, Floating-point, Rationals, Symbols, Keywords, Strings, Characters

Symbols
-------

Used to represent another value. When a number or a string is evaluated, you
get back exactly the same object

.. code-block:: clojure

    (def foo 22/7)
    foo
    ;;=> 22/7

When a symbol is evaluated, you get back whatever value that symbol is
referring to in the current context. In other words, symbols are typically used
to refer to function parameters, local variables, globals, and Java classes.

Keywords
--------

Always evaluate to themselves

.. code-block:: clojure

    :hello
    :2

Lists
-----

.. code-block:: clojure

    (foxtrot uniform charlie kilo)

:: 

    When a list is evaluated, the first item of the list—yankee in this case—is resolved to a
    function, macro, or special operator. If yankee is a function, the remaining items in
    the list are evaluated in order, and the results are passed to yankee as its parameters. (pg 29)

    NOTE: A form is any Clojure object meant to be evaluated, including but not
    limited to lists, vectors, maps, numbers, keywords, and symbols. A special
    form is a form with special syntax or special evaluation rules that
    typically aren’t implemented using the base Clojure forms. An example of a
    special form is the . (dot) operator used for Java interoperability
    purposes.

Vectors
-------

.. code-block:: clojure

    [1 2 3 4]

vectors evaluate each item in order. No function or macro call is performed on
the vector itself, although if a list appears within the vector, that list is
evaluated following the normal rules for a list.


Maps
----

.. code-block:: clojure

    {1 "one" 2 "two" 3 "three"}

every item in a map literal (each key and each value) is evaluated before the
result is stored in the map. (the order in which they’re evaluated isn’t guaranteed)

Sets
----

.. code-block:: clojure

    #{1 2 "skioo" :five 0x4}


Anonymous Functions
-------------------

.. code-block:: clojure

    (fn [x y]   ;;<-- A vector of the function parameters
        (println "Making a set")    ;;<-- Function body
        #{x y})     ;;<-- The last expression gives the return value


    ((fn [x y]  ;;<-- Define a function and call it right away
        (println "Making a set")
        #{x y})
     1 2)   ;;<-- Pass 1 and 2 to the function

Create a named function

.. code-block:: clojure

    (def make-set
        (fn [x y]
            (println "making a set")
            #{x y}))

Create a named function using ``defn`` macro

.. code-block:: clojure

    (defn make-set
        "Takes two values and makes a set from them."
        [x y]
        (println "Making a set")
        #{x  y})

Arity
    the differences in the argument count that a function will accept 

.. code-block:: clojure

    (defn make-set
        ([x]    #{x})
        ([x y]  #{x y}))


n-arguments

.. code-block:: clojure

    (def arity+ [first second & more]
        (vector first second more))





