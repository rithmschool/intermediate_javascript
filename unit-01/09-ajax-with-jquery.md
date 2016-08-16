#### [⇐ Previous](./10-how-the-web-works.md) | [Table of Contents](./../readme.md) | [Next ⇒](./12-project-three.md)

# AJAX with jQuery

### Objectives:

By the end of this chapter - you should be able to

- Define what AJAX is and how it is used with jQuery
- Describe how jQuery manages asynchronous code with callbacks and deferreds 
- Explain some drawbacks to using AJAX and why it can not be used to communicate with some APIs

### History and Key Terms

`XHR` - this stands for **X**ML**H**TTP**R**equest which is an API that allows for transferring data between client and server. This API is ONLY available to scripting languages in the browser (JavaScript) - so know that this is not a technology that can be used by server side languages like Ruby, Python, Java etc. The most important thing to understand about XHR is that when used, the response from the server can be received without refreshing the web page. XHR is the underlying technology that allows for "single page" applications like gmail (where data can be send/received without the page refreshing)

`Asynchronous` - [here](http://stackoverflow.com/questions/4559032/easy-to-understand-definition-of-asynchronous-event) is a great analogy for synchronous vs asynchronous code (WOW stands for World of Warcraft)

`XML` - this stands for Extensible Markup language **MORE**

`AJAX` - this stands for Asynchronous JavaScript and XML. It is a set of technologies that allows for building single page applications (applications that do not require full page refreshes to change data on the page). The XHR object (in JavaScript) allows 
    + Asynchronous - 
    + JavaScript - 
    + And - we hope you have this one :)
    + XML

You can read more about AJAX [here](https://developer.mozilla.org/en-US/docs/AJAX/Getting_Started)

### AJAX without jQuery

```javascript
var XHR = new XMLHttpRequest
XHR.onreadystatechange = function() {
    if (XHR.readyState == 4 && XHR.status == 200) {
      document.querySelector("body").innerHTML =
      XHR.responseText;
    }
};
XHR.open("GET", "http://omdbapi.com?t=titanic");
XHR.send();
```

### AJAX with jQuery

One of the greater benefits of using jQuery is that it gives us a set of functions to make AJAX calls with less code than above. jQuery has one major function called `$.ajax` which can be configured to make all types of HTTP requests. In the case you do not need a great deal of configuration - jQuery provides a few shorthand functions like `$.get`, `$.getJSON` and `$.post` which are all based off of the `$.ajax` function. Here are some examples:

#### $.ajax

The `.ajax` function accepts an object and returns a promise (which we will discuss later). Inside of the object you **must** use these keys like `method`, `url` etc. 

```javascript
$.ajax({
    // what HTTP verb?
    method: "GET",
    // where are we making a request to?
    url: "https://omdbapi.com",
    // what should we add to the query string?
    data: {
        // a key of t and a value of titanic which will look like ?t=titanic
        t: 'titanic'
    },
    // this will add an HTTP request header of  'Accept': 'application/json'
    dataType: "json"
    // you can think of this ".then" like - then what do we do? 
}).then(function(response){
    // let's see what the response is from the OMDB API!
    console.log(response)
}).catch(function(error){
    // something went wrong :(
    console.log(error)
})
```

#### $.get

This is simply just a shorthand for what we saw above, but without the `dataType: json`.

```javascript
$.get("https://omdbapi.com?t=titanic").then(function(response){
    console.log(response)
});
```

#### $.getJSON

This is simply just a shorthand for what we saw above, but with the `dataType: json`.

```javascript
$.getJSON("https://omdbapi.com?t=titanic").then(function(response){
    console.log(response)
});
```

#### $.post

If we want to make a POST request to a server, we can either change the `method` with `.ajax` or use `.post`. The second parameter is the data we want to send to a server. 

```javascript
$.post("https://someapi.com", {name: "new user"}).then(function(response){
    console.log(response)
});
```

### Promises

So in the example above we are using this `.then` function which takes in a callback. What is going on here? What we are doing is making use of something called a `Promise`. Promises are a different (and very often more preferable) way of managing asynchronous code. We will discuss promises later on in the class and compare them with callbacks, but if you would like to read more into this - the two chapters of You Don't Know JS are a great read (links to them are in the additional resources section).

In JavaScript, we have the ability to create our own promises using the `Promise` constructor. The idea behind a promise is that we execute some code that will run at some point in time.

Here is what `$.ajax` looks like with callbacks:

```javascript
$.ajax({
    method: "GET",
    url: "https://omdbapi.com",
    data: {
        // a key of t and a value of titanic which will look like ?t=titanic
        t: 'titanic'
    },
    dataType: "json",
    success: function(response){
        console.log(response)
    },
    error: function(error){
        console.log(error)
    }
})
```

Here is what `$.ajax` looks like with promises:

```javascript
$.ajax({
    method: "GET",
    url: "https://omdbapi.com",
    data: {
        // a key of t and a value of titanic which will look like ?t=titanic
        t: 'titanic'
    },
    dataType: "json"
}).then(function(response){
    console.log(response)
}).catch(function(error){
    console.log(error)
})
```

While these two examples do not look very different, there are a few advantages to using promises. We will discuss promises in much greater detail later on in this course, but if you would like to learn more feel free to check out the additional resources. Right now we are not going to examine what it looks like when multiple callbacks are nested inside of each other (which is a bit messy and where promises can really help with cleaning up code).

### Exercise

Complete the [AJAX with jQuery Exercise](https://github.com/rithmschool/prework_exercises/tree/master/ajax_with_jquery_exercise)

### Additional Resources

[https://github.com/getify/You-Dont-Know-JS/blob/master/async%20%26%20performance/ch2.md](https://github.com/getify/You-Dont-Know-JS/blob/master/async%20%26%20performance/ch2.md)

[https://github.com/getify/You-Dont-Know-JS/blob/master/async%20%26%20performance/ch3.md](https://github.com/getify/You-Dont-Know-JS/blob/master/async%20%26%20performance/ch3.md)

#### [⇐ Previous](./10-how-the-web-works.md) | [Table of Contents](./../readme.md) | [Next ⇒](./12-project-three.md)