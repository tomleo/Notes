Private vs. Public Classes
==========================

- All variables and methods are accessable.
- Functions with a starting with an underscore are conventionally private [1]_
- Variables of a class can be directly accessed or you can use the @property
  decorator to create getters/setters/deleters. 

Python Imports
==============

When you do `from x import y`, ALL of x is imported. Only y is brought into the
current namespace, **but the entire module x is processed**

.. code-block:: python

    #Module one
    def a():
        print "a"
    print "hi"
    def b():
        print "b"
    print "bye"

    #Module two
    from test import b
    b()

    #Running module two will output the following
    hi
    bye
    b

Everything in moduel two was run, even though you only imported b [3]_

Cyclic Imports
==============

Cyclic imports are a sign that a module should probably be split up.

If for whatever reason you need to work with code with cyclical imports which
cannot be re-arranged, import foo is the only option. [2]_

.. code-block:: python

    #good
    import myapp.utils as utils; utils.frobnicate()
    import myapp.utils as U; U.frobnicate()

    #bad
    from myapp.utils import frobnicate

    # If you're importing multiple things from one module, it's probably better
    # to just import the module, and do module.bar, module.foo, module.baz, etc.,
    # to keep the namespace clean. Avoid the following:
    from myapp.utils import frobnicate, foo, bar, baz, MyClass, SomeOtherClass, # yada yada


Inheritance
===========

Subclassing unittest.TestCase
-----------------------------

The following is a comment by Piet Delport (8:25 PM on 28 Oct 2012) [4]_


In Python (and any C3 class system), every class declaration should be read as
a single partial order: class A(B, C) should be understood to mean that A < B <
C, without putting special weight on A. (It can help to mentally ignore the
parentheses and actually read the syntax as "class A < B < C: ...", when in
doubt.) 

In other words, the implication that B becomes a subclass of C is just as
important and meaningful as the implication that A becomes a subclass of B. In
particular, the question of swapping B and C around is no different in general
than the question of swapping A and B around, and can have equally large
repercussions. 

Looking back at the example:

.. code-block:: python

    class RealTestCase(BaseTestCase, MyMixin):

This declaration should immediately jump out as a red flag: why is the mixin
being declared more general (BaseTestCase < MyMixin), when it actually depends
on the setUp() method provided by BaseTestCase or its ancestors (implying
MyMixin < BaseTestCase, or MyMixin < unittest.TestCase)? 

This points to the more serious underlying bug:

.. code-block:: python

    class MyMixin(object):
        def setUp(self):
            super(MyMixin, self).setUp()

As it stands, this code is unconditionally buggy: it declares a dependency on
object alone (MyMixin < object), but proceeds to super-call a method (setUp())
that object does not provide. 

Fixing this bug requires either removing the super-call (making MyMixin the
introducer of a new setUp() method), or inserting a dependency on an
appropriate base class (to extend its setUp() method):

.. code-block:: python

    class MyMixin(unittest.TestCase, object)  # MyMixin < TestCase < object
    class MyMixin(unittest.TestCase)          # equivalent shorthand

The bug can also be papered over by adding the missing dependency in a subclass:

.. code-block:: python

    class Foo(MyMixin, unittest.TestCase)  # MyMixin < TestCase
    class Bar(MyMixin, BaseTestCase)       # MyMixin < BaseTestCase (< TestCase)

but this merely hides the underlying bug: any subclass that does not declare
MyMixin's dependency for it will still hit the problem.

Server Programming
==================

Simple Server
-------------

.. code-block:: terminal256

    python -m SimpleHTTPServer 5000



Data Visualization
==================

Venn Diagrams
-------------

Left Handed People: 500
Right Handed People: 2300
Ambidextrous People: 23

.. code-block:: python

    from matplotlib import pyplot as plt
    from matplotlib_venn import venn2
    plt.figure(figsize=(4,4))
    plt.title("Venn Diagram: Which hand do people favor")
    v = venn2(subsets=(2300,23,500), set_labels=('Right Handed', 'Left Handed'))
    plt.show()

.. [1] http://stackoverflow.com/a/4555970
.. [2] http://stackoverflow.com/a/1704156
.. [3] http://stackoverflow.com/a/1704262
.. [4] http://nedbatchelder.com/blog/201210/multiple_inheritance_is_hard.html
