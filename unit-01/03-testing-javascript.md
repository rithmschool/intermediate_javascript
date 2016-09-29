#### [⇐ Previous](./02-recursion.md) | [Table of Contents](./../readme.md) | [Next ⇒](./04-the-keyword-this.md)

# Testing JavaScript

### Objectives:

By the end of this chapter - you should be able to

- Explain the benefits of testing code and what a test runner is
- Differentiate unit and integration tests
- Write unit tests with Mocha and Chai

### Getting Started

Before we see some code - let's define some technologies / functions we are going to be using.

`mocha` - this is our test runner and we will be using it to run all of our tests. You can read more about it [here](https://mochajs.org/)

`chai` - this is our expectation/assertion library - you can read more about it [here](http://chaijs.com/)

`describe` - this function is given to us by `mocha` and it is what we use to organize our tests. You can think of a describe function like talking to someone and telling them "let me describe ____ to you"

`it` - this function lives inside of `describe` functions. Inside of these `it` functions we make our expectations. So combined with `describe` we can think about our tests like this

- describe "Earth"
    - it "is round"
    - it "is the third planet from the sun"
    - it "is the densest of all the planets"

`expect` - this is a function given to us by Chai. Although Chai has a few different styles (expect/should/assert). You can read more about it [here](http://chaijs.com/guide/styles/#expect). Now combined with `describe` and `it` we can run tests that looks something like this:

- describe "Earth"
    - it "is round"
        - expect (earth.isRound).to.equal(true) 
    - it "is the third planet from the sun"
        - expect(earth.numberFromSun).to.equal(3) 
    - it "is the densest of all the planets"
        - expect(earth.density).to.be.at.least(5.51) 

Now that we have see this pseudo-code, let's look at some actual test code!

```javascript
var earth = {
    isRound: true,
    numberFromSun: 3,
    density: 5.51
}
describe("Earth", function(){
  it("is round", function(){
    expect(earth.isRound).to.equal(true)
  });
  it("is the third planet from the sun", function(){
    expect(earth.isRound).to.equal(3)
  });
  it("is the densest of all the planets", function(){
    expect(earth.density).to.be.at.least(5.51)
  });
});
```

Let's look at some more test code:

```javascript
var arr;
beforeEach(function(){
    arr = [1,3,5]
})

describe("Arrays", function(){
  describe("#push", function(){
    it("adds elements to an array", function(){
        arr.push(7)
        expect(arr.length).to.deep.equal([1,3,5,7])
    });
    it("returns the new length of the array", function(){
        expect(arr.push(7).to.equal(4))
    });
  });
});
```

So a couple new things here....

- What is this `beforeEach` thing? Sometimes before each `it` block, we want to initialize some code so that we don't use the same variable (and each tests changes information). We should always strive to not have our tests change the data we are working with in other tests. BeforeEach is a great way to save us from writing `arr = [1,3,5]` before every single block. That might sound confusing so here is an example:

```javascript
// WITHOUT BEFORE EACH (notice how many times we repeat arr =[1,3,5])
var arr;
describe("Arrays", function(){
  describe("#push", function(){
    it("adds elements to an array", function(){
        arr = [1,3,5]
        arr.push(7)
        expect(arr.length).to.deep.equal([1,3,5,7])
    });
    it("returns the new length of the array", function(){
        arr = [1,3,5]
        expect(arr.push(7).to.equal(4))
    });
    it("adds anything into the array", function(){
        arr = [1,3,5]
        expect(arr.push({}).to.equal(4))
    });
  });
});
```
 
Now using `beforeEach`:

```javascript
// WITH BEFORE EACH (much better....)
var arr;
beforeEach(function(){
    arr = [1,3,5]
})

describe("Arrays", function(){
  describe("#push", function(){
    it("adds elements to an array", function(){
        arr.push(7)
        expect(arr.length).to.deep.equal([1,3,5,7])
    });
    it("returns the new length of the array", function(){
        expect(arr.push(7).to.equal(4))
    });
    it("adds anything into the array", function(){
        expect(arr.push({}).to.equal(4))
    });
  });
});
```

- What is `deep.equal`? Remember, we can not use `===` (or even `==`) to compare any kind of Object (including arrays), so how can we see if they are the same thing? To do that in our tests we use `deep equal`. Along with things like `deep.equal`, Chai has many many more operators to check for all types of things about our data - it's quite a versatile tool!

### Running tests in the browser

Now that we have seen this code - how do we actually run these tests? If you google around, you will see that most of the tests we write are in the terminal, but for now we are going to be using the browser (mocha.js, chai.js and mocha.css) to run our tests. In some of the previous exercises you were using these same exact tools to get the tests to pass. Now you are going to be writing tests as well! In the exercise, we will provide you with the source code for mocha and chai (they are in a folder called `lib`). 

### Exercise

Complete the [JavaScript Testing Exercise](https://github.com/rithmschool/prework_exercises/tree/master/testing_exercise)

### Additional Resources

[http://www.andrewsouthpaw.com/2015/01/08/beginners-guide-to-testing-with-mocha-chai/](http://www.andrewsouthpaw.com/2015/01/08/beginners-guide-to-testing-with-mocha-chai/)

#### [⇐ Previous](./02-recursion.md) | [Table of Contents](./../readme.md) | [Next ⇒](./04-the-keyword-this.md)