# Recursion 

### Objectives:

By the end of this chapter, you should be able to:

- Define what recursion is and explain some advantages / disadvantages to using recursion over iteration
- Ensure that all recursive functions have a base case to prevent the call stack from being exceeded
- Refactor iterative functions into recursive functions 

### What is recursion?

A recursive function is a function that calls itself. That sounds pretty crazy - why would we do this? Often, recursion is an alternative to iteration and in many cases it can actually be more elegant, resulting in less code that is more readable. However, it's essential to have what's called a _base case_ in all recursive functions, as well as an understanding of the _call stack_. Let's examine these terms a bit more.

### Why use recursion?

Many times, recursion can solve the same problems as iteration, but in a more elegant way. Other times, recursion is **far more useful** when solving certain types of problems. Let's think about this problem:

Imagine we have an object and we're trying to figure out if a certain value exists in an object. We could easily loop through all the keys in the object and see if the value exists, but what happens when the value of a key is another object? What happens when we have something like this?

```js
var obj = {
    data: {
        info: {
            innerData: {
                moreInfo: {
                    name: "Bob"
                }
            }
        }
    }
}
```

We would need to loop over the `obj` variable and the `data`, `info`, `innerData` and `moreInfo` keys! Instead of writing multiple loops, we could call our function again, but with a different parameter! This idea of invoking the same function again is `recursion`. With recursive functions, each recursive call is different (accepts different input)

### Always have a base case

