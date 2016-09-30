#### [⇐ Previous](./02-recursion.md) | [Table of Contents](./../readme.md) | [Next ⇒](./04-the-keyword-this.md)

# Testing JavaScript

### Objectives:

By the end of this chapter, you should be able to:

- Explain the benefits of testing code
- Explain what a test runner is
- Write unit tests with Mocha and Chai
- Explain the process of "Red, Green, Refactor"
- Differentiate between unit and integration tests

### Writing Tests: The Why

As you begin writing more complicated functions and larger applications, you're bound to make mistakes. Everyone does it, even professional programmers. When your programs grow they can become more difficult to reason about, and as hard as you may try it's impossible to predict every bug in your program. Fixing bugs also has a cost, as it can be quite easy for one fix to introduce bugs in other parts of your application.

Is there any way to avoid our programs become more brittle and difficult to maintain as they grow in complexity? Yes! The solution to this problem lies in **testing** our code as thoroughly as possible. In this chapter, you'll learn how to write tests in JavaScript that you can run automatically to verify that the code you're writing does what you expect. This makes it easier to protect against bugs, and to ensure that you don't introduce new bugs in your code as you add new features or rewrite old ones. 

It might be difficult to appreciate the value of writing tests now, but it's a critical skill to have when you're working in a large codebase with a team of other developers. Are you down with **TDD** (Test Driven Development)? By the end of this chapter, hopefully you will be.

### Writing Tests: The How

Before we see some code, let's define some technologies and functions we are going to be using.

`mocha` - this is our test runner and we will be using it to run all of our tests. A **test runner** is a tool that is responsible for running tests that you write and logging the results of the tests for you to see. You can read more about it [here](https://mochajs.org/).

`chai` - this is our expectation/assertion library. `chai` provides additional ways for you to write tests; in particular, it lets you write tests so that they are quite straightforward to read. This isn't a necessary tool to use if you're writing tests with `mocha`, but you'll very often see them paired together. For now, you can simply think of `chai` as a way to enhance your tests and make them more readable. You can read more about `chai` [here](http://chaijs.com/).

When we write our tests, there are a few functions that we'll be using quite frequently. Here are three of the most important ones:

`describe` - this function is given to us by `mocha` and it is what we use to organize our tests. You can think of a `describe` function like talking to someone and telling them "let me describe ____ to you." Very often when you're writing unit tests, you'll have one `describe` block per function you're testing (this will make more sense once you've seen some examples).

`it` - this function lives inside of `describe` functions. Inside of these `it` functions we make our expectations. Each `it` function corresponds to a test; if one of our expectations inside of the `it` function isn't met, the test fails. 

This might all sound a little strange, so before we get into a JavaScript-specific example, let's just look at an example written in plain old english. Here's how you might scaffold some tests to check whether a planet in our solar system is Earth:

- describe "Earth"
    - it "is round"
    - it "is the third planet from the sun"
    - it "is the densest of all the planets"

`expect` - this is a function given to us by `chai`. `chai` has a few different styles (expect/should/assert); all let you write readable tests, so which style you use is up to you. We'll use `expect` for the tests we write. You can read more about it [here](http://chaijs.com/guide/styles/#expect). When combined with `describe` and `it`, we can write tests that looks something like this:

- describe "Earth"
    - it "is round"
        - expect (earth.isRound).to.equal(true) 
    - it "is the third planet from the sun"
        - expect(earth.numberFromSun).to.equal(3) 
    - it "is the densest of all the planets"
        - expect(earth.density).to.be.at.least(5.51) 

Now that we have see this pseudo-code, let's look at some actual test code! Here's an example of test code written in JavaScript using `mocha` and `chai`:

```javascript
var earth = {
    isRound: true,
    numberFromSun: 3,
    density: 5.51
};

describe("Earth", function(){

  it("is round", function(){
    expect(earth.isRound).to.equal(true);
  });

  it("is the third planet from the sun", function(){
    expect(earth.numberFromSun).to.equal(3);
  });

  it("is the densest of all the planets", function(){
    expect(earth.density).to.be.at.least(5.51);
  });

});
```

Note the syntax here: both `describe` and `it` take a string as their first parameter, and a callback as the second. The callback to a `describe` typically consists of several `it` functions. Inside of each `it` function is where we write our expectations.

### Running tests in the browser

Writing tests is all well and good, but how do we actually run these tests? If you google around, you will see that most of the tests we write are in the terminal. For now, though, we are going to be using the browser (mocha.js, chai.js and mocha.css) to run our tests. 

