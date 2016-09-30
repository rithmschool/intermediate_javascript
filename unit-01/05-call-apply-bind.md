#### [⇐ Previous](./04-the-keyword-this.md) | [Table of Contents](./../readme.md) | [Next ⇒](./06-introduction-to-oop.md)

# Call, Apply, and Bind

### Objectives:

By the end of this chapter - you should be able to

- Compare and contrast the parameters and return values of `call`, `apply` and `bind`
- List multiple use cases for each of these methods
- Change the context of the keyword `this` using `call`, `apply` and `bind`.

### A quick review of the keyword this

In the past section, we mentioned four ways to determine the value of the keyword this.

1. Default binding - catch all rule, when the keyword `this` is in the global context
2. Implicit Binding - when the keyword `this` is attached to a parent object
3. Explicit Binding - when we want to explicitly set the context for the keyword `this`
4. The `new` keyword - when the `new` keyword is used, the keyword `this` refers to an object that is created from a function after the `new` keyword (usually called a constructor function).

So which rule will we examine in this readme? If you said number 3 you are correct! To explicitly bind the keyword `this` we use three functions - `call`, `apply` and `bind`.

### Call

`call` will immediately invoke the function that it is attached to and takes in as the first parameter, what you would like the context of the keyword `this` to be (often called the `thisArg`). The second to n-th parameters of call are the parameters of the function you are immediately invoking, separated by commas;

```javascript
// CALL EXAMPLE HERE
```

#### Use cases for call

```javascript
function sumArgumentsIncorrectly(){
    return arguments.reduce(function(acc,next){
        return acc+next;
    },0) // this throws a type error because the arguments "array like object" does not contain the method reduce!
}

sumArgumentsIncorrectly(1,2,3,4,5) // 15

function sumArgumentsCorrectly(){
    // we are going to use the slice method which makes copies of arrays, but instead of making a copy of [], we will use the arguments array as the context that we want slice to be called in. We can immidiately attach reduce and we are good to go!
    return [].slice.call(arguments).reduce(function(acc,next){
        return acc+next;
    },0)
}

sumArgumentsCorrectly(1,2,3,4,5) // 15
```

### Apply

`apply` is very similar to `call`, except it only takes in a maximum of two arguments. The first is the same as `call`, but the second is always an array (`[]`) of parameters.

```javascript
// ADD APPLY EXAMPLE
```

#### Use cases for call

```javascript

var numbersArray = [1,2,3,4,5];

Math.max.apply(this,numbersArray) // 5

var arrayToBeFlattened = [1,2,[3,4],[5,6]]

[].concat.apply([],arrayToBeFlattened) // [1,2,3,4,5,6]
```

### Bind

Just like `call`, bind takes in as its first parameter what we would like the context of the keyword `this` to be and the 2nd to n-th parameters are the arguments to the function. However, bind does not immediately invoke the function - it returns a function definition that can be called at a later point in time (or right away, but you will very rarely do this).

```javascript
function add(a,b){
    return a+b;
}

var partialAdd = add.bind(this,2)
partialAdd(4) // 6
```

#### A review of Closure

In a previous unit we mentioned that closure is:

<blockquote>when a function is able to access variables from an outer function that has already returned.</blockquote>

We see that the bind function returns a new function to us so what bind is doing is simply using closure! Let's take a look at a very simple (JavaScript's implementation is quite a bit more complex) version of bind.

```javascript
function bind(fn,thisArg){
    var outerArgs = Array.prototype.slice.call(arguments)
    var argsWeWant = outerArgs.slice(2) // we don't want the fn and thisArg values! Let's copy from the 2nd index of the arguments array to the end!
    return function(){
        return fn.apply(thisArg,argsWeWant.concat(Array.from(arguments))) // remember that the 2nd parameter of apply takes in an array. So we are concatenating (joining) the arguments from the outer function with the arguments from the inner function to form 1 big array of arguments to be used when the inner function is finally called.
    }
}

function add(a,b){
  return a+b;
}

// check out some cool stuff we can do with our bind function now!

bind(add,this,1,11)() // 12
bind(add,this)(1,11) // 12
bind(add,this,1)(11) // 12
bind(add,this,1)(11,5,6,7,8,9) // 12 - the rest are ignored!
```

#### Use cases for bind

A very common use case for bind is when you lose the correct context of the keyword `this`, but do not want to execute the function immediately: 

```javascript
var obj = {
    firstName: "Elie",
    sayHi: function(){
        setTimeout(function(){
            console.log(this.firstName + " says hi!")
        },1000)
    }.bind(this)
}
```

In this example, we are ensuring that we get the correct context of the keyword `this`. If we did not use the `bind(this)` - the context of the keyword `this` would be the Window object, which does not have firstName property! 

So why did we not use `call` or `apply`? Remember that `bind` does not execute the function right away - instead it just returns a function definition. For the example above that is exactly what we want, because we don't want to run the function right away, we want to wait 1000 milliseconds!

### Exercise

Complete the [Call Apply Bind Exercise](https://github.com/rithmschool/prework_exercises/tree/master/call_apply_bind_exercise)

### Additional Resources

#### [⇐ Previous](./04-the-keyword-this.md) | [Table of Contents](./../readme.md) | [Next ⇒](./06-constructor-functions.md)