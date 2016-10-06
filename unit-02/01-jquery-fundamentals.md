#### [Table of Contents](./../readme.md) | [Next ⇒](./02-intermediate-jquery.md)

# jQuery Fundamentals

### Objectives:

By the end of this chapter, you should be able to:

- Include jQuery and perform basic DOM manipulation using it
- Explain what a jQuery object is and how it is different than an HTMLElement
- Traverse and manipulate the DOM using jQuery methods
- Add event listeners using jQuery

### What is jQuery?

jQuery is easily one of the most popular JavaScript libraries out there. It is very helpful with DOM manipulation, AJAX, and much more. To include it on a page, you can use the CDN here `<script src="http://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.js"></script>`. All of the jQuery selectors and many functions begin with a `$`. This is just an alias (nickname) for `jQuery`, so if you've installed jQuery, the expression `jQuery === $` will always be true!

### Common jQuery methods

For the following examples, you can use this HTML file. Open it up in Chrome and then paste the JavaScript code samples below into the console to start exploring how jQuery works.

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<article>
		This is an article.
		<div>
  			<p>This is a paragraph inside of a div inside of an article.</p>
		</div>
 		<input class="edit_user" type="text" value="My user name">
 		<ul class="items">
 			<li>Item 1</li>
 			<li>Item 2</li>
		</ul>
	</article>
	<script src="http://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.js"></script>
