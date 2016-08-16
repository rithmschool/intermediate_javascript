#### [⇐ Previous](./03-call-apply-bind.md) | [Table of Contents](./../readme.md) | [Next ⇒](./05-prototypes.md)

# Constructor Functions

### Objectives:

By the end of this chapter - you should be able to

- Create constructor functions with proper syntax and convention
- Use the new keyword to create objects from a constructor function
- Attach properties to a constructor function

### The meaning / purpose of a constructor function:

Let's imagine that we are tasked with building an application that requires us to create `car` objects. Each car that we make, should have a make, model and year. So we get started by doing something like this....

```javascript

var car1 = {
    make: "Honda",
    model: "Accord",
    year: 2002
}
var car2 = {
    make: "Mazda",
    model: "6",
    year: 2008
}
var car3 = {
    make: "BMW",
    model: "7 Series",
    year: 2012
}
var car4 = {
    make: "Tesla",
    model: "Model X",
    year: 2016
}
```

But notice how much duplication is going on! All of these objects look the same, yet we are repeating ourselves over and over again. It would be really nice to have a **blueprint** that we could work off of to reduce the amount of code that we have. That "blueprint" is exactly what our constructor function is!

### Our first constructor function

So what is a constructor function? It's written just like any other function, except we capitalize the name of the function to denote that it is a constructor. We call these functions constructors because their job is to construct objects. Here is what a constructor function to create car objects might look like. Notice the capitalization of the `name` of the function - this is **best** practice when creating constructor functions so that other people know what kind of function it is.

```javascript
function Car(make, model, year){
    this.make = make;
    this.model = model;
    this.year = year;
}
```

So how do constructor functions actually "construct" these objects? Through the `new` keyword that we briefly saw before. Let's quickly refresh our memory about what the `new` keyword does:

In fact we have seen the `new` keyword in the while quite a bit, even if you don't (and should not) use it!

```javascript
var arr = [] // same as var arr = new Array
var obj = {} // same as var obj = new Object
```

When the `new` keyword is used:

1. An empty object is created
2. The keyword `this` inside of the constructor function refers to the empty object that was just created
1. A `return this` is added to the constructor function
2. An internal link is created between the object and the `.prototype` property on the constructor function and we can actually access this link, it is called `__proto__` sometimes called "dunder" (double underscore) proto.

```javascript
function Person(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
}

var steve = new Person("Steve", "Perry");
steve.__proto__ === Person.prototype // true
```

### Classes

In many other programming languages - there is something that exists called a `class`. A class is what is used to construct objects (which are comomonly called `instances`). In JavaScript we **DO NOT** have classes built into the language. You may see that the newest version of the language has the keyword `class`, but that is just an abstraction of constructor functions and prototype properties and methods (which we will discuss in the next section). 

### The constructor property

Every single `.prototype` object has a property called `constructor` that points back to the original function. So in our example above:

```javascript
Person.prototype.constructor === Person // true
```

We will see how this can be changed in the next unit (and we will have to reset it back to an original value)

### Using call/apply with constructors

If we write constructor functions that are identical to other ones ,we can use `call` or `apply` to invoke other constructor functions and specify a different value for the keyword `this`. Here is an example:

```javascript
function Vehicle(make,model,year){
    this.make = make;
    this.model = model;
    this.year = year;
}

function Car(make,model,year){
    Vehicle.apply(this,arguments)
}

function Motorcycle(make,model,year,maxSpace){
    Vehicle.call(this,make,model,year)
    this.maxSpace = maxSpace
}
```

### Exercise

Complete the [Constructor Functions Exercise](https://github.com/rithmschool/prework_exercises/tree/master/constructor_functions_exercise)

### Additional Resources

#### [⇐ Previous](./05-call-apply-bind.md) | [Table of Contents](./../readme.md) | [Next ⇒](./07-prototypes.md)