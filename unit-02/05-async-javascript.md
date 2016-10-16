#### [⇐ Previous](./04-ajax-with-jquery.md) | [Table of Contents](./../readme.md) | [Next ⇒](./06-design-patterns.md)

# Asynchronous JavaScript

### Objectives:

By the end of this chapter - you should be able to

- Define key terms like threads, parallelism and concurrency
- Use callbacks, promises, generators and ES2017 async functions to handle asynchronous code

### Key Definitions + How JavaScript Works

One of the trickier things to wrap your head around in JavaScript is the idea of managing asynchronous code. Before we learn more about the different ways to manage async code, let's define some key terms to better understand what we mean when we discuss these techniques.

### Thread

A process can have many threads, if multi-threading is supported. In JavaScript, the language only supports a single thread, so there can only be a single sequential flow of control within a program. So how are we able to write asynchronous code that "seems" to be doing multiple things at once? The answer is that it really is not, it just may appear so. Let's learn more about some key terms related to this.

### Concurrency 

Concurrency occurs when more than one task makes progress regardless of another task. This means that if two tasks are happening, the second does not need to wait for the first to finish before it can complete execution. Concurrency can occur on one a single thread and that is how asynchronous tasks happen with JavaScript.

### Parallelism 

Parallelism occurs when more than one task runs **at the same time** as another task. This means there is no taking turns, each process moves at the same time. To achieve parallelism in JavaScript, we would have to run multiple JavaScript processes (either through multiple Node.js processes or using [Web Workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers) which are not fully implemented in all browsers )

### JavaScript

So how does JavaScript do it? The answer is through the event loop! You can read more about the event loop and single threaded concurrency model with JavaScript [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop), [here](http://altitudelabs.com/blog/what-is-the-javascript-event-loop/), or watch this great video [here](https://www.youtube.com/watch?v=8aGhZQkoFbQ)

### Callbacks

```js
$.getJSON("http://www.omdbapi.com/?t=titanic", function(data){
    console.log(data)
}, function(error){
    console.log("Oops something went wrong!", data)
})
```

But when we need to manage async code in a certain order, we nest our callbacks:

```js
$.getJSON("http://www.omdbapi.com/?t=titanic", function(data){
    console.log("Titanic - ", data)
    $.getJSON("http://www.omdbapi.com/?t=ghostbusters", function(data){
        console.log("Ghostbusters -", data)
        $.getJSON("http://www.omdbapi.com/?t=sharknado", function(data){
            console.log("Sharknado -", data)
        }, function(error){
            console.log("Oops something went wrong!", data)
        })
    }, function(error){
        console.log("Oops something went wrong!", data)
    })
}, function(error){
    console.log("Oops something went wrong!", data)
})
```

One of the simplest ways of managing asynchronous code is through the use of callbacks, but callbacks get quite messy when we have to start nesting functions inside of each other. This is often referred to as the "Pyramid of Doom" or "Callback Hell". Not only do we have functions nested inside of each other, but we have another function for each possible error. It would be easier if we had more control over our asynchronous code - that is where promises can help.

### Promises

Promises are almost always preferred over callbacks when managing asynchronous code. Promises make use of callback functions, but help avoid nested and improve readability. Another advantage of promises is that they are immutable, so once a promise is done, you can not accidentally call it again (something you can do accidentally with callbacks).

ES2015 introduces a native Promise API, but promises have been in use for quite a while. Different libraries have their own implementation of Promises (previously called "deferreds" in jQuery), but the idea is very similar between all of them. 

Think of a promise as a future notice of information. Kyle Simpson gives the good analogy of ordering a cheeseburger at a fast food restaurant. 

1. You order food at the counter 
2. You pay for the food and are given a "ticket" so that other people can order while you wait. This "ticket" is the idea of a promise (a future notice of information)
3. You keep waiting and either:
    - Your ticket is called, you get your food and you return the ticket (the promise is resolved)
    - Your ticket is called, but something went wrong with the order so you return your ticket (the promise is rejected)

Let's see what this looks like:

```js
function firstPromise(){
    return new Promise(function(resolve,reject){
        if(Math.random() > .5){
            resolve("You made it!")
        } else {
            reject("Your number was too low! Try again")
        }
    })
}

firstPromise().then(function(data){
    console.log(data)
}).catch(function(error){
    console.log(error)
})

```

jQuery actually has built in support for promises!

```js
function getMovieData(title){
    return $.getJSON(`http://www.omdbapi.com/?t=${title}`);
}

getMovieData("Titanic").then(function(data){
    console.log("Here is our movie data!", data)
}).catch(function(err){
    console.log("Oops, something went wrong", err)
})

```

The other nice thing about Promises is that you can resolve multiple promises with the Promise.all function (which accepts an array of promises)

```js

function getMovieData(title){
    return $.getJSON(`http://www.omdbapi.com/?t=${title}`);
}

Promise.all([getMovieData("Titanic"), getMovieData("Ghostbusters"), getMovieData("Sharknado")]).then(function(data){
    console.log(data)
}).catch(function(err){
    console.log("Oops, something went wrong!")
})
```

We can also return the value of promise to another, lets try this example by returning values to another promise:

```js
function first(){
    return new Promise(function(resolve,reject){
        setTimeout(function(){
            console.log("first is done")
            resolve(10)
        },500)
    })
}

