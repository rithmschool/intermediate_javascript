#### [⇐ Previous](./01-jquery-fundamentals.md) | [Table of Contents](./../readme.md) | [Next ⇒](./03-how-the-web-works.md)

# Intermediate jQuery

### Objectives:

By the end of this chapter, you should be able to:

- Utilize jQuery animations to add additional dynamic behavior to a page
- Describe what event delegation is, and how it is performed in jQuery 
- Refactor jQuery code to use selector caching
- Chain jQuery methods together 
- Iterate through jQuery objects using `each`, `map`, and `filter`. 
- Explain the difference between `event.preventDefault` and  `event.stopPropagation`

### jQuery Animations

jQuery comes with a few helpful built-in animations, as well as the ability to create your own using the `animate` function. These animations used to be more popular, but with CSS transitions/animations they have fallen a bit out of favor. However, they are still good to know, so let's take a look at some examples. Let's continue to use our HTML example from the previous chapter:

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

#### `fade` / `slide` / `hide` / `show`

Fading, sliding, hiding and showing are very common ways to manipulate the visibility of elements on the page with jQuery. To explore some common methods, put the following JavaScript code in a `script` tag below where you loaded jQuery:

```javascript
$(document).ready(function(){
	$("article").hide();
	$("article").fadeIn(2000); // fade in over 2000 milliseconds (there are also animations for sliding as well)
	var toggleShow = true;
	$("article").on("click", function(){
		if(toggleShow){
			$(".edit_user").show();
		}
		else {
			$(".edit_user").hide();  
		}
		$(".items").slideToggle(500); // if hidden, then slideIn. If showing, then slideOut.
		toggleShow = !toggleShow;  // switch the value of the boolean  
    });
});
```

Click on the `article` once it fades in to execute the callback written above!

You can also perform more general animations, similar to CSS animations, using jQuery's `animate` function. You can read more about this function [here](http://api.jquery.com/animate/).

### Event Delegation

Event delegation is a very commonly used feature if jQuery. But what is it? From [learn.jQuery.com](https://learn.jquery.com/events/event-delegation/):

>Event delegation refers to the process of using event propagation (bubbling) to handle events at a higher level in the DOM than the element on which the event originated. It allows us to attach a single event listener for elements that exist now or in the future.

Let's revisit the following HTML, and include jQuery:

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
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.js"></script>
</body>
</html>
```

What we would like to do is `console.log` the text of an `li` when the user clicks on it. Moreover, we'd like to do this by adding a single event listener on the `ul` instead of one for each `li`. We did something very similar to this when we learned about the DOM with vanilla JavaScript. So what does it look like in jQuery? We still use the `on` method, but with one small addition:

```javascript
$(function(){
    // pay CLOSE attention to the 2nd parameter
    $("ul").on("click", "li", function(e){
        console.log("You just clicked on ", $(e.target).text());
    });
});
```

In this case, you can also refer to the event target using the keyword `this`:

```javascript
// an equivalent approach:
$(function(){
    // pay CLOSE attention to the 2nd parameter
    $("ul").on("click", "li", function(){
        console.log("You just clicked on ", $(this).text());
    });
});
```

This is also quite useful for adding event listeners to elements that are not on the page yet. Create a new empty HTML file, throw the following code into it, and open the page in Chrome to explore an example of how this works:

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
      var count = 0;
      $("button").on("click", function(){
        var $newDiv = $("<div>", {
          text: `div number ${count}`
        });
        count++;
        $("#container").append($newDiv);
      });
      $("#container").on("click", "div", function(e){
         console.log("You just clicked on ", $(e.target).text());
      });
    });
  </script>
</body>
</html>
```

You can read more about event delegation [here](https://learn.jquery.com/events/event-delegation/) and [here](http://api.jquery.com/on/)

### Selector Caching

As we start building larger applications with jQuery, we can make those applications more efficient by caching our selectors. What this means is that when we find elements in the DOM that we will be using repeatedly, we should find them once and save them to variables. Here's an example:

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
      var $container = $("#container");

      $container.text("Hello World!");
      $container.css('color','red');
      $container.on("mouseenter", function(e){
        console.log("moused over!");
      });
      
      var $newDiv = $("<div>", {
        text: "I am a div!",
        css: {
          color: "green"
        }
      });
      
      $container.append($newDiv);
    });
  </script>
</body>
</html>
```

In this case, jQuery only needs to go into the DOM and search for the `div` with an id of `container` one time. Since we store that jQuery object in a variable, we don't need to go back into the DOM and find the element again on subsequent lines.

### Chaining

One of the nicest things about jQuery is the ability to chain functions together. What this allows us to do is something like this:

```javascript
$("div").first().parent().find(".projects").css("color","red").fadeOut(200);

// this can also be written as:

$("div")
    .first()
    .parent()
    .find(".projects")
    .css("color","red")
    .fadeOut(200);
```

This sort of method chaining is a fairly common pattern, so it's good to see an example of it.

### jQuery Iterators

Consider the following HTML page:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Long list</title>
</head>
<body>
  <ul>
    Profit/Loss by Day
    <li>-$10</li>
    <li>$9</li>
    <li>$1</li>
    <li>$1</li>
    <li>$0</li>
    <li>$0</li>
    <li>$10</li>
    <li>-$7</li>
    <li>-$6</li>
    <li>-$9</li>
  </ul>
  <script src="http://ajax.googleapis.com/ajax/libs/jquery/3.1.0/jquery.js"></script>
</body>
</html>
```

Suppose you wanted to print each day's profit or loss to the console using jQuery. One way you could do this is by iterating through the jQuery object you get when you grab all of the `li`s: 

