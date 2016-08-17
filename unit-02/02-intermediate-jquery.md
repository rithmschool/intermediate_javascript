#### [⇐ Previous](./06-es2015.md) | [Table of Contents](./../readme.md) | [Next ⇒](./08-how-the-web-works.md)

# jQuery Fundamentals

### Objectives:

By the end of this chapter - you should be able to

- Include jQuery and perform basic dom manipulation using the library
- Explain what a jQuery object is and how it is different than an HTMLElement
- Traverse and manipulate the DOM using jQuery methods
- Add event listeners using jquery
- Utilize jQuery animations to add additional dynamic behavior to a page
- Describe what event delegation is and how it is performed in jQuery 
- Refactor jQuery code to use selector caching

### What is jQuery?

jQuery is easily one of the most popular JavaScript libraries out there. It is very helpful with DOM manipulation, AJAX and much more. To include it on a page, you can use the CDN here `<script src="http://ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.js"></script>`. All of the jQuery selectors and many functions begin with a `$`. This is just an alias (nickname) for `jQuery` so the expression `jQuery === $` will always be true!

## Common jQuery methods

#### $(document).ready or $(function(){})

To make sure the DOM has loaded, we can use the `$(document).ready` function or use `$(function(){})` as shorthand.

```javascript
$(document).ready(function(){
    // the DOM has now loaded
})
```

#### jQuery objects / get / selectors

To get started with jQuery, let's find some elements in the DOM! The way jQuery selects items from the DOM is through using CSS selectors (another good reason to know them well!) just like `querySelector`. One of the trickier parts about jQuery is what is actually returned to you when you use many of the functions including the selectors. When you select something (in the example below that will be all the `article` tags) jQuery returns an (array-like) object called a `jQuery object`. 

```javascript
$(document).ready(function(){
    $("article")
})
```

