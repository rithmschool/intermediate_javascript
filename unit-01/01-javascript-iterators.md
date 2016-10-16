#### [Table of Contents](./../readme.md) | [Next ⇒](./02-recursion.md)

# JavaScript Iterators

### Objectives:

By the end of this chapter, you should be able to:

- Use `forEach`, `map`, `filter`, and `reduce` to iterate through, transform, and manipulate arrays
- Explain the advantages and drawbacks of using iterators versus loops

We've seen how to iterate through arrays using `for` loops. But JavaScript also has a number of built-in methods on arrays, called _iterators_, which can often be used in place of `for` loops. Choosing the right iterator can allow you to write code that's both shorter and easier to read. Before we get into tradeoffs between loops and iterators too much, though, let's take a look at the iterators available to you in JavaScript.

### `forEach`

The first iterator we are going to look at is `forEach`. As we'll see with each of these iterators, the first parameter to `forEach` is a callback. This callback can have up to three parameters: the value at the current array index, the current array index, and the array itself (this will become clearer after looking at a few examples). 

The most important thing to understand about `forEach` is that it **ALWAYS** returns `undefined`. With or without the `return` keyword - it does not make a difference.

Let's look at some examples:

```javascript
var arr = [4,3,2,1];
arr.forEach(function(val,index,arr){
	console.log(val);
});

// 4
// 3
// 2 
// 1

arr.forEach(function(val,index,arr){
	console.log(index);
});

// 0
// 1
// 2 
// 3

var doubledValues = arr.forEach(function(val,index,arr){
    return val*2;
});

doubledValues; // undefined
```

It's important to note that you don't need to supply all three arguments to the callback if you don't need them all. For instance, the first example above could also be written as:

```javascript
var arr = [4,3,2,1];
// the callback here just takes a single parameter, but works exactly the same as before!
arr.forEach(function(val){
	console.log(val);
});
```

You can read more about `forEach` [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/foreach).

### `map`

Unlike `forEach`, `map` returns a _new_ array of the values returned in the callback. The structure of the callback function to `map` is identical to `forEach`: once again, the three parameters are the value, index and array (in that order).

```javascript
var arr = [1,2,3,4];
arr.map(function(val, index, array){
	return val * 2;
}); // [2,4,6,8]

var tripledValues = arr.map(function(val,index,arr){
    return val*3;
});

tripledValues; // [3,6,9,12]

function doubleArray(arr){
    // return the result of arr.map
    return arr.map(function(val){
        // return a new array with each value doubled
        return val *2;
    });
}

doubleArray([2,4]); // [4,8]
```

You can read more about `map` [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map).

### filter

`filter` returns an array just like `map`, but inside the callback you must return an expression that evaluates to `true` or `false`. If the expression evaluates to `true`, the value will be added to the returned array. 

You can think of the callback to `filter` as a sort of testing function. If the element in the array passes the test, `filter` keeps the element; otherwise, `filter` tosses it out.

```javascript
var arr = [1,2,3,4];
var valuesGreaterThanTwo = arr.filter(function(val){
	return val > 2; 
});

valuesGreaterThanTwo // [3,4]
```

You can read more about `filter` [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter).

### reduce

`reduce` is one of the most powerful iterators, but it has a bit of a steeper learning curve. The purpose of `reduce` is to take an array and _reduce_ it to a single value. What makes `reduce` so powerful is that the value you reduce can be anything (a string, boolean, object, even a 2D array!). 

The parameters to the callback of reduce (as well as the function itself) are a bit different than `forEach`, `map` and `filter`. Unlike the other iterators we've seen, the callback to `reduce` takes _four_ arguments, not three. The second through fourth arguments are the same as the ones we've seen before (value, index, array). However, the first parameter is one we haven't seen before.

Depending on where you read documentation, you will see the first parameter of reduce being called `start`, `previous`, or `accumulator`. Here's how it works: at each step in the iteration, this value is whatever was returned by the callback in the previous iteration. For the first iteration (when there is no previous value), you can pass in an initial value as the second parameter to reduce. If you don't supply an argument, reduce will assume that the value of this parameter should be the first value in the array, and it will start the iteration from the second element. 

Here are some examples. In general, the best way to understand reduce is to create a snippet, set some break points, and watch how the arguments in the callback change with each iteration. Try it out!

```javascript
var arr = [2,4,6,8];
arr.reduce(function(acc,next){
	return acc + next;
},5);

/*
In the first iteration, acc is 5 and next is 2; the callback returns 5 + 2 = 7.
In the second iteration, acc is 7 and next is 4; the callback returns 7 + 4 = 11.
In the third iteration, acc is 11 and next is 6; the callback returns 11 + 6 = 17.
In the last iteration, acc is 17 and next is 8; the callback returns 17 + 8 = 25.
Now the array is exhausted, so reduce returns 25 (which is the sum of all elements in the array, plus 5)
*/

var arr = [2,4,6,8];
arr.reduce(function(acc,next){
	return acc + next;
});

// 20 (When no second argument is supplied to reduce, the `acc` starts at the first value in the array, i.e. 2. In this case, we simply get the sum of all values in the array.) 
```

You can obtain different data types from reducing by manipulating the initial value of the accumulator. Here's an example where an array is reduced to an object: 

