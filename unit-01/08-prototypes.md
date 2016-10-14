#### [⇐ Previous](./07-constructor-functions.md) | [Table of Contents](./../readme.md) | [Next ⇒](./09-intermediate-oop.md)

# Prototypes

### Objectives

By the end of this chapter, you should be able to:

- Define what an object's prototype is
- Explain what effect using the `new` keyword has on an object's prototype
- Explain why functions should be added to the prototype instead of the constructor function
- Create a "class" in javascript that inherits from a parent "class"

### Prototype Intro


Every single object that is created in JavaScript has a `prototype` property.  Let's start by looking at `Object.prototype`.  In the chrome console, try `Object.prototype` then expand the object you get back.  You can see that `Object` already has many properties on it's prototype.

When you create a constructor function, that function will have it's own prototype. Let's try that out by creating a `Person` constructor function:

```js
function Person(name) {
   this.name = name;
}

var tim = new Person("Tim");

Person.prototype
```

So far, our `Person` constructor function has a prototype and the only two properties available on the prototype should be `constructor` and `__proto__`.  Let's try adding a function to the Person prototype:

```js
Person.prototype.sayHello = function() {
	return "Hello, " + this.name;
};
```

Now that we have added sayHello to the prototype of `Person`, any person object that will be create or that _was_ created in the past has access to the function:

```js
var moxie = new Person("Moxie");
moxie.sayHello();  // returns "Hello, Moxie"

// Notice that sayHello still works for tim even though tim was created
// before the sayHello function was added to the prototype.
tim.sayHello();    // returns "Hello, Tim"
```

So the main things to know so far about an Object's prototype are the following:

1. Any function or property added to the prototype is shared among all instances of the Object (For example, sayHello is __shared__ among all `Person` instances).
2. Each constructor function has its own prototype.

### Shared Prototype Example

Let's look at another example of adding properties to a prototype.


```javascript
function Person(name){
    this.name = name;
}

Person.prototype.siblings = ["Haim", "David"];

var elie = new Person("Elie");
```

The above example creates a instance of a `Person` and sets a siblings array on the prototype.  The intention is for `elie` to have an array of siblings.  However, since the prototype is shared among __all__ instances of a `Person`, any other instance will also have access to the same siblings array:

```js
// The siblings array will now be ["Haim", "David", "Tamar"]
elie.siblings.push("Tamar"); // returns the new length of the array => 3

var anotherPerson = new Person("Mary");

anotherPerson.siblings.push("Leslie");
elie.siblings; // ["Haim", "David", "Tamar", "Leslie"]
```

We can see again from this example, that anything put on the prototype is __shared__ among all instances of that object.

### Constructor Function Best Practices

The best practices for creating constructor functions in JavaScript are:

1.  All of the properties that you do not want to be shared go inside of the constructor function
2. All properties that you want to be shared go on the prototype.  Almost all of the time, you will want to put functions on the prototype.  We will explain why soon!

Using our person example, if we want to add a siblings array to the `Person` class, we would add it in the constructor:

```js
function Person(name) {
	this.name = name;
	this.siblings = [];
}

var janey = new Person("Janey");
janey.silbings.push("Annie");
```

Now each time the `new` keyword is used on the `Person` constructor, a new object is create that has its own name and siblings property.  Now if we create another person it will have its own name and siblings array as well:

```js
var tim = new Person("Tim");
tim.siblings.push("Nicole");
tim.siblings.push("Jeff");
tim.siblings.push("Greg");
tim.siblings; // Returns ["Nicole", "Jeff", "Greg"];
```

So why do we want to add functions to the prototype?  Your code will function correctly if you create your function definitions in the constructor like this:

```js
// NOT A GOOD PRACTICE
function Person(name) {
   this.name = name;
   this.sayHi = function() {
       return "Hello, " + this.name;
   }
```

The problem is that every time you use the `new` keyword to create a `Person`, a new object gets created in memory that allocates space for the person's name and also for the `sayHi` function.  So if we have 10 `Person` objects that we create, there will be 10 copies of the same `sayHi` function.  Since the function does not need to be unique per `Person` instance, it is better to add the function to the prototype, like this:

```js
// BEST PRACTICE!!
function Person(name) {
   this.name = name;
}

Person.prototype.sayHi = function() {
    return "Hello, " + this.name;
}
```

Unless you have a good reason not to, __always put function definitions on the constructor function's prototype__.

### JavaScript Property Lookup

When you attempt to access a property on an object in JavaScript, there is a lookup process that goes on in order to find your property.  To find the value for a property, first the properties on the object are checked.  If the property is not found, then the properties on the prototype of the constructor function are checked. Let's look at an example:

```js
function Automobile(make, model, year) {
	this.make = make;
	this.model = model;
	if (year !== undefined) {
		this.year = year;
	}
}

Automobile.prototype.year = 2016;
```