```javascript
for (var i = 0; i < $("li").length; i++) {
	console.log("Day " + i + ": " + $("li").eq(i).text());
}
```

#### `each`

One way to refactor this code, however, is to use jQuery's built-in `each` function. `each` accepts a callback, and that callback can take two arguments: the first refers to the current index (analogous to `i` in the above example), and the second refers to the current element corresponding to that index. This is similar to the `forEach` method in JavaScript, but the order of the arguments is reversed! (Index, then element in jQuery; element, then index with `forEach`.)

Here's how we could acheive the same result using `each`:

```javascript
$("li").each(function(i,el) {
	console.log("Day " + i + ": " + $(el).text());
});
```

Note that the second parameter in the callback _doesn't_ refer to a jQuery object. If you want to use jQuery methods on it, you'll need to wrap it in `$(...)`, as we did in the example above.

#### `map`

jQuery has another iterator called `map`. The structure of this iterator is similar, but it does different things. `map` creates a new jQuery object that contains the return values from the callback inside of `map`. This is why, when using `map`, you need to be sure to always return something inside of the callback. Again, this is similar to the native JavaScript `map` method on arrays, but the order of the arguments is flipped.

Here's an example. Suppose you want to add some `p` tags to the page explaining what happened each day. Here's how you could do this with `map`:

```javascript
var summary = $("li").map(function(i,el) {
  return $("<p>", {
    text: "On day " + i + ", I earned " + $(el).text() 
  });
});
// summary is now a jQuery object which contains one p tag for each li

$("body").append(summary.get()); 
// this appends the summary to the page.
// The .get method turns summary, a jQuery object, into an array.
// Without invoking get, you'll receive a TypeError.
```

#### `filter`

There's a third iterator available to you in jQuery, called `filter`. Filter takes elements out of a jQuery object if they fail some test provided by the callback (very similar to the native `filter` array method). For example, suppose we wanted red text highlighting the days we lost money. Here's how we could achieve that effect using `filter`:

```javascript
$("li").filter(function(i,el) {
	return $(el).text()[0] === "-"
	// returns true if the first character in the text is a minus sign,
	// i.e. this checks whether or not the number is negative
}).css('color','red');
// We've chained filter and css together; filter returns to us the 
// li's with negative numbers in them, and we then style those li's
// to have red text.
```

Click on the links to learn more about jQuery's [each](http://api.jquery.com/each/), [map](http://api.jquery.com/map/), and [filter](http://api.jquery.com/filter/).

### `preventDefault` vs `stopPropagation`

In the last chapter, we saw that the callback to the "on" function accepts an argument corresponding to the event object. We've also talked about the `preventDefault` method on the event object.

There's another method, called `stopPropagation`, which is sometimes confused with `preventDefault`. Before closing out this chapter, let's briefly highlight an example to show the difference between the two.

Create a new HTML file, put the following code into it, then open the page in Chrome and open your dev tools (for simplicity, we've thrown the relevant JavaScript code inside of a `script` tag):

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Long list</title>
</head>
<body>
  <div>
    <a href="http://www.google.com" id="google">Google</a>
  </div>
  <div>
    <a href="http://www.yahoo.com" id="yahoo">Yahoo</a>
  </div>
  <div>
    <a href="http://www.bing.com" id="bing">Bing</a>
  </div>
  <script src="http://ajax.googleapis.com/ajax/libs/jquery/3.1.0/jquery.js"></script>
  <script>
    $(function() {
      
      $("div").on('click', function(e) {
        console.log("you clicked on a div!");
      });

      $("#google").on('click', function(e) {
        e.preventDefault();
      });

      $("#yahoo").on('click', function(e) {
        e.preventDefault();
        e.stopPropagation();
      });

      $("#bing").on('click', function(e) {
        e.stopPropagation();
      });

    });
  </script>
</body>
</html>
```

As you can see, this page consists of three `a` tags, each of which is inside of a `div`. There are listeners on each `a` tag, and on each `div`, so that some code should execute whenever you click on a link or on a `div`. 

However, the behavior you see should be slightly different depending on which link you click on:

- If you click on "Google," you should see that the link doesn't do anything, but you get a message logged to the console. `e.preventDefault()` is preventing the default behavior of the link (i.e., taking you to Google.com), but it doesn't affect the event listener on the parent `div`.
- If you click on "Yahoo," nothing should happen. The link is broken, as with "Google," but you also shouldn't see anything in the console! This is because `e.stopPropagation()` stops the click event from bubbling up from the `a` tag and on to the link.
- If you click on "Bing", you'll see that the link works. `e.stopPropagation()` doesn't prevent the default behavior of the link, it just stops the event from bubbling up to the parent div.

Just remember: `stopPropagation` will prevent an event from bubbling up, but it won't impact the event's behavior on the target. If you want to stop the default behavior of the event on its target, use `preventDefault`. This will begin to make more sense once you start building full-stack web applications, and begin using these methods (especially `preventDefault`) more regularly.

### Exercise

Complete the [jQuery Exercise](https://github.com/rithmschool/intermediate_js_exercises/tree/master/jquery_exercise)

### Additional Resources

[http://stackoverflow.com/questions/5963669/whats-the-difference-between-event-stoppropagation-and-event-preventdefault](http://stackoverflow.com/questions/5963669/whats-the-difference-between-event-stoppropagation-and-event-preventdefault)

#### [⇐ Previous](./01-jquery-fundamentals.md) | [Table of Contents](./../readme.md) | [Next ⇒](./03-how-the-web-works.md)