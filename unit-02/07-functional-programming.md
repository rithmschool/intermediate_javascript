#### [⇐ Previous](./06-design-patterns.md) | [Table of Contents](./../readme.md) | [Next ⇒](./08-introduction-to-d3.md)

### Objectives:

By the end of this chapter - you should be able to

- Define what functional programming is
- Understand key components of functional programming like pure functions, composition and currying
- Examine what reactive programming and observables are

### Functional Programming

When first looking at what functional programming you come accross a myriad of complex terms like monads and lambda calculus, but let's start with a simpler definition. Functional programming is an alternative to object oriented programming and it conists of building programs with small, reusable functions. That may seem trivial, but there are special conditions that these functions will need to have. First, we want to always strive to create "pure" functions.

### Pure Functions

A pure function is a predictable function that does not have an side-effects. What does that mean? When a pure function is called many times with the same input, it will always give the same output (this is also known as idempotence) and is predictable. Another characteristic of pure functions are that they do not modify external state, or change values outside of their scope. 

Let's try to identify some pure and impure functions:

Are the following functions **pure** or **impure**?

```js
var arr = [2,4,6]
function doubleValues(arr){
    for(var i =0; i< arr.length; i++){
        arr[i] = arr[i]*2
    }
}

doubleValues(arr)
arr // [4, 8, 12]

doubleValues(arr)
arr // [8, 16, 24]
```

The function is **impure** because there is a side effect, we are mutating or changing the `arr` variable and if we call this function again, we will get a different value!

```js
var arr = [2,4,6]
function doubleValues(arr){
    return arr.map(function(val){
        return val*2
    })
}

doubleValues(arr) // [4,8,12]
doubleValues(arr) // [4,8,12]
doubleValues(arr) // [4,8,12]
```

This function is **pure** because there is no side effect, if we wanted to double the result of double, we could combine these functions together! `doubleValues(doubleValues(arr)) // [8,16,24]` and we still would not change the `arr` variable. Pretty cool!

How about this one?

```js
var start = {};

function addNameToObject(obj,val){
    obj.name = val
    return obj;
}
```

The function is **impure** because there is a side effect, we are mutating or changing the `start` variable and if we call this function again, we will get a different value!

```js
var start = {};

function addNameToObject(obj,val){
    var newObj = {name: val}
    return Object.assign({}, obj, newObj)
}
```

The function is **impure** because there is a not side effect and we are not mutating or changing the `start` variable. if we call this function again, we will get a different value!

```js
var arr = [1,2,3,4]
function addToArr(arr,val){
    arr.push(val);
    return arr;
}

addToArr(arr, 5) // [1,2,3,4,5]
arr // [1,2,3,4,5]
```

The function is **impure** because there is a side effect and we are mutating or changing the `arr` variable. if we call this function again, we will get a different value!

```js
var arr = [1,2,3,4]
function addToArr(arr,val){
    var newArr = arr.concat(val);
    return newArr;
}

addToArr(arr, 5) // [1,2,3,4,5]
```

The function is **pure** because there is a not side effect and we are notmutating or changing the `arr` variable. if we call this function again, we will get a different value!

