use strict mode in javascript

Type
----

typeof will always return "object" for native non callable objects.

.. code:: javascript

    >> [typeof null, null instanceof Object]
    ["object", false]

the + operator has higher precedence than the ternary one

.. code:: javascript

    >> var val = 'smtg';
    >> console.log('Value is ' + (val === 'smtg') ? 'Something' : 'Nothing');
    Something 

The + sign concatenates strings, the - sign causes a type cast

.. code:: javascript

    >> '5' + 3
    "53"
    >> '5' - 3
    2

Scope
-----

The var declaration is hoisted to the function scope, but the initialization is not.

.. code:: javascript

    var name = 'World!';
    (function () {
        if (typeof name === 'undefined') {
            var name = 'Jack';
            console.log('Goodbye ' + name);
        } else {
            console.log('Hello ' + name);
        }
    })();
    Goodbye Jack

Local vs. Global Functions, y is an automatic global, not a function local one like x

.. code:: javascript

    (function(){
      var x = y = 1;
    })();
    console.log(y);
    console.log(x);
    1
    RefereceError: x is not defined

Switch Statement
----------------

switch uses === internally and new String(x) !== x

.. code:: javascript

    function showCase(value) {
        switch(value) {
        case 'A':
            console.log('Case A');
            break;
        case 'B':
            console.log('Case B');
            break;
        case undefined:
            console.log('undefined');
            break;
        default:
            console.log('Do not know!');
        }
    }
    showCase(new String('A'));
    Do not know!


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


It should be noted that name is a read only property for example

.. code:: javascript

    function foo() { }
    var oldName = foo.name;
    foo.name = "bar";
    [oldName, foo.name]
    ["foo", "foo"]



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

The following code example evaluates to false

.. code:: javascript

    function f() {}
    var a = f.prototype, b = Object.getPrototypeOf(f);
    a === b

This is because f.prototype is the object that will become the parent of any objects created with
new f while Object.getPrototypeOf returns the parent in the inheritance hierarchy.
        

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


Nodejs
------

Node virtual machine
https://github.com/creationix/nvm


Left off at 15:00 [1]_


Arrays
------

.. code:: javascript

    var x = [1,2,3,]; //ERROR
    var y = {'a': 1, 'b': 2, 'c': 3,}; //ERROR

If, when defining an array or an object, you leave a trailing comma after the
last item in your collection, IE will fail to parse your javascript file:

Design Patterns
===============

Shifting of classes
-------------------

If you are going to be adding/removing classes of an element you should cache
them.

.. code:: javascript

    var navright = document.querySelector('#right');
    var navleft = document.querySelector('#left');
    var navup = document.querySelector('#up');
    var navdown = document.querySelector('#down');

Furthermore if you are going to be adding a class to indicate some styling
state of an element you should do the following [2]_

.. code:: javascript

    var current = null;
    document.body.addEventListener('click', function(ev) {
        if (ev.target.tagName === 'BUTTON') {
            if (current) {
                current.classList.remove('focus');
            }
            current = ev.target;
            current.classList.add('focus');
        }
    }, false);




.. [1] http://www.objectplayground.com/
.. [2] http://jsfiddle.net/codepo8/cb7pG/3/light/