function second(previousPromiseData){
    return new Promise(function(resolve,reject){
         setTimeout(function(){
            console.log("second is done and we just got", previousPromiseData)
            resolve(previousPromiseData + 10)
         },500)
     })
}

function third(previousPromiseData){
    return new Promise(function(resolve,reject){
         setTimeout(function(){
            console.log("third is done and the total is", previousPromiseData + 10)
            resolve()
         },500)
     })
}

first()
    .then(second)
    .then(third)
```

### Generators

In ES2015, a special type of function called a `generator` was introduced. Generator functions are functions that can `return` multiple values using the `yield` keyword. Previously, we have only seen functions that `return` once, but generators can change that for us. Let's start with a simple generator example (generator functions are denoted using a `*`)

```js
function* firstGenerator(){
    for(var i = 0; i<5; i++){
        yield i
    }
}

var gen = firstGenerator();
gen.next() // {value: 0, done: false}
gen.next() // {value: 1, done: false}
gen.next() // {value: 2, done: false}
gen.next() // {value: 3, done: false}
gen.next() // {value: 4, done: false}
gen.next() // {value: 5, done: true}

// we can also iterate over a generator using a for..of loop

for(var data of firstGenerator()){
    console.log(data)
}

// 0 
// 1
// 2
// 3
// 4
```

Let's now see what our async example would look like if we used generators:

```js

function getMovieData(title){
    $.getJSON(`http://www.omdbapi.com/?t=${title}`, function(data){
        // we could also do gen.next(data) to make this function run all three at once
        console.log(data)
    }, function(err){
        console.log(err)
    })
}

function *displayResults() {
    var result1 = yield getMovieData("Titanic");
    console.log(result1)
    var result2 = yield getMovieData("Ghostbusters");
    console.log(result2);
    var result3 = yield getMovieData("Sharknado");
    console.log(result3);
}

var gen = displayResults();
// gen.next(); // Titanic
// gen.next(); // Ghostbusters
// gen.next(); // Sharknado

// if we want to print all without using next()
for(var movieData of gen){
    console.log(movieData)
}
```

While this may seem simple to look at, generators are not always the easiest to read and can add a great deal of complexity quickly. A better option is to use ES2017 async functions, which we will discuss later. 

### ES2017 Async Functions

In the next version of JavaScript, two keywords `async` and `await` will be added to the languages. These two keywords allow us to write code that "looks" synchronous, but is actually asynchronous. We prefix our functions with the `async` keyword to denote that they are actually asynchronous and we invoke those functions with the keyword `await` to ensure that they have completed before their values are stored. These keywords are actually not new to programming languages, both Python and C# have the same concept 

In order to use ES2017, we need to use a transpiler (turn ES2017 code into ES5/ES2015 code that the browser understands). We will use `babel` as our transpiler and in order to transpile our code we will use the `babel-cli`. In a more serious development environment we would use a module bundler like `webpack`, but that is a bit too advanced right now. To get started with this, make sure you have `node.js` installed and run the following steps

```sh
mkdir learn_async_await && cd learn_async_await
npm init -y
npm install --save-dev babel-cli babel-preset-latest request
echo '{ "presets": ["latest"] }' > .babelrc
touch script.js
```

Inside our script.js, let's make an API request to get some movie data:

```js

// the request module allows us to make server side API calls
var request = require('request');

// Using Promises alone
function getMovieWithPromises (title) {
    return new Promise(function(resolve, reject) {
        request(`http://www.omdbapi.com/?t=${title}`, function(err, res, body) {
        if (err) {
            reject(err);
        }
        resolve(body);
        });
    });
}


function showDataWithPromises(title){
    getMovieWithPromises(title).then(function(data){
        console.log("movie loaded!")
        console.log(data)
    }).catch(function(err){
        console.log(err)
    })
}

showDataWithPromises("Titanic")

// Using Async Functions

    // make use of the previously defined getMovieWithPromises since async functions return a Promise
async function showDataWithAsync(title){
  var data = await getMovieWithPromises(title)
  console.log(data)
}

// since async functions return a promise we can even chain them if we'd like!
showDataWithAsync("Titanic").then(function(data){
    console.log("movie loaded!")
    console.log(data)
}).catch(function(err){
    console.log("Something went wrong!", err)
})

// We can operate on the result of these functions just like with Promises
Promise.all([showDataWithAsync("Titanic"), showDataWithAsync("Ghostbusters")]).then(function(data){
    console.log(data)
});

```

To run this we can type `./node_modules/.bin/babel-node script.js`

The value with async functions is that we can write code that looks very synchronous and readable and still make use of Promises, which have a friendly API.

### Additional Resources

[https://github.com/getify/You-Dont-Know-JS/blob/master/async%20%26%20performance/ch2.md](https://github.com/getify/You-Dont-Know-JS/blob/master/async%20%26%20performance/ch2.md)

[https://github.com/getify/You-Dont-Know-JS/blob/master/async%20%26%20performance/ch3.md](https://github.com/getify/You-Dont-Know-JS/blob/master/async%20%26%20performance/ch3.md)

#### [⇐ Previous](./04-ajax-with-jquery.md) | [Table of Contents](./../readme.md) | [Next ⇒](./06-design-patterns.md)