The most important thing to have in any recursive function is a base case. A base case is a terminating case that ends the recursive calls. Without a base case, your recursive function will keep calling itself until you run out of memory. What this means is that you have too many functions on the call stack and your stack "overflows" (that's where the name StackOverflow comes from)!

Here's an example of a recursive functon without a base case:

```javascript
function thisIsAProblem() {
    thisIsAProblem();
}
```

If you invoke `thisIsAProblem`, the function will call itself. But this inner function will again call `thisIsAProblem`, which will again call `thisIsAProblem`, and so on. There's never any way for this function to stop calling itself, which means the call stack will just fill up with copies of `thisIsAProblem`, and none of these functions will be able to pop off the stack. Try pasting this code in the console; you should get a `RangeError` with the message `Maximum call stack size exceeded`.

### Visualize the call stack

When going through a recursive function, always do your best to visualize the call stack. The Chrome dev tools can help with this. Whenever you call a function again, think about how you are adding it to the stack. Finally, remember that the stack is a LIFO (**L**ast **I**n, **F**irst **O**ut) data structure. The last function that is placed (pushed) on the stack will be the first one removed from (popped off) the stack.

### Getting Started

Let's take a look at a recursive function that won't throw a `RangeError`. We will call this function `sumRange`. It will take a number and return the sum of all numbers from 1 up to the number passed in. For example, `sumRange(3)` should return 6, since 1 + 2 + 3 = 6. 

You know enough JavaScript to write this function iteratively, but let's try to take a recursive approach. In order to do that, we'll need to think about a couple of things:

- The base case: how do we stop this function from overflowing the call stack?
- How should we call the function from within itself? (We need to make sure we do something a bit different each time so that we don't overflow the stack.)

Let's take a look below:

```javascript
function sumRange(num){
   if(num === 1) return 1; 
   return num + sumRange(num-1);
}
```

The base case here is the first line inside of the function; without it, the stack will overflow (try to answer for yourself why this is the case). Then, in the second line of the function, we're calling `sumRange`, but passing in `num - 1`. This is another hallmark of recursion: when we call the function again, we typically modify the parameters of the function in some way so that we can eventually reach the base case. In this example, if the second line read `return num + sumRange(num)`, we'd once again overflow the stack (try to answer for yourself why this is the case, too).

To really understand what this function is doing, put the code in a snipped and set a break point on the `return num + sumRange(num-1);` line. Then invoke the function with some argument (e.g. `sumRange(4)`), and watch how the parameters change as you hit the break point on each function call.

### Your Turn

Write a function called `power` which takes in a base and an exponent. If the exponent is `0`, return `1`. Otherwise, return the result of the `base` multiplied by the power function to the exponent - 1. You can think of it in terms of this example:

```javascript
2^4 = 2 * 2^3;
2^3 = 2 * 2^2;
2^2 = 2 * 2^1;
2^1 = 2 * 2^0; // once our exponent is 0 we KNOW that the value is always 1!
```

Here is what that looks like

```javascript
function power(base,exponent){
   if(exponent === 0) return 1;
   return base * power(base,exponent-1);
}
```

Next, try to write a function that returns the _factorial_ of a number. As a quick refresher, a factorial of a number is the result of that number multiplied by the number before it, and the number before that number, and so on, until you reach 1. The factorial of 1 is just 1. For example:

```javascript
factorial(5); // 5 * 4 * 3 * 2 * 1 === 120
```

Here is a possible solution:

```javascript
function factorial(num){
  if(num === 1) return 1;
  return num * factorial(num-1);
}
```

### Scope in Recursion

To help with scope in recursion, we can create a `wrapper` or `helper` function which will be called multiple times in an outer function (to provide additional scope). This is done through a process called `helper method recursion`. Let's start with a problem called `all` which accepts an array and a callback and returns true if every value in the array returns `true` when passed as parameter to the callback function. Here is an iterative solution:

### Iterative Solution

```js
function all(array, condition){
    for(var i = 0; i < array.length; i++){
        if(!(condition(array[i]))){
            return false;
        }
    }
    return true
}

all([1,2,3,4], function(val){
    return val > 0
}) // true

all(["1","2",3,"4"], function(val){
    typeof val === 'string'
}) // false

```

### Helper Method Recursion

Now let's see how we could tackle this problem using helper method recursion:

```js
function allRecursive(array, condition) {
    var copy = array.slice()    
    function allRecursiveHelper(arr, cb){
        if (arr.length === 0) return true;
        if (condition(arr[0])){
            arr.shift();
            return allRecursive(arr,condition);
        } else {
            return false;
        }
    }
    return allRecursiveHelper(array, condition)
}

var numbersArray = [1,2,3,4,5]
allRecursive(numbersArray, function(v) {
    return v > 0;
})
```

### Pure Recursion

Now let's see how we could tackle this problem using a single function:

```js
function allRecursive(array, condition) {
    var copy = copy || array.slice();
    if (copy.length === 0) return true;
    if (condition(copy[0])){
        copy.shift();
        return this.allRecursive(copy,condition);
    } else {
        return false;
    }
}

var numbersArray = [1,2,3,4,5]
allRecursive(numbersArray, function(v) {
    return v > 0;
})
```

### Reimplementing document methods using recursion

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <div class="row main sidepane">
        <h1>
            <p class="row foo"></p>
            <div class="row main sidepane">
                <h2>
                    <div class="row test">1</div>
                </h2>
            </div>
        </h1>
    </div>
    <div class="row">
        <div class="main">
            <h2 class="row">
                <div id="foo" class="row main sidepane">
                    2
                </div>
            </h2>
        </div>
    </div>
    <div class="row"></div>
</body>
</html>
```

Given the following HTML, if you needed to select an element that has an ID of `foo`, you could use the `document.getElementById` method. Try to implement that function on your own! Here are some hints:

- Use helper method recursion, it will be much easier!
- In your outer function, store a variable that will either be the element found or `null` if the element can not be found
- In your inner function, iterate over an elements children and if you find the correct 
- Invoke your inner function and pass in `document.body.children` so you start from the root of the DOM.

Given the following HTML, if you needed to select all of the elements that are a div, you could use the `document.getElementsByTagName` method. Try to implement that function on your own! Here are some hints:

- The code will be VERY similar to your previous method except you will be returning an array and not checking for the same `id`, but another property.

Now given the following HTML, if you needed to select all of the elements that have a class of row, you could use the `document.getElementsByClassName` method. Try to implement that function on your own! Here are some hints:

- The code will be VERY similar to your previous method except you will be checking to see if an element contains a class (research `classList` and you will find a convinient method). 

### Additional Resources

[Computerphile Recursion](https://www.youtube.com/watch?v=Mv9NEXX1VHc)

[Fun Fun Function Recursion](https://www.youtube.com/watch?v=k7-N8R0-KY4)
