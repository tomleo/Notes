# Language
use strict mode in javascript
## Type

typeof will always return "object" for native non callable objects.

``` javascript
>> [typeof null, null instanceof Object]
["object", false]
```

the + operator has higher precedence than the ternary one

``` javascript
>> var val = 'smtg';
>> console.log('Value is ' + (val === 'smtg') ? 'Something' : 'Nothing');
Something 
```

The + sign concatenates strings, the - sign causes a type cast

``` javascript
>> '5' + 3
"53"
>> '5' - 3
2
```

### Comparison

[^1]

``` javascript
// Objects evaluate to true
if( {} ) {
    console.log("Really?"); // => Really?
} 

Boolean([]) => true
Boolean({}) => true
Boolean(null, NaN, 0, '', undefined) => false
```

## Scope

The var declaration is hoisted to the function scope, but the
initialization is not.

``` javascript
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
```

Local vs. Global Functions, y is an automatic global, not a function
local one like x

``` javascript
(function(){
  var x = y = 1;
})();
console.log(y);
console.log(x);
1
RefereceError: x is not defined
```

## Hoisting

Variable declarations get hoisted to the top of their scope, their
assignment does not [^2]

``` javascript
function example() {
    console.log(declaredButNotAssigned); // => undefined
    var declaredButNotAssigned = true;
}

function example() {
    var declaredButNotAssigned;
    console.log(declaredButNotAssigned); // => undefined
    declaredButNotAssigned = true;
}
```

Anonymouse function expressions hoist their variable name, but not the
function assignment

``` javascript
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
```

Function declarations hoist their name and function body

``` javascript
function example() {
  superPower(); // => Flying

  function superPower() {
    console.log('Flying');
  }
}
```

## Switch Statement

switch uses === internally and new String(x) !== x

``` javascript
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
```

## How to define functions and objects in JS

To create an object you can use Object.create function or simply write
the function explicitly like below.

``` javascript
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
```

``` console
>>> this.object1
Object { val=42, get=myMethod()}
>>> this.object2
Object { val=3.14159, get=myMethod()}
>>> this.object3
Object { val=42}
```

It should be noted that name is a read only property for example

``` javascript
function foo() { }
var oldName = foo.name;
foo.name = "bar";
[oldName, foo.name]
["foo", "foo"]
```

## Prototypal Inheritance

When looking for attributes JS will go up the inheritance chain until it
finds what it is looking for. The Object.create(obj) is a way to create
an object based on obj prototype.

``` javascript
var parent = {
    get: function fn() {
        return this.val;
    },
    val: 42
};
var child = Object.create(parent);
child.val = 25;

var grandchild = Object.create(child);
```

``` console
>>> parent.get();
42 //parent.get() -> fn() which looks for -> this.val=42

>>> child.get();
25 //child.get() does not exist so looks in parent for get -> parent.get()
   //which looks for this.val=25

>>> grandchild.get();
25 //grandchild.get() does not exist so looks in child for get -> child.get()
   //does not exist so looks in parent for get -> parent.get() which looks for
   //this.val which does not exist so looks for child.get=25
```

The following code example evaluates to false

``` javascript
function f() {}
var a = f.prototype, b = Object.getPrototypeOf(f);
a === b
```

This is because f.prototype is the object that will become the parent of
any objects created with new f while Object.getPrototypeOf returns the
parent in the inheritance hierarchy.

## Polymorphism

``` javascript
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
```

``` console
>>> answer.get();
42
>>> firmAnswer.get();
"42!!"
```

## Getting the this right or... getting the right this

``` javascript
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
```

``` console
>>> helloWorld.get()
"Hello World"
>>> goodByeWorld.get()
"Goodbye World"
```

``` javascript
// variables referencing "this" should be named _this
function() {
  var _this = this;
  return function() {
    console.log(_this);
  };
}
```

## Using Constructors

Instead of defining a class' variables you can pass variables to a
constructor.

``` javascript
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
```

``` console
var hello = Object.create(helloPrototype);
helloWorld.constructor("Hello");
helloWorld.get();

var helloWorld = Object.create(helloWorldPrototype);
helloWorld.constructor("Hello");
helloWorld.get();
```

## Arrays

``` javascript
var x = [1,2,3,]; //ERROR
var y = {'a': 1, 'b': 2, 'c': 3,}; //ERROR
```

If, when defining an array or an object, you leave a trailing comma
after the last item in your collection, IE will fail to parse your
javascript file:
# Modules
## Asynchronous Definition

``` note
Default implies optional
```

`define(id?, dependencies?, factory);`

id: id of the module being defined (default: to the id of the module that the loader was requesting)

dependencies: an array of the dependencies that are required by the module that
is being defined