To get started, we'll create a basic HTML page, and include the relevant JavaScript and CSS files:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Our First Mocha Tests</title>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/mocha/3.1.0/mocha.css" rel="stylesheet" />
</head>
<body>
  <div id="mocha"></div>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mocha/3.1.0/mocha.js"></script>
  <script src="http://chaijs.com/chai.js"></script>
</body>
</html>
```

Note that we include a `div` with an id of `mocha` in the body. When we run the tests, `mocha` will display the results in this div; if you don't have an element with an id of `mocha` on the page, an error will show up when you try to run the tests.

Before including our test code, we'll also need to do a bit of setup for `mocha`. For now, let's just create a `script` tag in our HTML and throw some JavaScript inside of it:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Our First Mocha Tests</title>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/mocha/3.1.0/mocha.css" rel="stylesheet" />
</head>
<body>
  <div id="mocha"></div>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mocha/3.1.0/mocha.js"></script>
  <script src="http://chaijs.com/chai.js"></script>
  <script>
  	mocha.setup('bdd'); // This sets up mocha and makes the describe function available to us
  	var expect = chai.expect; // This makes the expect function available to us
  	
  	// PUT TESTS HERE!
  	
  	mocha.checkLeaks(); // checks to be sure no variables are leaked to the global namespace during execution of the tests
  	mocha.run(); // runs the tests!
  </script>
</body>
</html>
```

Next, put the tests where the comment is, and refresh the page:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Our First Mocha Tests</title>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/mocha/3.1.0/mocha.css" rel="stylesheet" />
</head>
<body>
  <div id="mocha"></div>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mocha/3.1.0/mocha.js"></script>
  <script src="http://chaijs.com/chai.js"></script>
  <script>
  	mocha.setup('bdd'); // This sets up mocha and makes the describe function available to us
  	var expect = chai.expect; // This makes the expect function available to us
  	
 	var earth = {
		isRound: true,
		numberFromSun: 3,
		density: 5.51
	};

	describe("Earth", function(){

		it("is round", function(){
 			expect(earth.isRound).to.equal(true);
		});

		it("is the third planet from the sun", function(){
			expect(earth.numberFromSun).to.equal(3);
		});

		it("is the densest of all the planets", function(){
			expect(earth.density).to.be.at.least(5.51);
		});

	});
  	
  	mocha.checkLeaks(); // checks to be sure no variables are leaked to the global namespace during execution of the tests
  	mocha.run(); // runs the tests!
  </script>
</body>
</html>
```

You should see information on three passing tests in your browser! To see what failed tests look like, try setting `earth.isRound` to false and running the tests again.

Note: in practice, you won't be writing JavaScript inside of your HTML files. Instead, you'll typically have a couple of external JavaScript files. One of them is called a spec file (short for specification), and will contain all of your tests. The other will be the code that you're actually testing. So a more standard way to write the HTML might look like this: 

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Our First Mocha Tests</title>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/mocha/3.1.0/mocha.css" rel="stylesheet" />
</head>
<body>
  <div id="mocha"></div>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mocha/3.1.0/mocha.js"></script>
  <script src="http://chaijs.com/chai.js"></script>
  <script>mocha.setup('bdd');</script>
  <!-- Spec file (contains all tests): -->
  <script src="yourFunctionsSpec.js"></script>
  <!-- JavaScript being tested: -->
  <script src="yourFunctions.js"></script>
  <script>
  	mocha.checkLeaks();
  	mocha.run();
  </script>
</body>
</html>
```

### More Examples

Let's look at some more test code. Feel free to copy and paste this into our setup from above:

```javascript
var arr;
beforeEach(function(){
  arr = [1,3,5];
});

describe("Arrays", function(){
  describe("#push", function(){
    it("adds elements to an array", function(){
      arr.push(7);
      expect(arr).to.deep.equal([1,3,5,7]);
    });
    it("returns the new length of the array", function(){
      expect(arr.push(7)).to.equal(4);
    });
    it("adds anything into the array", function(){
      expect(arr.push({})).to.equal(4);
    });
  });
});
```

There are a couple new things going on here; let's investigate!

First, what is this `beforeEach` thing? Sometimes before each `it` block, we want to initialize some code so that the setup before running the test is the same. In the above example, our first test mutates the `arr` array by adding the number 7 to it. Rather than having to keep track of that mutation across all subsequent tests, it's easier to just set `arr` equal to the same array before each test runs. In other words, we should always strive to not have our tests change the data we are working with in other tests. `beforeEach` is a great way to save us from writing `arr = [1,3,5]` before every single block. Here's how the above code would need to look if we didn't use a `beforeEach`:

