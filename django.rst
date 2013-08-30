============
Django Notes
============

Debugging
=========

Error: cannot import name <Name>
--------------------------------

STR: When running manage.py runserver normally a traceback will be displayed
however, sometimes you get the cryptic error "cannot import name <name>"

Solution::
    
    python manage.py shell --traceback

It should also be noted that the problem can sometimes be in the middle of the
traceback not at the end. _[1]

.. [1]: http://stackoverflow.com/a/8797685/465270
