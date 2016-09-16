#### [⇐ Previous](./09-javascript-iterators.md) | [Table of Contents](./../readme.md) | [Next ⇒](./11-introduction-to-the-dom.md)

# Recursion 

### Objectives:

By the end of this chapter, you should be able to:

- Define what recursion is and explain some advantages / disadvantages to using recursion over iteration
- Ensure that all recursive functions have a base case to prevent the call stack from being exceeded
- Refactor iterative functions into recursive functions 

### What is recursion?

A recursive function is a function that calls itself. That sounds pretty crazy - why would we do this? Often, recursion is an alternative to iteration and in many cases it can actually be more elegant, resulting in less code that is more readable. However, it's essential to have a base case in all recursive functions, as well as an understanding of the call stack. Let's examine these a bit more.

### Always have a base case

The most important thing to have in any recursive function is a base case, which is a terminating case that ends the recursive calls. Without a base case, your recursive function will keep calling itself until you run out of memory. What this means is that you have too many functions on the call stack and your stack "overflows" (that's where the name StackOverflow comes from)!

### Visualize the call stack

When going through a recursive function, always do your best to visualize the call stack (the chrome dev tools can help with this) and every time you call a function again, think about how you are adding it to the stack. Finally, remember that the stack is a LIFO data structure. The last function that is placed (pushed) on the stack, will be the first one removed from (popped off) the stack.

### Getting Started

Let's take a look at our first recursive function. We will call this function `sumRange` and it will take a number and return the sum of all numbers from 1 up to the number passed in. For example, `sumRange(3)` should return 6, since 1 + 2 + 3 = 6. 

You know enough JavaScript to write this function iteratively, but let's try to take a recursive approach. In order to do that, we'll need to think about a couple of things:

- base case (how do we stop this function from running?)
- how to call the function again (we need to make sure we do something a bit different each time so that we don't overflow the stack)

Let's take a look below:

```javascript
function sumRange(num){
   if(num === 1) return 1; 
   return num + sumRange(num-1);
}
```

The base case here is the first line inside of the function; without it, the stack will overflow (try to answer for yourself why this is the case). Then, in the second line of the function, we're calling `sumRange`, but passing in `num - 1`. This is another hallmark of recursion: when we call the function again, we typically modify the parameters of the function in some way so that we can eventually reach the base case. In this example, if the second line read `return num + sumRange(num)`, we'd once again overflow the stack (try to answer for yourself why this is the case, too).

### Your Turn

Write a function called `power` which takes in a base and an exponent. If the exponent is `0`, return `1`. Otherwise, return the result of the `base` multiplied by the power function to the exponent-1. You can think of it in terms of this example:

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

Next, try to write a function that returns the _factorial_ of a number. As a quick refresher, a factorial of a number is the result of that number multiplied by the number before it (until you reach 1, which has a factorial of 1). For example:

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

### Exercise

Complete the [Recursion Exercise](https://github.com/rithmschool/prework_exercises/tree/master/recursion_exercise)

### Additional Resources

[Computerphile Recursion](https://www.youtube.com/watch?v=Mv9NEXX1VHc)

[Fun Fun Function Recursion](https://www.youtube.com/watch?v=k7-N8R0-KY4)

#### [⇐ Previous](./09-javascript-iterators.md) | [Table of Contents](./../readme.md) | [Next ⇒](./11-introduction-to-the-dom.md)