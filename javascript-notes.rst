use strict mode in javascript

Type
====

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

Comparison
----------

[6]_

.. code:: javascript

    // Objects evaluate to true
    if( {} ) {
        console.log("Really?"); // => Really?
    } 

    Boolean([]) => true
    Boolean({}) => true
    Boolean(null, NaN, 0, '', undefined) => false

Scope
=====

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

Hoisting
========

Variable declarations get hoisted to the top of their scope, their assignment does not [5]_

.. code:: javascript

    function example() {
        console.log(declaredButNotAssigned); // => undefined
        var declaredButNotAssigned = true;
    }

    function example() {
        var declaredButNotAssigned;
        console.log(declaredButNotAssigned); // => undefined
        declaredButNotAssigned = true;
    }

Anonymouse function expressions hoist their variable name, but not the function assignment

.. code:: javascript

    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }


    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // the same is true when the function name
    // is the same as the variable name.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }

Function declarations hoist their name and function body

.. code:: javascript

    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }


Switch Statement
================

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
=========================================

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
======================

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
=============

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

.. code:: javascript

    // variables referencing "this" should be named _this
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }

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
======



Node virtual machine
--------------------

https://github.com/creationix/nvm

.. note::

    in practice, I've found nvm confusing to use. I docker container may be a
    better way to go


Left off at 15:00 [1]_

Installing Node
~~~~~~~~~~~~~~~

curl -sL https://deb.nodesource.com/setup | sudo bash -
sudo apt-get install -y nodejs


Arrays
======

.. code:: javascript

    var x = [1,2,3,]; //ERROR
    var y = {'a': 1, 'b': 2, 'c': 3,}; //ERROR

If, when defining an array or an object, you leave a trailing comma after the
last item in your collection, IE will fail to parse your javascript file:

Design Patterns
===============

IIFE (Immediately-Invoked Function Expression)
----------------------------------------------

[7]_

.. code:: javascript

    // good (guards against the function becoming an argument when two files with IIFEs are concatenated)
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();

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


Module pattern
--------------

This pattern allows for public and private variables as well as public and
private methods via a function closure. [4]_ 

.. code:: javascript

    var myNamespace = (function () {
     
      var myPrivateVar, myPrivateMethod;
     
      // A private counter variable
      myPrivateVar = 0;
     
      // A private function which logs any arguments
      myPrivateMethod = function( foo ) {
          console.log( foo );
      };
     
      return {
     
        // A public variable
        myPublicVar: "foo",
     
        // A public function utilizing privates
        myPublicFunction: function( bar ) {
     
          // Increment our private counter
          myPrivateVar++;
     
          // Call our private method using bar
          myPrivateMethod( bar );
     
        }
      };
     
    })();

Naming Functions
----------------

Naming your functions results in easier to read stack traces

.. code:: javascript

    var log = function log(msg) {
      console.log(msg);
    };

Method Chaining 
---------------
Jedi.prototype.jump = function() {
    this.jumping = true;
    return this;
};

Jedi.prototype.setHeight = function(height) {
    this.height = height;
    return this;
};

var luke = new Jedi();
luke.jump().setHeight(20);

Feature Detection
-----------------

.. code-block:: javascript

    // Dependency Testing
    // via Quo vadis, JavaScript? Devday.pl keynote by Christian Heilmann
    if ('visibilityState' in document) {
        // Modern browser. Let's load JavaScript
        if ('serviceWorker' in navigator) {
            // Let's add offline support
            navigator.serviceWorker.register('sw.js', {
                scope: './'
            });
        }
    }

Promise
-------

Promise `Anit-patterns <http://taoofcode.net/promise-anti-patterns/>`_

jQuery
------

All jQuery Ajax methods return Deferred objects, and then provides a single callback. [3]_

RxJS
----

JavaScript is multithreaded so don’t need to worry about shared state running through different threads. [9]_

Traversing a stream is semantically equivalent as subscribing to it

ES6
===

method definitions
------------------

example via [8]_

.. code-block:: es5

    var obj = {
        foo: function() {}
    }

.. code-block:: es6

    var obj = {
        foo() {}
    };

computed properties
-------------------

example via [8]_

.. code-block:: es5

    var foo='someName';
    var obj = {};
    obj[foo] = 42;

.. code-block:: es6

    var foo='someName';
    var obj = {
        [foo]: 42
    };

    //method definition using computed properties
    var obj = {
        [foo]() {}
    };

destructuring
-------------

example via [8]_

.. code-block:: es5

    function foo(obj) {
        var username = obj.username;
        var res = obj.res;
    }

.. code-block:: es6

    function foo({username, res}) {}
                

.. [1] http://www.objectplayground.com/
.. [2] http://jsfiddle.net/codepo8/cb7pG/3/light/
.. [3] http://css-tricks.com/multiple-simultaneous-ajax-requests-one-callback-jquery/
.. [4] http://addyosmani.com/resources/essentialjsdesignpatterns/book/#modulepatternjavascript
.. [5] http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html
.. [6] https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108
.. [7] http://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214#7365214
.. [8] http://stackoverflow.com/questions/31382489/es6-hash-array-index-function-call-mixed-syntax
.. [9] RxJS in Action by Paul P. Daniels
