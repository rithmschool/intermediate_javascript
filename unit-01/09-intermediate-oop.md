#### [⇐ Previous](./08-prototypes.md) | [Table of Contents](./../readme.md) | [Next ⇒](./10-es2015.md)

# Intermediate Object Oriented Programming

### Objectives

By the end of this chapter, you should be able to:

* Create a diagram which is a high level solution to an OOP problem
* Translate an OOP diagram into code

### Solving Problems With Object Oriented Programming

Object oriented programming is a powerful tool for solving problems in your code, but it takes some practice before the concepts are solidified in your head.

Here are some suggestions for helping you solve these problems:

1. Identify components, processes, and data in the problem
2. Diagram how the components interact with one another
3. Code and test smaller components first as you work up to large pieces


### Tic Tac Toe

Let's apply these ideas to tic tac toe.  First, what are components processes and data involved in tic tac toe?  Here is a brain dump of all of the things I can think of:

* Board
* Player
* X's, O's
* Squares
* Turns (Maybe?)
* Game
* Winner/Loser

Now let's organize these thoughts a little more using words like has, owns, uses:

* There is 1 board
* A board has squares
* Squares can be X, O or empty
* There is 1 game
* A game has 2 players
* A game should decide whose turn it is
* A game has a board
* The game decides who wins or loses based on the board
* A board view can be displayed using a board


### Diagraming

Now that we have grouped some of the objects in the program, let's diagram it.  This can be as simple as a hand written drawing to help organize your thoughts.


### Exercise

Try to implement tic tac toe based on the design we've started.  You can get started with the code [here](https://github.com/rithmschool/intermediate_js_exercises/tree/master/intermediate_oop/tic_tac_toe)

### Additional Resources

#### [⇐ Previous](./08-prototypes.md) | [Table of Contents](./../readme.md) | [Next ⇒](./10-es2015.md)
