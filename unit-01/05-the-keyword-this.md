#### [⇐ Previous](./03-testing-javascript.md) | [Table of Contents](./../readme.md) | [Next ⇒](./05-call-apply-bind.md)

# The Keyword this

### Objectives:

By the end of this chapter, you should be able to:

- Explain what the keyword `this` is and what a call-site is
- Examine the four ways of determining the value of the keyword `this`
- Discuss this difference between the default and implicit binding of the keyword `this`

### What is the meaning of `this`?

The keyword `this` is one of the more unique features of JavaScript. It often presents a stumbling block to people learning JavaScript, even if they already know some other language. However, the rules governing `this` are relatively straightforward once you've familiarized yourself with them.

At its core, `this` is just a keyword in JavaScript that refers to some object. For instance, if you hop into the Chrome console and type `this`, you'll see that it refers to the `window`. However, the value of the keyword `this` depends on where in your code you use it. Let's take a look at the different ways the value for the keyword `this` gets set.

### The four ways to figure out the keyword this

The four terms here come from the incredible book *You Don't Know JS* by Kyle Simpson. You can read more about these four terms [here](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch2.md)

### Default Binding

You can think of this as the last possible option when all other rules do not apply. The default binding exists when the keyword `this` is in the global context. As we've already seen, in the case of the browser the keyword `this` has a default binding to the `window`. (Note: if `strict mode` is enabled, the default binding of `this` is `undefined`. You can read more about strict mode [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode).

```javascript
var thing = this;
thing; // window

function outer() {
    return this;
}

outer(); // window
```

For the other three bindings, the value of the keyword `this` is set to some value other than the default when it is used inside of a function. But the details of how the value gets set depend on the **call-site** of the function. The call-site is simply the way in which the function is called.

Let's examine the different ways that the keyword `this` can be set based on the call-site of its enclosing function.

### Implicit Binding

One option is that the function wrapping the keyword `this` is being called as a method on some object. In this case the call-site has some parent object, and according to the implicit binding rule, `this` should refer to this parent object. Anytime you see the keyword `this`, you should check to see whether the closing function has an associated parent object when the function is called.

```javascript
var friend = {
    firstName: "Ash",
    sayHi: function(){
        return this.firstName + " says hello!";
    }
};

// the keyword this will refer to friend when we invoke sayHi:
friend.sayHi(); // Ash says hello!
```

### Explicit Binding

We'll go into detail on this option in the [next chapter](./05-call-apply-bind.md), but here's a quick introduction. Sometimes, the keyword `this` is not what we want it to be. When we want to change the context of the keyword `this`, we use explicit binding. This can be done by using one of three functions: `call`, `apply`, or `bind`. All three take as their first parameter what you want the value of the keyword `this` to be; we'll dig into the differences between them later.

```javascript
var person = {
    firstName: "Jane",
    greetings: {
        sayHi: function(){
            return this.firstName + " says hi!";
        },
        sayBye: function(){
            return this.firstName + " says bye!";
        },
    }
}

person.greetings.sayHi() // undefined says hi! - uh-oh, we've lost the this value that we want!
person.greetings.sayHi.call(person) // Jane says hi! - here we're forcing the keyword this to refer to person

var person2 = {
    firstName: "Jim"
}

person.greetings.sayBye.call(person2) // Jim says bye! - here we're borrowing the sayBye method from person and calling it to person2!
```

We will examine `call`, `apply` and `bind` more in the [next chapter](./05-call-apply-bind.md), so don't worry if the above examples seem a bit confusing.

### `new` Binding

We will examine the `new` keyword more in a [later chapter](./07-constructor-functions.md), but for now it's important to know that the keyword `new` will trump any of the other rules listed above.The `new` keyword is used to create objects from a function (typically called a _constructor function_). When the keyword `new` is used, four things happen;

1. An empty object is created,
2. The keyword `this` inside of the constructor function refers to the empty object that was just created,
1. A `return this` is added to the constructor function (you don't need to explicitly return a value),
2. An internal link is created between the object and the `.prototype` property on the constructor function (don't worry about what this means for now, we'll cover it soon!).


```javascript
function Person(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
}

var elie = new Person("Elie", "Schoppik")

elie // {firstName: "Elie", lastName: "Schoppik"}
```

At the end of the above example, the variable `elie` refers to an object whose `firstName` and `lastName` properties were set by the constructor function. 

### Exercise

Make sure you have a basic understanding of these four examples, and a solid understanding of the first two in particular. If you still feel confused, take a look at the additional resources below. If some parts of this chapter are still a bit confusing, that's totally fine; come back after you've read through the material on OOP and the examples on explicit and `new` binding should be clearer.

### Additional Resources

[https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch2.md](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch2.md)

#### [⇐ Previous](./03-testing-javascript.md) | [Table of Contents](./../readme.md) | [Next ⇒](./05-call-apply-bind.md)