```javascript
var arr = [1,2,3,4];
arr.reduce(function(acc,next){
	if(next >= 2){
        acc['key' + next] = next;
    } 
    return acc;
},{});

// Think about what acc and next are for each step in the iteration!
// Ultimately, this reduce will return the following:
// Object {key2: 2, key3: 3, key4: 4}
```

You can read more about `reduce` [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce).

Relatedly, you can also reduce an array starting from the end and moving backwards, rather than starting from the front and moving forwards. To do this you can use the `reduceRight` iterator. You can read more about `reduceRight` [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight).

### some

To figure out if **ANY** single value satisfies a condition in an array, we can use the `some` function. This will return `true` if **ANY** value passes a condition specified in the callback, and `false` if all values fail the condition.

```javascript
var arr = [1,2,3,4];

var anythingGreaterThanTwo = arr.some(function(val){
    return val > 2;
});

var anyStrings = arr.some(function(val){
	return typeof val === "string";
});

anythingGreaterThanTwo; // true
anyStrings; // false
```

You can read more about `some` [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some).

### every

To figure out if **ALL** values satisfy a condition in an array, we can use the `every` function. This will return `true` if **ALL** values pass a condition specified in the callback. If even one value fails the condition, `every` will return `false`.

```javascript
var arr = [1,2,3,4];

var everythingGreaterThanTwo = arr.every(function(val){
    return val > 2;
});

var everythingLessThanFive = arr.every(function(val){
	return val < 5;
});

everythingGreaterThanTwo; // false
everythingLessThanFive; // true
```

You can read more about `every` [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every).

### find

`find` was added to JavaScript in ES2015 to make it easier to find an element in an array based on some condition. This iterator will return the first value in an array that satisfies a condition (an expression that returns `true` in the callback).

```javascript
var arr = [-3,1,8,4];
var firstValueGreaterThanTwo = arr.find(function(val){
    return val > 2;
});

firstValueGreaterThanTwo; // 8
```

You can read more about `find` [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find).

### findIndex

To find the first index in an array that satisfies a condition (an expression that returns `true` in the callback) - we can use the `findIndex` function.

```javascript
var arr = [-3,1,8,4];
var firstIndexOfElementGreaterThanTwo = arr.findIndex(function(val){
    return val > 2;
});

firstIndexOfElementGreaterThanTwo; // 2
```

You can read more about `findIndex` [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex)

### Combining Iterators

By combining iterators, you can often manipulate arrays using a sequence of very simple functions, rather than one larger, more complex loop. For example, suppose you wanted to take an array of numbers, filter out the even ones, double the remaining values, and then add everything up. We can do this by chaining three iterators together:

```javascript
var arr = [1,2,3,4,5]
arr.filter(function(val){
	return val % 2 !== 0; // only keep odd numbers
}).map(function(val){
    return val * 2; // double remaining values
}).reduce(function(acc,next){
    return acc + next; // add everything up
},0) // 18
```

This sort of chaining helps keep your code easy to reason about, because each callback is only responsible for one thing. To make things even more readable, you could give names to the callbacks:

```js
var arr = [1,2,3,4,5];

function isNumberOdd(val) {
	return val % 2 === 1;
}

function double(val) {
	return val * 2;
}

function sum(a,b) {
	return a + b;
}

arr.filter(isNumberOdd)
	.map(double)
	.reduce(sum,0); // 18
```

By keeping the code inside of our functions simple, we can reduce (no pun intendend) the likelihood that we'll introduce bugs into our code, and when we come back to our code weeks or months later, it'll hopefully be easier for us to wrap our heads around.

### Additional note and refactoring when using iterators

Just because JavaScript has a lot of different iterators, this doesn't necessarily mean you should always use an iterator in place of a `for` loop. One of the biggest advantages to using a for loop is that you can break out of the loop or continue to the next iteration using the keywords `break` and `continue`. If you try to use these keywords inside of an iterator, however, you'll get a `SyntaxError`.

Now that you've seen some iterators, let's highlight some situations when you might want to use one over another. If you're used to writing `for` loops you'll probably be most likely to use `forEach`, but this isn't always the best iterator for the job:

```javascript
var arr = [1,2,3]

// using forEach

var newArr = [];
arr.forEach(function(val){
    newArr.push(val*2);
});

// In the above example, we're creating a new array
// with the same length as the old array. So instead
// of forEach, we can refactor to use map:

var newArr = arr.map(function(val){
    return val*2;
});
```

```javascript
var arr = [1,2,3]

// using forEach
var newArr = [];
arr.forEach(function(val){
    if(val >= 2){
        newArr.push(val);
    }
})

// In the above example, we're creating a smaller array
// with the same length as the old array. We're also
// not changing any values. So instead
// of forEach, we can refactor to use filter:

// refactor to use filter
var newArr = arr.filter(function(val){
    return val >= 2;
})
```

Before using an iterator, it's always good to ask whether `forEach` is the best candidate for the job, or whether another iterator would be more appropriate. If you're trying to manipulate values in an array to return a new array of the same length, `map` is probably a better choice. If you're trying to remove some elements in the array and keep others, `filter` is more likely the way to go. And if you're looking to take an array and output some other value (a number, a string, an object, etc.), consider whether or not you can use `reduce` to get the job done.

### Exercise

Complete the [Iterators Exercise](https://github.com/rithmschool/intermediate_js_exercises/tree/master/iterators_exercise).

#### [Table of Contents](./../readme.md) | [Next ⇒](./02-recursion.md)