When you examine this in the chrome console you will see that this looks like an array with `[]` notation around it. Just know that you can **not** directly use vanilla javascript methods like `innerText`, `style` etc on these - but not to worry, jQuery has all of that covered for us! You can read more about jQuery objects [here](https://www.smashingmagazine.com/2014/05/mystery-jquery-object-syntax-basic-introduction/)

#### find / parent / children / prev / next

For DOM traversal we can use `find` which accepts a css selector to find elements inside a selected element. To access the children of a selected element we can use `children` and to access a child's parent element using `parent`. To find the next element we can use `next` and to find the previous we can use `prev`.

```javascript
$(document).ready(function(){
    let $divsInsideArticle = $("article").find("div").children()
})
```

#### html / text / val

To grab the innerHTML of a selected element(s) we can use the `html` function. To grab the innerText of a selected element(s) we can use the `text` function. 
To grab the innerHTML of a selected element(s) we can use the `html` function. 

```javascript
$(document).ready(function(){
    $("article").html() // innerHTML
    $("article").text() // innerText
    $("input").val() // value
    $("input").val("new value") // value = new value
})
```

#### addClass / removeClass / toggleClass

To add, remove and toggle (if add, remove and vice versa) classes we can use the `addClass`, `removeClass` and `toggleClass` functions.

```javascript
$(document).ready(function(){
    $("article").addClass("hidden") // add a class
    $("article").removeClass("hidden") // remove a class
    $("article").toggleClass("hidden") // toggle the class (if on -> off, if off -> on)
})
```

#### css / data / attr 

To access/modify the styles of an element we can use the `.css` function (one parameter for getting a value and two for setting a value). 

To access/modify the attributes of an element we can use the `.attr` function (one parameter for getting a value and two for setting a value). 

To access/modify the data-attributes of an element we can use the `.data` function (one parameter for getting a value and two for setting a value). 

```javascript
$(document).ready(function(){
    let currentBackground = $("article").css("background-color")
    $("article").css("background-color", "red");
    $("article").attr("style", "display:flex;");
    $("article").data("id", "1");
})
```

#### after / before / append / prepend

To add elements to the DOM we can place them `after` an element selected, `before` an element selected or we can `append` them to an element selected (nested in the element) or `prepend` them to a selected element (nested in the beginning)

```javascript
$(document).ready(function(){
    let newParagraph = $("<p>")
    newParagraph.text("Another article")

    let anotherParagraph = $("<p>", {
        text:"Shorter than two lines",
        css: {
            color: "purple",
            // we have to use quotes because of the '-'
            "font-size": "2em"
        }
    })
    $("article").append(newParagraph)
    $("article").prepend(anotherParagraph)
})
```

#### remove / empty

To remove elements from the DOM we can use the `.remove` function on a selected element(s). If we want to just remove the content inside of a selected element(s), we can use the `.empty` function.

```javascript
$(document).ready(function(){
    $("article").empty() // remove all content inside the article
    $("article").remove() // remove the element from the DOM
})
```

#### on / off

To add event listeners we can use the `on` function which takes a selected element or elements and attaches an event listener on what has been selected. Just like `addEventListener`, the first parameter is the name of the event followed by a callback function. There is an optional second parameter to `on` which we will examine later when we discuss event delegation. To remove event listeners we use the `off` function.

```javascript
$(document).ready(function(){
    $("article").on("click", function(e){
        console.log(e.target.value)
    })
})
```

#### fade / slide / hide / show

jQuery comes with a few helpful built in animations as well as the ability to create your own using the `animate` function. These animations used to be more popular, but with CSS transitions/animations they have fallen a bit out of favor. However, they are still good to know so let's take a look at how they are used. 

```javascript
$(document).ready(function(){
    $("article").fadeIn(200) // fade in over 200 milliseconds (there are also animations for sliding as well)
    let toggleShow = true
    $("article").on("click", function(){
        if(toggleShow){
            $(".edit_user").show()
        }
        else {
            $(".edit_user").hide()   
        }
        $(".items").slideToggle(500) // if hidden, then slideIn. If showing, then slideOut.
        toggleShow = !toggleShow  // switch the value of the boolean  
    })
})
```

You can read more about animate right [here](http://api.jquery.com/animate/)

#### filtering with jQuery

jQuery has quite a few helpful methods to select elements based on certain filters. These methods include `is`, `has`, `not`, `eq` and many more. You can read more about them [here](http://api.jquery.com/category/traversing/filtering/)

### Event Delegation

From learn.jQuery - Event delegation refers to the process of using event propagation (bubbling) to handle events at a higher level in the DOM than the element on which the event originated. It allows us to attach a single event listener for elements that exist now or in the future.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <ul> Ingredients
        <li>Sugar</li>
        <li>Milk</li>
        <li>Eggs</li>
        <li>Flour</li>
        <li>Baking Soda</li>
    </ul>
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.js"></script>
</body>
</html>
```

What we would like to do is add a single event listener on the `ul` instead of one for each `li`. We did something very similar to this when we learned about the DOM with vanilla JS. So how does it look like in jQuery? We still use the `on` method - but with one small addition

```javascript
$(function(){
    // pay CLOSE attention to the 2nd parameter
    $("ul").on("click", "li", function(e){
        console.log("You just clicked on ", e.target.value)
    });
});
```

This is also quite useful for adding event listeners to elements that are not on the page yet. Let's look at this example:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <button>Click me to make a new div!</button>
  <div id="container"></div>
  <script src="http://ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
  <script>
    $(function(){
      let count = 0;
      $("button").on("click", function(){
        let $newDiv = $("<div>", {
          text: `div number ${count}`
        })
        count++
        $("#container").append($newDiv);
      })
      $("#container").on("click", "div", function(e){
         console.log("You just clicked on ", $(e.target).text())
      });
    });
  </script>
</body>
</html>
```

You can read more about event delegation [here](https://learn.jquery.com/events/event-delegation/) and [here](http://api.jquery.com/on/)

### Selector Caching

As we start building larger applications with jQuery, one area we can start to be far more efficient in is our caching of selectors. What this means is that when we find elements in the DOM and we will be using that same found element - we should find it once and save it to a variable. Here is an example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="container"></div>
  <script src="http://ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
  <script>
    $(function(){
      let $container = $("#container")

      $container.text("Hello World!")
      $container.css('color','red')
      $container.on("mouseenter", function(e){
        console.log("moused over!")
      })
      let $newDiv = $("<div>", {
        text: "I am a div!",
        css: {
          color: "green"
        }
      })
      $container.append($newDiv)
    });
  </script>
</body>
</html>
```

### Chaining

One of the nicest things about jQuery is the ability to chain functions together (if you ever want to write functions that can be chained, you must return `this`). What this allows us to do is something like this:

```javascript
$("div").first().parent().find(".projects").css("color","red").fadeOut(200)

// this can also be written as:

$("div")
    .first()
    .parent()
    .find(".projects")
    .css("color","red")
    .fadeOut(200)
```

### Exercise

Complete the [jQuery Exercise](https://github.com/rithmschool/prework_exercises/tree/master/jquery_exercise)

### Additional Resources

#### [⇐ Previous](./08-es2015.md) | [Table of Contents](./../readme.md) | [Next ⇒](./10-how-the-web-works.md)