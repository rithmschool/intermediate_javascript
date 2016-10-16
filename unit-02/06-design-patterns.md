#### [⇐ Previous](./05-async-javascript.md) | [Table of Contents](./../readme.md) | [Next ⇒](./07-functional-programming.md)

# Design Patterns In JavaScript

### Objectives:

- Define what a design pattern is in JavaScript
- Compare and contrast creational, structural and behavioral design patterns
- Implement and examine real world use cases for multiple design patterns

### Design Patterns

As programming languages and programming in general has evolved and applications have grown larger and larger, established best practices around designing the structure of our code have been implemented. Patterns are reusable solutions to solve potential issues when building applications. There are many different types of design patterns that can be used to solve a multitude of problems when building software.

When reading about design patterns, you will see the term `class` and `instance` quite frequently. It is important to note that JavaScript does **not** have built in class support, so we use objects and functions to mimic this behavior.

There are three types of design patterns, creational, structural and behavioral

**Creational** - these patterns are focused on object creation. 
**Structural** - these patterns are focused on managing relationships between objects. Structural patterns ensure that when one part of your application changes, it does not effect other parts of your application. 
**Behavioral** - these patterns are focused on managing communication between objects. 

Let's first examine a few creational design patterns:

### Constructor Pattern

The constructor pattern is a creational pattern for making objects and it's one that we have seen before! The constructor pattern makes use of a function called a `constructor` to create objects! Here is a small example of the constructor pattern with the prototype object (which is shared amongst all objects created from this constructor function using the `new` keyword)

```js
function Person(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
}
```

### Module Pattern

Before we examine the module patterns, let's redefine what a "module" is. A module is a self-contained (or encapsulated) and reusable piece of code. The Module Pattern is an alternative to creating a class, and allows us to emulative native functionality in other programming languages. In modules, we can declare public and private variables and methods inside one object, and avoid variable collisions in the global scope. The module pattern makes use of `closure` to create private variables and uses an `IIFE` (Immediately Invoked Function Expression) for an easier syntax when accessing the module. Here is an example of the module pattern

```js
var myModule = (function() {

    // A private variable inside the scope of the IIFE that is 
    var privateVariable = "secret";

    // A private function inside the scope of the IIFE
    function privateFunction() {
        console.log("You just called the private function!");
    };

    // everything returned in the object is accessible publicly  
    return {
        // A public variable
        publicVariable: "You can see me!",

        // A public function utilizing privates
        displayPrivateVariable: function() {
            console.log(privateVariable);
        },
        invokePrivateFunction: function() {
            privateFunction();
        }
    };
})();

myModule.publicVariable // "You can see me!"
myModule.displayPrivateVariable() // "secret"

myModule.privateFunction // undefined
myModule.privateVariable // undefined
```

We can also pass values to our modules from other global modules as parameters. This idea is known as `importing`.

```js
var myModule = (function(j, r) {
    // we now have imported jQuery and React and named them as j and r in our module!    
})(jQuery, React);

```

The module patterns is great way to create private variables and encapsulation in our code. A potential disadvantage is that private variables are not very flexible, which can make testing modules challenging and fixing bugs difficult when there is an issue with private variables and methods.

You can read much more about the module pattern [here](http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html)

### Revealing Module Pattern

A slight variation on the module patterns is the revealing module pattern where public functions are defined before the `return {}` inside of the module and inside the `return {}` we use references to previously defined functions. This makes the code easier to read - let's see an example.

```js

var myRevealingModule = (function(){
    var start = 0;

    function incrementNumberPrivate(){
        start++;
    }
    function incrementNumberPublic(){
        incrementNumberPrivate();
    }
    function initialize(){
        incrementNumberPublic();
    }
    function getPrivateNumber(){
        return start;
    }

    return {
        init: initialize,
        add: incrementNumberPublic,
        status: getPrivateNumber
    }
})()
```

### Singleton Pattern

The Singleton pattern is a bit different than the Constructor and Module pattern even though it is also a Creational pattern. In traditional object oriented languages, the Singleton pattern creates a new instance, only if one does not exist already (Singleton => "single instantiation"). If the instance exists, the patterns returns a reference to the instance. Therefore you can only create a single instance, or in JavaScript a single object from this pattern. The Singleton pattern is used heavily in front end frameworks like Angular.