```javascript
// WITHOUT BEFORE EACH (notice how many times we repeat var arr = [1,3,5])

describe("Arrays", function(){
  describe("#push", function(){
    it("adds elements to an array", function(){
        var arr = [1,3,5];
        arr.push(7);
        expect(arr).to.deep.equal([1,3,5,7]);
    });
    it("returns the new length of the array", function(){
        var arr = [1,3,5];
        expect(arr.push(7)).to.equal(4);
    });
    it("adds anything into the array", function(){
        var arr = [1,3,5];
        expect(arr.push({})).to.equal(4);
    });
  });
});
```

Second, what is `deep.equal`? Remember, when we try to compare objects (including arrays) in JavaScript, only the reference is checked! If you have two arrays, using a `==` or `===` comparison won't tell you whether or not those arrays have the same values:

```javascript
var numbers = [1,2,3];
var numbersCopy = numbers;
var numbersOtherCopy = [1,2,3];

numbers === numbersCopy; // true, since both variables refer to the same object in memory
numbers === numbersOtherCopy // false! even though both arrays have the same structure, they are not the same array in memory.
```

So, when writing tests, how can we see if two arrays or objects consist of the same values? To do that in our tests we use `deep equal`. Deep equality checks whether the elements in two arrays are equal, rather than simply checking if the two arrays refer to the same place in memory. Along with things like `deep.equal`, `chai` has many many more operators to check for all types of things about our data - it's quite a versatile tool! When writing tests, it's always helpful to have the `chai` [documentation](http://chaijs.com/api/bdd/) open - it's a very useful when trying to formulate the proper `expect` statements. 

### Red, Green, Refactor

Once you get used to writing tests, you can use a workflow very common in TDD. This workflow is called "Red, Green, Refactor," and goes like this:

1. Start by writing a test. Make sure the tests fails (i.e. is red). Writing a failing test is important -- if the test passes before you write any code, then what are you actually testing?
2. Go write code to make the test pass.
3. Refactor your code as needed. As long as the tests are still passing, you can be reasonably confident that you aren't introducing new bugs into the program.

Let's walk through this process with a simple example. Suppose you wanted to write a function called `onlyStrings` which takes in an array, and returns only the elements in the array that are strings. Here are some tests you might want to write:

```javascript
describe("onlyStrings", function(){
  it("returns an array", function(){
    expect(onlyStrings([1,2,3])).to.be.an('array');
  });
  it("does not change arrays of strings", function(){
    expect(onlyStrings(["a","b","c"])).to.deep.equal(["a","b","c"]);
  });
  it("removes non-string primitives from an array", function(){
    expect(onlyStrings([1,"hi",null,"cool",undefined,"woah",false,"ok"])).to.deep.equal(["hi","cool","woah","ok"]);
  });
  it("removes reference types from an array", function(){
    expect(onlyStrings([{},"a",[],"b",function(){},"c"])).to.deep.equal(["a","b","c"]);
  });
});
```

After writing the tests, you would then write the `onlyStrings` function. Here's one possible implementation:

```javascript
function onlyStrings(arr) {
  var strings = [];
  for (var i = 0; i < arr.length; i++) {
    if (typeof arr[i] === "string") strings.push(arr[i]);
  }
  return strings;
}
```

With this code, the tests should pass. Upon further reflection, however, you may decide to refactor this function so that it uses `filter` instead:

```
function onlyStrings(arr) {
  return arr.filter(function(el) { return typeof el === "string"; });
}
```

With this implementation, the tests _still_ pass, and you can be fairly certain that your changes to the `onlyStrings` function haven't introduced any new bugs.

### Unit vs. Integration Tests

As you're reading about testing, you're likely to come across two different kinds of tests: unit tests and integration tests. As you're first writing tests, you'll probably be writing mostly unit tests. These are tests which are written for one small component of your application, e.g. one function. They're meant to test the individual pieces, or _units_ of your application. Integration test, by contrast, are meant to test the system as a whole, and ensure that different pieces of the application are working correctly as a whole. The distinction isn't terribly important right now, but it's good to know what the terms mean and how they're different. For more on this, check out [this](http://stackoverflow.com/questions/5357601/whats-the-difference-between-unit-tests-and-integration-tests) Stack Overflow article.

### Exercise

Complete the [JavaScript Testing Exercise](https://github.com/rithmschool/intermediate_js_exercises/tree/master/testing_exercise)

### Additional Resources

[http://www.andrewsouthpaw.com/2015/01/08/beginners-guide-to-testing-with-mocha-chai/](http://www.andrewsouthpaw.com/2015/01/08/beginners-guide-to-testing-with-mocha-chai/)

#### [⇐ Previous](./02-recursion.md) | [Table of Contents](./../readme.md) | [Next ⇒](./04-the-keyword-this.md)