</body>
</html>
```

#### `$(document).ready` or `$(function(){})`

To make sure the DOM has loaded, we can use the `$(document).ready` function or use `$(function(){})` as shorthand.

```javascript
$(document).ready(function(){
    // the DOM has now loaded
});
```

It's critical that you wait for the DOM to load before using jQuery - otherwise you might try to manipulate DOM elements that aren't on the page yet!

#### jQuery objects / get / selectors

To get started with jQuery, let's find some elements in the DOM! The way jQuery selects items from the DOM is through using CSS selectors (another good reason to know them well!), just like `querySelector` and `querySelectorAll`. One major difference between jQuery methods and the native JavaScript methods, however, is what they return to you. When you select something (in the example below this will be all the `article` tags), jQuery returns an (array-like) object called a `jQuery object`. 

```javascript
$(document).ready(function(){
    console.log($("article"));
});
```

When you examine this in the chrome console you will see that this looks like an array with `[]` notation around it. Just know that you can **not** directly use vanilla JavaScript methods like `innerText`, `style` etc. on these - but not to worry, jQuery has all of that covered for us! You can read more about jQuery objects [here](https://www.smashingmagazine.com/2014/05/mystery-jquery-object-syntax-basic-introduction/).

#### `find` / `parent` / `children` / `prev` / `next`

For DOM traversal we can use `find`, which accepts a CSS selector to find elements inside a selected element. To access the children of a selected element we can use `children` and to access a child's parent element we can use `parent`. To find the next element we can use `next` and to find the previous we can use `prev`. 

Here's a quick example:

```javascript
$(document).ready(function(){
    var $childDivsInsideArticle = $("article").find("div").children();
});
```

#### `html` / `text` / `val`

To grab the innerHTML of a selected element(s) we can use the `html` function. To grab the innerText of a selected element(s) we can use the `text` function. 

```javascript
$(document).ready(function(){
    $("article").html(); // innerHTML
    $("article").text(); // innerText
    $("input").val(); // value
    $("input").val("new value"); // value = new value
});
```

#### `addClass` / `removeClass` / `toggleClass`

To add, remove and toggle (if add, remove and vice versa) classes we can use the `addClass`, `removeClass` and `toggleClass` functions.

```javascript
$(document).ready(function(){
    $("article").addClass("hidden"); // add a class
    $("article").removeClass("hidden"); // remove a class
    $("article").toggleClass("hidden"); // toggle the class (if on -> off, if off -> on)
});
```

#### `css` / `data` / `attr` 

To access/modify the styles of an element we can use the `css` function (one parameter for getting a value and two for setting a value). 

To access/modify the attributes of an element we can use the `attr` function (one parameter for getting a value and two for setting a value). 

To access/modify the data-attributes of an element we can use the `data` function (one parameter for getting a value and two for setting a value). 

```javascript
$(document).ready(function(){
    var currentBackground = $("article").css("background-color");
    $("article").css("background-color", "red");
    $("article").attr("style", "display:flex;");
    $("article").data("id", "1");
});
```

#### filtering with jQuery

jQuery has quite a few helpful methods to select elements based on certain filters. These methods include `is`, `has`, `not`, `eq` and many more. You can read more about them [here](http://api.jquery.com/category/traversing/filtering/).

The `eq` method is particularly important when accessing elements inside of a jQuery object. For example, try running the following code in the console on the page with our sample HTML:

```javascript
var firstLi = $("li")[0];
firstLi.text(); // What do you expect?
```

You might expect that the second line to return "Item 1," since that's the text inside of the first `li`. But the problem with accessing the first `li` using bracket notation is that the result is no longer a jQuery object! In this case, `firstLi` is just a plain old HTML element: `firstLi === document.querySelector('li')` returns `true`. And since `text` is a jQuery method, not a vanilla JavaScript one, trying to call it on firstLi results in a `TypeError`.

So how can we fix this? One way would be to use `innerText` instead of `text`: 

```javascript
var firstLi = $("li")[0];
firstLi.innerText; // "Item 1"
```

But mixing vanilla JavaScript and jQuery for DOM access and manipulation is a little confusing. Since we've installed jQuery, it's best to go all in and use jQuery wherever we can.

This is what makes the `eq` function so useful. We can use it in place of bracket notation to access elements inside of a jQuery object. Unlike bracket notation, however, the result of `eq` will _again be a jQuery object!_ So code like this works just fine:

```javascript
var firstLi = $("li").eq(0);
var secondLi = $("li").eq(1);
firstLi.text(); // "Item 1"
secondLi.text(); // "Item 2"
```

`eq` also makes things easier when we want to do method chaining in jQuery (which we'll talk about more in the next chapter).

#### `after` / `before` / `append` / `prepend`

To add elements to the DOM we can place them `after` a selected element or `before` a selected element. We can also `append` them to a selected element (nested at the end of an element) or `prepend` them to a selected element (nested at the beginning).

When creating new elements, there are a couple of different options we can use to add attributes, text, and so on. Check out the difference between the way we create `newParagraph` and `anotherParagraph` below:

```javascript
$(document).ready(function(){
    var newParagraph = $("<p>");
    newParagraph.text("Another article");
    newParagraph.css("color","red");

    var anotherParagraph = $("<p>", {
        text:"Shorter than two lines",
        css: {
            color: "purple",
            // we have to use quotes because of the '-'
            "font-size": "2em"
        }
    });
    $("article").append(newParagraph);
    $("article").prepend(anotherParagraph);
});
```

#### `remove` / `empty`

To remove elements from the DOM we can use the `remove` function on a selected element (or elements). If we want to just remove the content inside of the element(s), we can use the `empty` function.

```javascript
$(document).ready(function(){
    $("article").empty(); // remove all content inside the article
    $("article").remove(); // remove the article element itself from the DOM
});
```

#### `on` / `off`

To add event listeners we can use the `on` function, which takes a selected element (or elements) and attaches an event listener on what has been selected. Just like `addEventListener`, the first parameter is the name of the event followed by a callback function. There is an optional second parameter to `on` which we will examine later when we discuss event delegation. To remove event listeners we use the `off` function.

```javascript
$(document).ready(function(){
    $("article").on("click", function(e){
        console.log(e.target.value);
    });
});
```

### Exercise

Go to the [jQuery website](http://jquery.com/) website, hop into the console, and play around with all of the jQuery methods we've discussed here.

After that, read on! There will be an exercise waiting for you at the end of the next chapter.

### Additional Resources

#### [Table of Contents](./../readme.md) | [Next ⇒](./02-intermediate-jquery.md)