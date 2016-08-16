#### [⇐ Previous](./05-constructor-functions.md) | [Table of Contents](./../readme.md) | [Next ⇒](./06-es2015.md)

# Prototypes

### Objectives:

By the end of this chapter - you should be able to

- Define what an object's prototype is
- Explain what effect using the `new` keyword has on an object's prototype
- List the reasons why adding functions to a prototype is more common than adding to a constructor

Every single object that is created in JavaScript has a `prototype` property. The prototype property can contain properties and functions that can be shared amongst any objects that contain that same `prototype` property. So how do objects "contain" a `prototype` property? Remember when we used the `new` keyword? One of the things it did, was create a link between the object created (From the new keyword) and the `prototype` property of whatever function created it!

Let's see this in action:

```javascript
function Person(name){
    this.name = name;
}

var elie = new Person("Elie")

console.dir(elie) // console.dir is a great way to further examine objects

elie.__proto__ === Person.prototype // true
Person.prototype.constructor === Person // true
```

```javascript
function Person(name){
    this.name = name;
}

var elie = new Person("Elie")

Person.prototype.favoriteColor = "purple"

elie.favoriteColor // purple

var matt = new Person("Matt")

matt.favoriteColor // purple

Person.prototype.sayHi = function(){
    return this.name + " says hi!"
}

elie.sayHi() // Elie says hi!
```

What we see from this example is that even after we create objects using the `new` keyword, we can later on add functions/properties to the prototype object of the constructor function - and those objects can use it! What's also important to note here is that properties/functions that exist on the prototype object are **shared** amongst all of the objects that have access to ti. Here is an example

```javascript
function Person(name){
    this.name = name;
}

Person.prototype.siblings = ["Haim", "David"]

var elie = new Person("Elie")

elie.siblings.push("Tamar") // returns the new length of the array => 3

var anotherPerson = new Person("Bob")

anotherPerson.siblings.push("James") // returns the new length of the array => 4

elie.siblings // ["Haim", "David", "Tamar", "James"]
```

Notice here that the `anotherPerson` modified something that `elie` could access as well! That is because properties on the prototype are **shared** amongst all objects that are created using the `new` keyword and a constructor function. 

### Inheritance

In Object Oriented Programming, one of the essential principles is inheritance. The idea behind inheritance is that one (or more in some other languages) or more (parent / super) classes can pass along functions and properties to other (child / sub) classes. 

```javascript
function Parent(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
}

Parent.prototype.sayHi = function(){
    return this.firstName + " " + this.lastName + " says hi!" 
}

Parent.prototype.sayBye = function(){
    return this.firstName + " " + this.lastName + " says bye" 
}

function Child(firstName, lastName){
    // this is NOT inheritance
    Parent.apply(this,arguments);
}

// this IS inheritance (create a new prototype + reset the constructor)
Child.prototype = Object.create(Parent.prototype);
Child.prototype.constructor = Child;

var c = new Child("Bran", "Stark");

c.sayHi() // Bran Stark says hi!
```

So what have we done here? We've set the prototype of the Child to be a newly created object with a prototype of `Parent.prototype` (`Object.create` accepts as a parameter another object to set as the `prototype`).

You may see inheritance done by using the `new` keyword instead of using `Object.create`. This will do almost the same thing, but add additional unnecessary properties on the prototype (since it is creating an object with undefined properties just for the prototype)

### Why do we need Object.create?

Why can't we just do `Child.prototype = Parent.prototype`? Remember that when we assign objects equal to each other, they simply are just references. This means that `Child.prototype` is a reference to `Parent.prototype` which means that if we add to the `Child.prototype`, objects created from the `Parent.prototype` will have access to them, which would be bad!

```javascript
Child.prototype = Parent.prototype;
Child.prototype === Parent.prototype; // true - this is BAD!
Child.prototype = Object.create(Parent.prototype);
Child.prototype === Parent.prototype; // false - we want these to be different
```

### What about resetting the constructor?

The next line `Child.prototype.constructor = Child;` not nearly as important as the one above, but if you are interested in learning why this is preferred - you can read more [here](http://stackoverflow.com/questions/8453887/why-is-it-necessary-to-set-the-prototype-constructor)

### Exercise

Complete the [Prototypes Exercise](https://github.com/rithmschool/prework_exercises/tree/master/prototypes_exercise)

### Additional Resources

[http://www.bennadel.com/blog/2184-object-create-improves-constructor-based-inheritance-in-javascript---it-doesn-t-replace-it.htm](http://www.bennadel.com/blog/2184-object-create-improves-constructor-based-inheritance-in-javascript---it-doesn-t-replace-it.htm)

#### [⇐ Previous](./06-constructor-functions.md) | [Table of Contents](./../readme.md) | [Next ⇒](./08-es2015.md)