Let's see an example:

```js

var firstSingleton = (function(){
    
    var instance;

    // this code will be executed ONCE
    function initialize(){
        var count = 0;
        var privateName = "Samuel Singleton";
        
        function privateMethod(){
            return "Your name is " + privateName;
        }

        return {
            sayMyName: privateMethod,
            increment: function(){
                count++;
                return count;
            },
            getCount: function(){
                return count;
            }
        }
    }

    // let's see if we need to call the initialize function

    return {
        findOrCreateInstance: function(){
            if(!instance){
                instance = initialize();
            }

            return instance; 
        }
    }
})()

var one = firstSingleton.findOrCreateInstance();
var two = firstSingleton.findOrCreateInstance();

one === two // true

one.increment(); // 1
one.increment(); // 2
one.increment(); // 3
two.getCount(); // 3

``` 

The Singleton pattern is very useful when you only want one instance possibly created, but it can be difficult to test and when used too heavily throughout a codebase, it can actually be a sign of poor design.

### Facade Pattern

The Facade pattern is used quite heavily in jQuery and involves presenting an outward appearance that hides underlying complexity. The idea is that using this pattern, we can provide a simple looking API and obscure the complexity from others. Some examples of the Facade pattern in jQuery are `.css()`,`.animate()` and other abstractions like `.getJSON()`, `.get(),` and `.post()`. Take a look at **one** of the potential implementations of `$(document).ready` [here](https://github.com/jquery/jquery/blob/f18ca7bfe0f5e3184bf1ed55daf1668702c5577a/src/core/ready-no-deferred.js) and you can see just how much complexity is hidden from what appears to be a simple method.

Let's look at a simple example of the facade pattern from Addy Osmani's essential design patterns :

```js
var module = (function() {
 
    var _private = {
        i: 5,
        get: function() {
            console.log( "current value:" + this.i);
        },
        set: function( val ) {
            this.i = val;
        },
        run: function() {
            console.log( "running" );
        },
        jump: function(){
            console.log( "jumping" );
        }
    };
 
    return {
 
        facade: function( args ) {
            _private.set(args.val);
            _private.get();
            if ( args.run ) {
                _private.run();
            }
        }
    };
}());
 
 
// Outputs: "current value: 10" and "running"
module.facade( {run: true, val: 10} );
```

We can see here that we are abstracting quite a bit of detail when the facade function is called. When you find yourself using a library or framework and using many predetermined values (especially for keys in objects), you very well may be using a facade! 

### Observer Pattern

The Observer pattern  is where on object (the subject), has a list of objects that depend on it (observers/subscribers) and automatically notifies the observers when there are any changes. The subject has the ability to subscribe and unsubscribe objects from the list as well. There is also a pattern called pub/sub (Publish/Subscribe), which is very similar to the Observer Pattern except in a pub/sub model there is a additional data that sits between the subject and observer. 

You can find some great examples of the observer pattern [here](https://addyosmani.com/resources/essentialjsdesignpatterns/book/#observerpatternjavascript)

 The Observer pattern is quite common in popular JavaScript libraries like RxJS and Redux. You have also already seen many examples of the Observer pattern with jQuery methods like `on()` and `off()` to subscribe and unsubscribe to events.

### Additional Resources

There are quite a few design patterns that we have not seen, you can learn more about them with these resources. 

[https://scotch.io/bar-talk/4-javascript-design-patterns-you-should-know](https://scotch.io/bar-talk/4-javascript-design-patterns-you-should-know)

[http://code.tutsplus.com/tutorials/understanding-design-patterns-in-javascript--net-25930](http://code.tutsplus.com/tutorials/understanding-design-patterns-in-javascript--net-25930)

[https://addyosmani.com/resources/essentialjsdesignpatterns/book/](https://addyosmani.com/resources/essentialjsdesignpatterns/book/)

#### [⇐ Previous](./05-async-javascript.md) | [Table of Contents](./../readme.md) | [Next ⇒](./07-functional-programming.md)