#### [⇐ Previous](./08-prototypes.md) | [Table of Contents](./../readme.md) | [Next ⇒](./10-es2015.md)

# Intermediate Object Oriented Programming

### Objectives

By the end of this chapter, you should be able to:

* Identify requirements, data, and processes for an OOP problem
* Translate a high level OOP design into code

### Solving Problems With Object Oriented Programming

Object oriented programming is a powerful tool for solving problems in your code, but it takes some practice before the concepts are solidified in your head.

Here are some suggestions for helping you solve these problems:

1. Ask lots of questions about requirements
	1. How should the app work in a certain case (think about edge cases)
	2. What data is given?
	3. Can I make an assumption about X or Y?
	4. Will data be given in a certain format?
	5. How would I test this app?
2. List all the requirements (don't worry about grouping things into classes or functions yet)
3. Break the problem down! Group requirements into related functionality
4. Decide what classes should exist, what data the classes own, and what operations the classes need
5. To help organize your thoughts, diagram the classes. Keep track of what is being inherited from what or which classes own other classes.
6. Code!!!

### Pong - OOP Example

Let's follow the methodology above to solve this problem.  We will work on the arcade classic, pong!

### Define The Problem

Here are some sample questions you may ask to help you figure out the requirements.  This is version 1 of the game so some functionality may be delayed until version to.  The bold text is the answer to the questions

* Should there be a game start screen? __No__
* Do I need to keep track of the score? __No__
* Who should start the game? __Pick a player who always starts__
* What should happen when a ball passes a player?  __The ball is placed at a random starting point in the middle__
* Does the game ever end? __Not in version 1__
* Should there be a computer player (artificial intelligence)? __No__
* How are the paddles moved?  __For the player on the left, s = up, z = down.  For the player on the right, up arrow is up, and down arrow is down__
* Should the game be able to resize? __No__
* Should the velocity of the ball change depending on where the ball hit the paddle?  __No__

You should be glad that we asked all of these questions.  Now we have a lot less to do.


### List Requirements

After asking questions, write down your requirements so you that you can refer back to them:

* Create a 2 player pong game
* Each player has a paddle (a rectangle that can move up and down on the screen)
* Both players are humans (no AI)
* The ball should randomly be started in the middle
* There should be a dashed line in the middle of the screen
* Once the ball goes off the screen, the player on the opposite side has scored (no need to keep track of the score though)
* When a ball hits a paddle, the ball should reverse its direction

### Group Functionality

Think about things that are logically related:

* A ball can move on the screen
* A paddle can move on the screen
* There is a line in the middle of the screen that cannot move
* The game has 2 paddles and a ball
* All objects on the screen need to be drawn somehow

### Organize Ideas Into Classes

Now that we have a high level idea of what we want, we need to start thinking a bit about implementation details.  The first choice is how should we create animation.  For this game, let's use canvas.  Next, we should turn those groups into classes:

* Velocity - just an x and y velocity that gets added to the position every frame
* CanvasComponent - has a velocity, position, height, and width.  Represents an object in the canvas.  This object should stay dumb and not have any context specific code
* Ball - has a CanvasComponent and is responsible for keeping it self within the canvas by doing bounds checking
* Paddle - An object on the page that can move up and down only.  Responds to keys to move up and down.  Checks for collisions with a ball.
* Pong - A class to represent the game.  Creates the paddles and a ball.  Also draws the dashes down the middle of the screen.
* main.js - Responsible for creating an instance of pong and starting it.



### Diagraming

Now that we have an idea of our design, let's diagram it.  This can be as simple as a hand written drawing to help organize your thoughts.  Try to capture the relationship between classes.  Take note of how one class may relate to another or the data that a class will own.

### Pong OOP Solution

You can see a solution to the pong game we have described [here](https://github.com/rithmschool/intermediate_javascript_exercises/blob/master/intermediate_oop/pong).


### Exercise

Implement a tic tac toe game using OOP.  The starter code is in the  [tic tac toe](https://github.com/rithmschool/intermediate_js_exercises/tree/master/intermediate_oop/tic_tac_toe) folder of the intermediate javascript exercises repo.


#### [⇐ Previous](./08-prototypes.md) | [Table of Contents](./../readme.md) | [Next ⇒](./10-es2015.md)