```
resolved prior to execution of the module factory function, and the resolved
values should be passed as arguments to the factory function with argument
positions corresponding to index in the dependencies array. The dependencies
ids may be relative ids, and should be resolved relative the module being
defined. 

If the value of "require", "exports", or "module" appear in the dependency
list, the argument should be resolved to the corresponding free variable as
defined by the CommonJS modules specification. This argument is optional. If
omitted, it should default to ["require", "exports", "module"].
```
[^12]

## ES6 Modules

```
with cyclic dependencies, you can’t access imports in the body of the module. 
```
[^13]

ECMAScript 6 provides the following ways of importing [4]:

Imports:

``` javascript
// Default exports and named exports
import theDefault, { named1, named2 } from 'src/mylib';
import theDefault from 'src/mylib';
import { named1, named2 } from 'src/mylib';

// Renaming: import named1 as myNamed1
import { named1 as myNamed1, named2 } from 'src/mylib';

// Importing the module as an object
// (with one property per named export)
import * as mylib from 'src/mylib';

// Only load the module, don’t import anything
import 'src/mylib';
```

Exports:
``` javascript
export var myVar1 = ...;
export let myVar2 = ...;
export const MY_CONST = ...;

export function myFunc() {
    ...
}
export function* myGeneratorFunc() {
    ...
}
export class MyClass {
    ...
}
```


```
you can list everything you want to export at the end of the module
(which is once again similar in style to the revealing module
pattern).

    const MY_CONST = ...;
    function myFunc() {
        ...
    }
    
    export { MY_CONST, myFunc };

You can also export things under different names:

    export { MY_CONST as THE_CONST, myFunc as theFunc };

```

```
System.import() enables you to:

Use modules inside <script> elements (where module syntax is not supported, consult Sect. “Further information” for details).
Load modules conditionally.
System.import() retrieves a single module, you can use Promise.all() to import several modules:

    Promise.all(
        ['module1', 'module2', 'module3']
        .map(x => System.import(x)))
    .then(([module1, module2, module3]) => {
        // Use module1, module2, module3
    });
```


# Nodejs

## Node virtual machine

https://github.com/creationix/nvm

> **note**
>
> in practice, I've found nvm confusing to use. I docker container may
> be a better way to go

Left off at 15:00 [^3]

## Installing Node

curl -sL <https://deb.nodesource.com/setup> | sudo bash -sudo apt-get
install -y nodejs

## IO

All synchronous (or blocking) filesystem methods in the fs module end
with 'Sync'.

``` javascript
// return a Buffer object containing the complete contents of the file
fs.readFileSync('/path/to/file')
```

Buffer objects are Node's way of efficiently representing arbitrary
arrays of data, whether it be ascii, binary or some other format. Buffer
objects can be converted to strings by simply calling the toString()
method on them.

``` javascript
var str = buf.toString().
```



# Design Patterns
## IIFE (Immediately-Invoked Function Expression)

[^4]

``` javascript
// good (guards against the function becoming an argument when two files with IIFEs are concatenated)
;(function() {
  var name = 'Skywalker';
  return name;
})();
```

## Shifting of classes

If you are going to be adding/removing classes of an element you should
cache them.

``` javascript
var navright = document.querySelector('#right');
var navleft = document.querySelector('#left');
var navup = document.querySelector('#up');
var navdown = document.querySelector('#down');
```

Furthermore if you are going to be adding a class to indicate some
styling state of an element you should do the following [^5]

``` javascript
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
```

## Module pattern

This pattern allows for public and private variables as well as public
and private methods via a function closure. [^6]

``` javascript
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
```

## Naming Functions

Naming your functions results in easier to read stack traces

``` javascript
var log = function log(msg) {
  console.log(msg);
};
```

## Method Chaining

``` javascript
Jedi.prototype.jump = function() {

:   this.jumping = true; return this;

};

Jedi.prototype.setHeight = function(height) {

:   this.height = height; return this;

};

var luke = new Jedi(); luke.jump().setHeight(20);
```

## Feature Detection


``` javascript
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
```

## Promise

