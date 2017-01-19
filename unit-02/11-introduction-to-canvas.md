#### [⇐ Previous](./10-intermediate-d3.md.md) | [Table of Contents](./../readme.md) | [Next ⇒](./12-project.md)

# Introduction To HTML Canvas

### Objectives:

By the end of this chapter, you should be able to:

* Describe what a canvas element is and what it's used for
* Setup a canvas element and describe the coordinate system
* Draw (and clear) simple shapes on the canvas (squares, triangles, circles)
* Move shapes in the canvas


### Canvas Introduction

A canvas is an html element that allows the developer to draw shapes, text, etc.  It was originally introduced by Apple as an attempt to make the web more dynamic.

Here is a basic canvas tag:

```html
<canvas height="200" width="200">
	This text is shown to the user if the browser does not support canvas
</canvas>
```

Your imagination is the only thing holding you back with the canvas element.  This tuts plus article, [21 Ridiculously Impressive HTML5 Canvas Experiments](https://code.tutsplus.com/articles/21-ridiculously-impressive-html5-canvas-experiments--net-14210), gives some really cool examples of what is possible.

We have much more modest goals for this lesson.  We are going to go over the basic canvas setup, drawing a few shapes, and then make our shapes move.

### Canvas Setup

To create your first canvas element, let's start with this html:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Canvas Intro</title>
  <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
  <canvas id="my-canvas"></canvas>
</body>
</html>
```

And this code for `style.css`:

```css
#my-canvas {
	background-color: blue;
	height: 400px;
	width: 400px;
}
```

If you open up your html page in a browser, you should see a blue html canvas on the screen!

Now, open up your javascript console, and get the canvas element by doing the following:

```js
var canvas = document.getElementById("my-canvas");
```

Next, we'll start adding elements to the canvas, but first we need to understand the x and y coordinate system.

### Coordinates System in Canvas

In the canvas coordinate system, the **x** coordinate grows from the left side of the element to the right and the **y** coordinate grows from the top of the canvas to the bottom.

This coordinate system can seem not very intuitive to some people since they are used to graphs from math where the y value gets larger from bottom to top, but once you get the hang of it, you'll be fine.

We also want to make sure that the width and height of our canvas is the same as the width and height on the page.  Try doing the following in the console:

```js
var canvas = document.getElementById("my-canvas");
console.log("width: ", canvas.width, "height: ", canvas.height);
```

The above code should return a width of 300 and a height of 150, but our canvas has a different dimension on the page.  In fact, 300 and 150 are the default values of width and height for a canvas element.  The difference of the canvas dimensions and the real world pixel dimensions will lead to confusion and problems, so the easiest thing to do is update the canvas size to the values from our style:

```js
var canvas = document.getElementById("my-canvas");
canvas.width = canvas.scrollWidth;
canvas.height = canvas.scrollHeight;
```

### Drawing

Once you have the canvas element, the next thing you need is the context for drawing.  The context is the object that actually let's you draw to the screen.  To get the context, do the following:

```js
var ctx = canvas.getContext('2d');
```

With the context, draw a rectangle:

```js
var upperLeftX = 0;
var upperLeftY = 0;
var width = 50;
var height = 50;
ctx.fillRect(upperLeftX, upperLeftY, width, height);
```

The above code creates a black rectangle on our blue canvas.  To draw with a different color, we need to specify the `fillStyle`:

```js
ctx.fillStyle = "orange";
ctx.fillRect(upperLeftX, upperLeftY, width, height);
```

Next, let's draw a triangle.  There is no build in way to draw a triangle, but rather you can create a path that defines a shape.  So our path will need 3 points:

```js
ctx.fillStyle = "red";
ctx.moveTo(0,0);
ctx.lineTo(40, 40);
ctx.lineTo(0, 80);
ctx.fill();
```

Let's say you've drawn somethings, and now you want to clear the screen and start over.  To do that, you can use `clearRect`:

```js
var canvasWidth = 400;
var canvasHeight = 400;
ctx.clearRect(0, 0, canvasWidth, canvasHeight);
```

__Exercise__

Figure out how to draw text on the canvas

### Making Things Move

To create movement in the canvas, you need to draw something, clear the screen, then draw something again that has moved slightly.

Before we get there, let's clean up our code a little.  In the example below, the code for drawing a square is kept inside of the square object. The square object is responsible for drawing itself based on the properties that it has:

```js
var canvas = document.getElementById("my-canvas");
canvas.width = canvas.scrollWidth;
canvas.height = canvas.scrollHeight;
var ctx = canvas.getContext('2d');
	
var square = {
	corner: [0,0],
	width: 50,
	height: 50,
	color: "red",
	draw: function() {
		ctx.fillStyle = this.color;
		ctx.fillRect(this.corner[0], this.corner[1], this.width, this.height);
	}
}
	
function draw() {
	ctx.clearRect(0, 0, canvas.width, canvas.height);
	square.draw();
}

draw();
```

The first step to adding movement is to create a drawing loop using `setInterval`.  So rather than calling the `draw` function directly, setInterval will call it.  The more frequently we call draw, the higher the _frame rate_ of our motion is.  In other words, the _frame rate_ is the number of screens drawn per second.  If we draw a frame every second, we would have a frame rate of 1 frame per second. Typically, if you want the motion too look natural, you want a frame rate of at least 20 frames per second:

```js
// Drawing every 50ms = 20 draws in 1 second or 20 fps
var intervalId = setInterval(draw, 50);
```

Now in the console if you do the following:

```js
square.corner = [50, 50];
```

You should see the square being redrawn somewhere else.

Now to make the square move diagonally across the screen, modify the draw function to update the corner of the square slightly on each draw:

```js
function draw() {
	ctx.clearRect(0, 0, canvas.width, canvas.height);
	square.corner[0] += 2;
	square.corner[1] += 2;
	square.draw();
}
```

Now you should see a square moving diagonally across your screen!
