# Functions
  * [Arrow Functions](#arrow-functions)
    + [Using Arrow Functions](#using-arrow-functions)
    + [Concise and block body syntax](#concise-and-block-body-syntax)
  * [Arrow Functions and the ''this'' Keyword.](#arrow-functions-and-the---this---keyword)
  * [Default Function Parameters](#default-function-parameters)
    + [Defaults and destructuring arrays](#defaults-and-destructuring-arrays)
    + [Defaults and destructuring objects](#defaults-and-destructuring-objects)
    + [Array defaults vs. object defaults](#array-defaults-vs-object-defaults)
  * [Classes](#classes)
    + [Static methods](#static-methods)
  * [Subclasses with ES6](#subclasses-with-es6)

## Arrow Functions
ES6 introduces a new kind of function called the **arrow function** (`=>`). Arrow functions are very similar to regular functions in behavior, but are quite different syntactically.

``` javascript
const upperizedNames = ['Farrin', 'Kagure', 'Asser'].map(
  name => name.toUpperCase()
);
```

### Using Arrow Functions
Regular functions can be either **function declarations** or **function expressions**, however arrow functions are *always* expressions. In fact, their full name is "arrow function expressions", so they can only be used where an expression is valid. This includes being:

stored in a variable,
passed as an argument to a function,
and stored in an object's property.

``` javascript
// multiple parameters requires parentheses
const orderIceCream = (flavor, cone) => console.log(`Here's your ${flavor} ice cream in a ${cone} cone.`);
orderIceCream('chocolate', 'waffle');
```
> **Prints:** Here's your chocolate ice cream in a waffle cone.

### Concise and block body syntax
If you need more than just a single line of code in your arrow function's body, then you can use the "block body syntax".

``` javascript
const upperizedNames = ['Farrin', 'Kagure', 'Asser'].map( name => {
  name = name.toUpperCase();
  return `${name} has ${name.length} characters in their name`;
});
```

Important things to keep in mind with the block syntax:

- it uses curly braces `{ }` to wrap the function body, and
- a `return` statement needs to be used to actually return something from the function.


## Arrow Functions and the ''this'' Keyword.
With regular functions, the value of `this` is set based on *how the function is called*. With arrow functions, the value of `this` is based on the *function's surrounding context*. In other words, the value of `this` inside an arrow function is the same as the value of `this` *outside the function*.


``` javascript
// constructor
function IceCream() {
  this.scoops = 0;
}

// adds scoop to ice cream
IceCream.prototype.addScoop = function() {
  setTimeout(() => { // an arrow function is passed to setTimeout
    this.scoops++;
    console.log('scoop added!');
  }, 0.5);
};

const dessert = new IceCream();
dessert.addScoop();

console.log(dessert.scoops);
```

> **Prints**: 1

Since arrow functions inherit their `this` value from the surrounding context, this code works!

When `addScoop()` is called, the value of `this` inside `addScoop()` refers to `dessert`. Since an arrow function is passed to `setTimeout()`, it's using its **surrounding context** to determine what `this` refers to inside itself. So since `this` *outside* of the arrow function refers to `dessert`, the value of `this` *inside* the arrow function will also refer to `dessert`.

## Default Function Parameters
``` javascript
function greet(name = 'Student', greeting = 'Welcome') {
  return `${greeting} ${name}!`;
}

greet(); // Welcome Student!
greet('James'); // Welcome James!
greet('Richard', 'Howdy'); // Howdy Richard!
```

### Defaults and destructuring arrays
``` javascript
function createGrid([width = 5, height = 5] = []) {
  return `Generating a grid of ${width} by ${height}`;
}
```

### Defaults and destructuring objects
``` javascript
function createSundae({scoops = 1, toppings = ['Hot Fudge']} = {}) {
  const scoopText = scoops === 1 ? 'scoop' : 'scoops';
  return `Your sundae has ${scoops} ${scoopText} with ${toppings.join(' and ')} toppings.`;
}
```

### Array defaults vs. object defaults
``` javascript
function createSundae({scoops = 1, toppings = ['Hot Fudge']} = {}) { … }
```
..with the `createSundae()` function using object defaults with destructuring, if you want to use the default value for `scoops` but change the `toppings`, then all you need to do is pass in an object with toppings:

``` javascript
createSundae({toppings: ['Hot Fudge', 'Sprinkles', 'Caramel']});
```

Compare the above example with the same function that uses array defaults with destructuring.

``` javascript
function createSundae([scoops = 1, toppings = ['Hot Fudge']] = []) { … }
```
With this function setup, if you want to use the default number of `scoops` but change the `toppings`, you'd have to call your function a little...oddly:

``` javascript
createSundae([undefined, toppings: ['Hot Fudge', 'Sprinkles', 'Caramel']]);
```

Since arrays are positionally based, we have to pass `undefined` to "skip" over the first argument (and accept the default) to get to the second argument.

Unless you've got a strong reason to use array defaults with array destructuring, we recommend going with **object defaults with object destructuring**!


## Classes
ES6 classes are just a mirage and hide the fact that prototypal inheritance is actually going on under the hood

``` javascript
class Plane {
  constructor(numEngines) {
    this.numEngines = numEngines;
    this.enginesActive = false;
  }

  startEngines() {
    console.log('starting engines…');
    this.enginesActive = true;
  }
}

typeof Plane; // function
```

> Class is just a function

**Commas are not used to separate properties or methods in a Class. If you add them, you'll get a `SyntaxError` of `unexpected token`.**

### Static methods
``` javascript
class Plane {
  constructor(numEngines) {
    this.numEngines = numEngines;
    this.enginesActive = false;
  }

  static badWeather(planes) {
    for (plane of planes) {
      plane.enginesActive = false;
    }
  }

  startEngines() {
    console.log('starting engines…');
    this.enginesActive = true;
  }
}
```
See how `badWeather()` has the word static in front of it while `startEngines()` doesn't? That makes `badWeather()` a method that's accessed directly on the `Plane` class, so you can call it like this:

``` javascript
Plane.badWeather([plane1, plane2, plane3]);
```

When creating a new instance of a JavaScript class, the `new` keyword must be used
``` javascript
const plane = new Plane(1); // this works!
```

## Subclasses with ES6
`super` and `extends` keywords to extend a class.

``` javascript
class Tree {
  constructor(size = '10', leaves = {spring: 'green', summer: 'green', fall: 'orange', winter: null}) {
    this.size = size;
    this.leaves = leaves;
    this.leafColor = null;
  }

  changeSeason(season) {
    this.leafColor = this.leaves[season];
    if (season === 'spring') {
      this.size += 1;
    }
  }
}

class Maple extends Tree {
  constructor(syrupQty = 15, size, barkColor, leaves) {
    super(size, barkColor, leaves);
    this.syrupQty = syrupQty;
  }

  changeSeason(season) {
    super.changeSeason(season);
    if (season === 'spring') {
      this.syrupQty += 1;
    }
  }

  gatherSyrup() {
    this.syrupQty -= 3;
  }
}

const myMaple = new Maple(15, 5);
myMaple.changeSeason('fall');
myMaple.gatherSyrup();
myMaple.changeSeason('spring');
```

Both `Tree` and `Maple` are JavaScript classes. The `Maple` class is a "subclass" of `Tree` and uses the `extends` keyword to set itself as a "subclass". To get from the "subclass" to the parent class, the `super` keyword is used. Did you notice that `super` was used in two different ways? In `Maple`'s constructor method, `super` is used as a function. In `Maple`'s `changeSeason()` method, `super` is used as an object!


**`super` must be called before `this`**