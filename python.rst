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

.. [1] http://stackoverflow.com/a/4555970
.. [2] http://stackoverflow.com/a/1704156
.. [3] http://stackoverflow.com/a/1704262