Promise [Anit-patterns](http://taoofcode.net/promise-anti-patterns/)

## jQuery

All jQuery Ajax methods return Deferred objects, and then provides a
single callback. [^7]

## RxJS

JavaScript is multithreaded so don’t need to worry about shared state
running through different threads. [^8]

Traversing a stream is semantically equivalent as subscribing to it

# ES6
## method definitions

example via [^9]

``` es5
var obj = {
    foo: function() {}
}
```

``` es6
var obj = {
    foo() {}
};
```

## computed properties

example via [^10]

``` es5
var foo='someName';
var obj = {};
obj[foo] = 42;
```

``` es6
var foo='someName';
var obj = {
    [foo]: 42
};

//method definition using computed properties
var obj = {
    [foo]() {}
};
```

## destructuring

example via [^11]

``` es5
function foo(obj) {
    var username = obj.username;
    var res = obj.res;
}
```

``` es6
function foo({username, res}) {}
```

## Method Definitions

I sometimes unintentionally write JS like this

``` javascript

object_name {
    method_name(arg) {}
}

```

instead of 

``` javascript

object_name {
    method_name: function(arg) {}
}

```

Here's an example of this in action http://codepen.io/tomleo/pen/QEVwBz

The issue with this "syntax error" is that it's actually
[valid ES6 syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Method_definitions)
and won't throw an error when developing in Chrome or Firefox. However
it's not supported in IE or Safari. It also causes yuglify to choke
during deployment.

JavaScript linting tools like eslint are handy in catching these issues.










# Backbone.js


## Models

### Class Methods
> Backbone accepts class methods as the second argument when extending
> any of its classes -- []()

``` javascript
var Car = Backbone.Model.extend({
  // instance methods

}, {
  // class methods

  manufacturersByCountry: function(country) {
    // custom logic to query manufacturers
  }
});
```

### Overriding core methods

Similar to calling `super` of a method in python:

``` javascript
var Car = Backbone.Model.extend({
    initialize: function() {
        // my custom stuff
        // ..
        // ..

        Backbone.View.prototype.initialize.call(this);
    }
});
```

This will copy the sync functionality with the current context:

``` javascript
sync: function () {
  return Backbone.sync.apply(this, arguments);
}
```

Actually calls the parent/prototype method:

``` javascript
var Note = Backbone.Model.extend({
  set: function (attributes, options) {
    Backbone.Model.prototype.set.apply(this, arguments);
    ...
  }
});
```

### Listen to events from Model

``` es5
Backbone.Model.extend({
    foo: function(bar) {
        this.save({
            a: 1,
            b: 2
        });
        return {'hello': 'world'};
    }
});

Backbone.Marionette.LayoutView.extend({
    initialize: function() {
        this.listenTo(this.model, 'foo', this.handleFoo);
    },
    handleFoo: function(obj) {
        console.log(obj) // {'hello': 'world'}
    }
});
```


## Views

> During runtime, the Backbone.View's constructor calls initialize and
> expects the user to implement some logic there to eventually render
> the view. More on constructors later. -- [](http://chrisawren.com/posts/Learning-Advanced-JavaScript-via-the-Backbone-js-source-code)


> Avoid calling the render method for every single model that is being added
> `this.listenTo(this.collection, ‘add’, _.debounce(_.bind(this.render), 128))`
> -- ()[https://www.toptal.com/backbone-js/top-8-common-backbone-js-developer-mistakes]

### Rendering

To create elements not attached to the DOM:

``` javascript
Backbone.View.extend({
    tagName: 'li',
});
```

To create elements with container already in the DOM:

``` javascript
Backbone.View.extend({
    el: '#propertyList',
});
```
### setElement

`setElement` will create a cached `$el` reference for you, moving the
delegated events for a view from the old element to the new one.

``` javascript
var button1 = $('<button></button>');
var button2 = $('<button></button>');
var view = new View({el: button1});
view.setElement(button2);
```

More notes located in hacking/learning-backbone/index.html




## Plugins
    
- [rendrjs/rendr](https://github.com/rendrjs/rendr): Render your Backbone.js apps on the client and the server, using Node.js
- [rotundasoftware/backbone.subviews](https://github.com/rotundasoftware/backbone.subviews): minimalist view mixin for creating and managing subviews
- []()

## Patterns



# Ref
[^1]: <https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108>

[^2]: <http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html>

[^3]: <http://www.objectplayground.com/>

[^4]: <http://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214#7365214>

[^5]: <http://jsfiddle.net/codepo8/cb7pG/3/light/>

[^6]: <http://addyosmani.com/resources/essentialjsdesignpatterns/book/#modulepatternjavascript>

[^7]: <http://css-tricks.com/multiple-simultaneous-ajax-requests-one-callback-jquery/>

[^8]: RxJS in Action by Paul P. Daniels

[^9]: <http://stackoverflow.com/questions/31382489/es6-hash-array-index-function-call-mixed-syntax>

[^10]: <http://stackoverflow.com/questions/31382489/es6-hash-array-index-function-call-mixed-syntax>

[^11]: <http://stackoverflow.com/questions/31382489/es6-hash-array-index-function-call-mixed-syntax>

[^12]: <http://wiki.commonjs.org/wiki/Modules/AsynchronousDefinition>

[^13]: <http://www.2ality.com/2014/09/es6-modules-final.html>