You can read more about pure functions [here](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976#.d1qdboexh), [here](https://egghead.io/lessons/javascript-redux-pure-and-impure-functions), and if you are looking for a more advanced read, take a look [here](http://www.nicoespeon.com/en/2015/01/pure-functions-javascript/)

### Closures

Another very common tool with functional programming is the use of closures, or functions that make use variables defined in outer functions that have previously returned. We will see how closures allow us to make use of partial application, or partially calling and applying parameters to one function. We will also make heavy use of closures when we discuss currying. Here is an example of a simple closure

```js
function outer(){
    var num = 10
    return function inner(newNum){
        // the inner function makes use of num
        // which was defined in the outer function 
        // which has returned by the time inner makes use of it
        return num + newNum;
    }
}
```

While closures are a very powerful tool in functional programming (and in general), they can cause memory issues when not written properly. You can learn more about these issues (called memory leaks) [here](https://auth0.com/blog/four-types-of-leaks-in-your-javascript-code-and-how-to-get-rid-of-them/)

### Currying

Currying is the process of breaking down a function that takes multiple arguments into a series of functions that take part of the arguments. Let's examine a very simple curry function. We will partially apply a functions arguments one at a time

```js
function simpleCurry(fn, ...outerArgs){
    return function(...innerArgs){
        return fn.apply(this, outerArgs.concat(innerArgs))
    }
}

function add(a,b){
    return a+b
}

var a1 = simpleCurry(a1,2)
a1(10) // 12
```

This simpleCurry works fine when we only have two parameters, but what happens if we have an infinite number of arguments? Or what happens if we don't bother to pass in a value at all? Let's look at a more complex curry.

```js
function complexCurry(fn) {
  return function f1(...f1innerArgs) {
    if (f1innerArgs.length >= fn.length) {
      return fn.apply(this, f1innerArgs);
    } else {
      return function f2(...f2innerArgs) {
        return f1.apply(this, f1innerArgs.concat(f2innerArgs)); 
      }
    }
  };
}
complexCurry(add)()()()(2)()()()(4) // 6
```

You can read more about currying [here](https://medium.com/javascript-scene/curry-or-partial-application-8150044c78b8#.fl7cycdpe), [here](http://blog.carbonfive.com/2015/01/14/gettin-freaky-functional-wcurried-javascript/), and [here](https://medium.com/@kbrainwave/currying-in-javascript-ce6da2d324fe#.pnzb8aq4c)

### Composition

Very commonly, currying is used to combine two or more functions to produce a new function. We call this process of combining functions "composition". Let's imagine that we want to do the following to a string of data

- reverse the string
- make sure every variable is upper case
- filter out all of the vowels
- join it back into a string with a ":" in the middle.

We could do that as:

```js
function convert(str){
    return str.split('')
                .reverse()
                .map(function(val){
                    return val.toUpperCase();
                })
                .filter(function(val){
                    return ["A","E","I","O","U"].indexOf(val) !== -1
                })
                .join(":")
}

convert("hello") // "O:E"
```

This works totally fine, but what if we could combine all our functions into a single function! When we think about composition we start from the inside out. So if our function is `f(g(x))` - we would start with `x, g and then f`. Here is what it might look like with a `compose` function:

```js
function compose(...functions) {
    return function(start){
        return functions.reduceRight(function(acc, next) {
            return next(acc)
        } , start);
    } 
} 
```

Since we are composing functions from the inside out (`f(g(x))` => `x -> g -> f`) we need to accumulate the functions from right to left, so we use `reduceRight`. We will see there is a more readable way of doing this later on if we go from left to right.

Since we are passing function definitions we need to curry our functions in order to return one function at a time. Remember, the purpose of currying is to return a single argument back so let's bring that back:

```js
function complexCurry(fn) {
  return function f1(...f1innerArgs) {
    if (f1innerArgs.length >= fn.length) {
      return fn.apply(this, f1innerArgs);
    } else {
      return function f2(...f2innerArgs) {
        return f1.apply(this, f1innerArgs.concat(f2innerArgs)); 
      }
    }
  };
}
```

Now let's curry our functions and pass them into compose.

```js
var join = curry(function(str, arr){
    return arr.join(str)
})

var toUpperCase = curry(function(str){
    return str.toUpperCase();
})

var split = curry(function(delimiter, str){
    return str.split(delimiter);
})

var removeLetters = curry(function(str){
    return ["A","E","I","O","U"].indexOf(str) !== -1;
})

var reverse = curry(function(arr){
    return arr.reverse();
})

var convertLetters = compose(
    join(''),
    filter(removeLetters),
    reverse(),
    split(''),
    toUpperCase()
)

console.log(convertLetters('This is some pretty crazy stuff')); // UAEEOII
```

If we wanted to start with the outer most function `f -> g -> x` we can use what is called a `pipe` or `flow` function.

```js
function flow(...functions) {
    return function start(){
        return functions.reduce(function(acc, next) {
            next(acc)
        } , start);
    } 
} 
```

We could then hopefully do something like this:

```js
var convertLetters = flow(
    toUpperCase(),
    split(''),
    reverse(),
    filter(removeLetters),
    join('')
)

console.log(convertLetters('This is some pretty crazy stuff'));
```

You can read more about function composition [here](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-function-composition-20dfb109a1a0#.15n09uqc1)

### Additional Resources

#### [⇐ Previous](./06-design-patterns.md) | [Table of Contents](./../readme.md) | [Next ⇒](./08-introduction-to-d3.md)
