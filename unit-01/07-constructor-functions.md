#### [⇐ Previous](./06-introduction-to-oop.md) | [Table of Contents](./../readme.md) | [Next ⇒](./08-prototypes.md)

# Constructor Functions

### Objectives:

By the end of this chapter, you should be able to:

- Explain what constructor functions are and why they are used
- Create constructor functions with proper syntax and convention
- Use the `new` keyword to create objects from a constructor function
- Use `call` and `apply` inside of a constructor function 

### The meaning / purpose of a constructor function

Let's imagine that we are tasked with building an application that requires us to create `car` objects. Each car that we create should have a make, model and year. So we get started by doing something like this....

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

But notice how much duplication is going on! All of these objects look the same, yet we are repeating ourselves over and over again. It would be really nice to have a **blueprint** that we could work off of to reduce the amount of code that we have. That "blueprint" is exactly what a _constructor function_ provides!

### Our first constructor function

So what is a constructor function? It's written just like any other function, except that by convention we capitalize the name of the function to denote that it is a constructor. We call these functions constructors because their job is to construct objects. Here is what a constructor function to create car objects might look like. Notice the capitalization of the `name` of the function - this is **best** practice when creating constructor functions so that other people know what kind of function it is.

```javascript
function Car(make, model, year){
    this.make = make;
    this.model = model;
    this.year = year;
}
```

So how do constructor functions actually "construct" these objects? Through the `new` keyword that we briefly saw before. Let's quickly refresh our memory about what the `new` keyword does:

In fact we have been using the `new` keyword implicitly for some time (though in these cases, it's best not to use it explicitly):

```javascript
var arr = [] // same as var arr = new Array
var obj = {} // same as var obj = new Object
```

When the `new` keyword is used:

1. An empty object is created,
3. The keyword `this` inside of the constructor function refers to the empty object that was just created,
3. A `return this` is added to the constructor function (this is why you don't need to explicitly return any value),
4. An internal link is created between the object and the `.prototype` property on the constructor function. We can actually access this link, it is called `__proto__`, sometimes called "dunder" (double underscore) proto.

```javascript
function Person(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
}

var steve = new Person("Steve", "Perry");
steve.__proto__ === Person.prototype // true
```

So what's a prototype? Great question! We'll get to that in the next chapter.

### Classes

Many other programming languages have a concept of classes. A class is what is used to construct objects (which are commonly called `instances`). In JavaScript we **DO NOT** have classes built into the language. You may see that the newest version of the language has the keyword `class`, but that is just an abstraction of constructor functions and prototype properties and methods (which we will discuss in the next section). 

### The constructor property

Every single `.prototype` object has a property called `constructor` that points back to the original function. So in our example above:

```javascript
Person.prototype.constructor === Person // true
```

We will see how this can be changed in the next chapter (and we will have to reset it back to an original value).

### Using call/apply with constructors

If we write constructor functions that are identical to other ones, we can use `call` or `apply` to invoke other constructor functions and specify a different value for the keyword `this`. Here is an example:

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

Complete the [Constructor Functions Exercise](https://github.com/rithmschool/intermediate_js_exercises/tree/master/constructor_functions_exercise)

### Additional Resources

#### [⇐ Previous](./05-call-apply-bind.md) | [Table of Contents](./../readme.md) | [Next ⇒](./07-intermediate-oop.md)