Notice that year is set on the prototype to 2016.  Also, if no year is passed into the constructor, an assignment to year will not be made.

```js
var newCar = new Automobile("Ferrari", "488 Spider");

// In this case, we did not pass in a year,
// so it was never set in the constructor function
newCar.hasOwnProperty("year"); // Returns false

newCar.year; // returns 2016
```

Now, if we create a car with a year, the property on the car object will be seen first in the property lookup:

```js
var probe = new Automobile("Ford", "Probe", 1993);

probe.hasOwnProperty("year"); // returns true

probe.year; // returns 1993
```

### Prototype Inheritance

Looking at our example again, we can see how our `Automobile` constructor function inherits properties from the Object constructor function:

```js
function Automobile(make, model, year) {
	this.make = make;
	this.model = model;
	if (year !== undefined) {
		this.year = year;
	}
}

Automobile.prototype.year = 2016;
var probe = new Automobile("Ford", "Probe", 1993);

probe.hasOwnProperty("year"); // returns true

probe.year; // returns 1993
```
Where did the function `hasOwnProperty` come from?  It is defined in the `Object` prototype.  Since all objects in JavaScript inherit from the Object prototype, your `Automobile` object has access to the `hasOwnProperty` function through the prototype from `Object`.

Let's investigate the prototype chain for automobile using `__proto__` and `console.dir`:

```js
var probe = new Automobile("Ford", "Probe", 1993);

// Inspect the returned object in the terminal
// It shows us the prototype associated with the instance of Automobile
// You should see the constructor function and a property for year
probe.__proto__;

// Inspect the returned object in the terminal
// It shows us the parent prototype (Object's prototype) that is associated
// with the instance of Automobile
// You should see many properties here, including hasOwnProperty!
probe.__proto__.__proto__;

// Click through the returned object to see the __proto__ chain.
console.dir(probe);
```

### Creating Your Own Inheritance Chain

An important concept in object oriented programming is inheritance. The idea behind inheritance is that one or more parent / super classes can pass along functions and properties to other child / sub classes. 

```javascript
function Parent(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
}

Parent.prototype.sayHi = function(){
    return this.firstName + " " + this.lastName + " says hi!";
}

function Child(firstName, lastName){
    // This is how we "inherit" properties from the parent
    Parent.apply(this,arguments);
}

// This is how we inherit functions
// (create a new prototype + reset the constructor)
Child.prototype = Object.create(Parent.prototype);
Child.prototype.constructor = Child;

var c = new Child("Bran", "Stark");

c.sayHi() // Bran Stark says hi!
```

So what have we done here? We've set the prototype of the `Child` to be a newly created object with a prototype of `Parent.prototype` (`Object.create` accepts as a parameter another object to set as the `prototype`).

### What does Object.create do?

Why can't we just do `Child.prototype = Parent.prototype`? Remember that when we assign objects equal to each other, they are just references. This means that `Child.prototype` is a reference to `Parent.prototype` which means that if we add to the `Child.prototype`, objects created from the `Parent.prototype` will have access to them, which would be bad!

```javascript
Child.prototype = Parent.prototype;

// true - this is BAD!
Child.prototype === Parent.prototype;

Child.prototype = Object.create(Parent.prototype);

// false - This is GOOD! We want these to be different
Child.prototype === Parent.prototype;
```

### What about resetting the constructor?

Let's examine the last line: `Child.prototype.constructor = Child;`. Without this line, if you examine `Child.prototype.constructor`, this will refer to the `Parent`, and not the `Child`! In many cases this won't actually matter, but it can definitly be confusing, since If you call `.prototype.constructor` on a constructor function, you expect it to point back to the original constructor function. The details here aren't that important for right now, but if you are interested in learning more, check out [this](http://stackoverflow.com/questions/8453887/why-is-it-necessary-to-set-the-prototype-constructor) Stack Overflow article.

### BAD PRACTICE: Using new to create a child class

You may see inheritance done by using the `new` keyword instead of using `Object.create`. This will do almost the same thing, but add additional unnecessary properties on the prototype (since it is creating an object with undefined properties just for the prototype). For more on this, check out [this](http://stackoverflow.com/questions/13040684/javascript-inheritance-object-create-vs-new) Stack Overflow question.

### Exercise

Complete the [Prototypes Exercise](https://github.com/rithmschool/intermediate_js_exercises/tree/master/prototypes_exercise)

### Additional Resources

[http://www.bennadel.com/blog/2184-object-create-improves-constructor-based-inheritance-in-javascript---it-doesn-t-replace-it.htm](http://www.bennadel.com/blog/2184-object-create-improves-constructor-based-inheritance-in-javascript---it-doesn-t-replace-it.htm)

#### [⇐ Previous](./07-constructor-functions.md) | [Table of Contents](./../readme.md) | [Next ⇒](./09-intermediate-oop.md)