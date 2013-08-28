=================================================
Structure and Interpretation of Computer Programs
=================================================

normal-order evaluation -> fully expand and then reduce
applicative-order -> evaluate the arguments and then apply

Conditionals
------------

.. block:: lisp

    (cond (<p1> <e1>)
          (<p2> <e2>)
          ...
          (<p^n> <e^n>))

    (cond (<p1> <e1>) (else <e2>))

    (if <predicate> <consequent> <alternative>)


    (and <e1> … <en>)
    (or <e1> … <en>)
    (not <e>)

