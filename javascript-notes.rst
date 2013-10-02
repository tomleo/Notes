use strict mode in javascript

How to define functions and objects in JS
-----------------------------------------

To create an object you can use Object.create function or simply write the
function explicitly like below.

.. code:: javascript

    function myMethod() { return this.val; }
    var object1 = {
        get: myMethod,
        val: 42
    };
    var object2 = {
        get: myMethod,
        val: 3.14159
    };
    var object3 = {
        get: myMethod.call(),
        val: 42
    };

.. code:: console

    >>> this.object1
    Object { val=42, get=myMethod()}
    >>> this.object2
    Object { val=3.14159, get=myMethod()}
    >>> this.object3
    Object { val=42}



Prototypal Inheritance
----------------------

When looking for attributes JS will go up the inheritance chain until it finds
what it is looking for. The Object.create(obj) is a way to create an object
based on obj prototype.

.. code:: javascript

    var parent = {
        get: function fn() {
            return this.val;
        },
        val: 42
    };
    var child = Object.create(parent);
    child.val = 25;

    var grandchild = Object.create(child);

.. code:: console

    >>> parent.get();
    42 //parent.get() -> fn() which looks for -> this.val=42

    >>> child.get();
    25 //child.get() does not exist so looks in parent for get -> parent.get()
       //which looks for this.val=25

    >>> grandchild.get();
    25 //grandchild.get() does not exist so looks in child for get -> child.get()
       //does not exist so looks in parent for get -> parent.get() which looks for
       //this.val which does not exist so looks for child.get=25

Polymorphism
-------------

.. code:: javascript

    var answer = {
        get: function fn1() {
            return this.val;
        },
        val: 42
    };

    var firmAnswer = Object.create(answer);
    firmAnswer.get = function fn2() {
        return this.val + "!!";
    };

.. code:: console

    >>> answer.get();
    42
    >>> firmAnswer.get();
    "42!!"

Getting the this right or... getting the right this
---------------------------------------------------

.. code:: javascript

    var hello = {
        get: function fn1() {
            return this.val;
        },
        val: "Hello"
    };

    var helloWorld = Object.create(hello);
    helloWorld.get = function fn2() {
        return hello.get() + " World";
    };
    helloWorld.val 

    var goodByeWorld = Object.create(hello);
    goodByeWorld.get = function fn2() {
        return hello.get.call(this) + " World";
    goodByeWorld.val = "Goodbye";

.. code:: console

    >>> helloWorld.get()
    "Hello World"
    >>> goodByeWorld.get()
    "Goodbye World"

Using Constructors
------------------

Instead of defining a class' variables you can pass variables to a
constructor.

.. code:: javascript

    var helloPrototype = {
        constructor: function fn0(val) {
            this._val = val;
        },
        get: function fun1() {
            return this._val;
        }
    };

    var helloWorldPrototype = Object.create(helloPrototype);
    helloWorldPrototype.get = function fn2() {
        return helloPrototype.get.call(this) = "World";
    };

.. code:: console

    var hello = Object.create(helloPrototype);
    helloWorld.constructor("Hello");
    helloWorld.get();

    var helloWorld = Object.create(helloWorldPrototype);
    helloWorld.constructor("Hello");
    helloWorld.get();

Left off at 15:00 [1]_

.. [1] http://www.objectplayground.com/


