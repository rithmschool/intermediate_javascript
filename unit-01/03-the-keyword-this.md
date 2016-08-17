#### [⇐ Previous](./01-testing-javascript.md) | [Table of Contents](./../readme.md) | [Next ⇒](./03-call-apply-bind.md)

# The Keyword this

### Objectives:

By the end of this chapter - you should be able to

- Explain what the keyword `this` is and what a call site is
- Examine the four ways of determining what the value of the keyword `this` is
- List use cases of the keyword `this`

#### The four ways to figure out the keyword this

The four terms here come from the incredible You Don't Know JS by Kyle Simpson. You can read more about these four terms [here](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch2.md)

#### Default Binding

You can think of this as the last possible option when all other rules do not apply. The default binding exists when the keyword `this` is in the global context. In the case of the browser - they keyword `this` when not following any other rules will be `window` (unless `strict mode` is enabled, you can read more about strict mode  [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode).

```javascript
var thing = this;
thing // window

function outer(){
    return this;
}

outer() // window
```

#### Implicit Binding

When the keyword `this` has a parent object associated with it, the implicit binding rule applies. Anytime you see the keyword `this`, try to find a parent object and go **one** level up from when the function is called.

```javascript
var friend = {
    firstName: "Ash"
    sayHi: function(){
        return this.firstName + " says hello!" 
    }
}

friend.sayHi() // Ash says hello!
```

#### Explicit Binding

Sometimes, the keyword `this` is not what we want it to be. When we want to change the context of the keyword `this` we use explicit binding. This can be done by using one of three functions (`call`, `apply` and `bind`)

```javascript
var person = {
    firstName: "Jane"
    greetings: {
        sayHi: function(){
            return this.firstName + " says hi!";
        },
        sayBye: function(){
            return this.firstName + " says bye!";
        },
    }
}

person.greetings.sayHi() // undefined says hi!
person.greetings.sayHi.call(person) // Jane says hi!

var person2 = {
    firstName: "Jim"
}

person.greetings.sayBye.call(person2) // Jim says bye!
```

We will examine `call`, `apply` and `bind` more in the next section

#### The `new` keyword

The `new` keyword is used to create objects from a function (typically called a constructor function). When the keyword `new` is used - 4 things happen;

1. An empty object is created
2. The keyword `this` inside of the constructor function refers to the empty object that was just created
1. A `return this` is added to the constructor function
2. An internal link is created between the object and the `.prototype` property on the constructor function 

```javascript

function Person(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
}

var elie = new Person("Elie", "Schoppik")

elie // {firstName: "Elie", lastName: "Schoppik"}
```

We will examine the `new` keyword more in a later section, but for now - know that the keyword `new` will trump any of the other rules listed above.

### Exercise

Make sure you have a thorough understanding of these four examples - if you still feel confused, take a look at the additional resources below.

### Additional Resources

[https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch2.md](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch2.md)

#### [⇐ Previous](./03-testing-javascript.md) | [Table of Contents](./../readme.md) | [Next ⇒](./05-call-apply-bind.md)