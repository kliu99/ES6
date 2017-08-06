# Syntax
  * [Let and Const](#let-and-const)
    + [Rules for using `let` and `const`](#rules-for-using-let-and-const)
    + [Use cases](#use-cases)
  * [Template Literals](#template-literals)
  * [Destructuring](#destructuring)
    + [Destructuring values from an array](#destructuring-values-from-an-array)
    + [Destructuring values from an object](#destructuring-values-from-an-object)
  * [Object Literal Shorthand](#object-literal-shorthand)
  * [For...of Loop](#forof-loop)
  * [Spread`...` Operator](#spread-operator)
    + [Combining arrays](#combining-arrays)
  * [`...`Rest Parameter](#rest-parameter)
    + [Variadic functions](#variadic-functions)
    

## Let and Const
There are now two new ways to declare variables in JavaScript: `let` and `const`.

### Rules for using `let` and `const`
`let` and `const` also have some other interesting properties.

- Variables declared with `let` can be reassigned, but can’t be redeclared in the same scope.
- Variables declared with `const` must be assigned an initial value, but can’t be redeclared in the same scope, and **can’t be reassigned**.


### Use cases
The big question is when should you use `let` and `const`? The general rule of thumb is as follows:

- use `let` when you plan to reassign new values to a variable, and
- use `const` when you don’t plan on reassigning new values to a variable.

> Since `const` is the strictest way to declare a variable, we suggest that you **always declare variables with `const`** because it'll make your code easier to reason about since you know the identifiers won't change throughout the lifetime of your program. **If you find that you need to update a variable or change it, then go back and switch it from `const` to `let`**.


## Template Literals
**Template literals** are essentially string literals that include embedded expressions.

Denoted with backticks ( `` ) instead of single quotes ( '' ) or double quotes ( "" ), template literals can contain placeholders which are represented using `${expression}`. This makes it much easier to build strings.

``` javascript
let message = `${student.name} please see ${teacher.name} in ${teacher.room} to pick up your report card.`;
```

## Destructuring
In ES6, you can extract data from arrays and objects into distinct variables using *destructuring*.

### Destructuring values from an array

``` javascript
const point = [10, 25, -34];

const [x, y, z] = point;

console.log(x, y, z);
```

> **Prints:** 10 25 -34


**TIP:** You can also ignore values when destructuring arrays. For example, const [x, , z] = point; ignores the y coordinate and discards it.


### Destructuring values from an object

``` javascript
const gemstone = {
  type: 'quartz',
  color: 'rose',
  karat: 21.29
};

const {type, color, karat} = gemstone;

console.log(type, color, karat);
```

> **Prints:** quartz rose 21.29

In this example, the curly braces `{` `}` represent the object being destructured and `type`, `color`, and `karat` represent the variables where you want to store the properties from the object. Notice how you don’t have to specify the property from where to extract the values. Because `gemstone` has a property named `type`, the value is automatically stored in the `type` variable. Similarly, `gemstone` has a `color` property, so the value of `color` automatically gets stored in the `color` variable. And it's the same with `karat`.

> **TIP:** You can also specify the values you want to select when destructuring an object. For example, `let {color} = gemstone;` will only select the `color` property from the `gemstone` object.


## Object Literal Shorthand

Before ES6
``` javascript
let type = 'quartz';
let color = 'rose';
let carat = 21.29;

const gemstone = {
  type: type,
  color: color,
  carat: carat
};

console.log(gemstone);
```

ES6
``` javascript
let type = 'quartz';
let color = 'rose';
let carat = 21.29;

const gemstone = { type, color, carat };

console.log(gemstone);
```


Before ES6
``` javascript
const gemstone = {
  calculateWorth: function() { ... }
};
```

ES6
``` javascript
const gemstone = {
  calculateWorth() { ... }
};
```


## For...of Loop
You can stop or break a for...of loop at anytime.

``` javascript
const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (const digit of digits) {
  if (digit % 2 === 0) {
    continue;   // break;
  }
  console.log(digit);
}
```

> **TIP:** It’s good practice to use plural names for objects that are collections of values. That way, when you loop over the collection, you can use the singular version of the name when referencing individual values in the collection. For example, `for (const button of buttons) {...}`.


## Spread`...` Operator
The spread operator, written with three consecutive dots (`...`), is new in ES6 and gives you the ability to expand, or spread, iterable objects into multiple elements.

``` javascript
const primes = new Set([2, 3, 5, 7, 11, 13, 17, 19, 23, 29]);
console.log(...primes);
```

> **Prints:** 2 3 5 7 11 13 17 19 23 29

### Combining arrays
``` javascript
const fruits = ["apples", "bananas", "pears"];
const vegetables = ["corn", "potatoes", "carrots"];
const product = [fruits, vegetables];
console.log(produce);

// need to use: const produce = fruits.concat(vegetables);
```

> **Prints:** [Array[3], Array[3]]


using the spread`...` operator
``` javascript
const product = [...fruits, ...vegetables];
console.log(produce);
```

> **Prints:** ["apples", "bananas", "pears", "corn", "potatoes", "carrots"]


## `...`Rest Parameter
The rest parameter, also written with three consecutive dots ( `...` ), allows you to represent an indefinite number of elements as an array. This can be helpful in a couple of different situations.

``` javascript
const order = [20.17, 18.67, 1.50, "cheese", "eggs", "milk", "bread"];
const [total, subtotal, tax, ...items] = order;
console.log(total, subtotal, tax, items);
```

> **Prints:** 20.17 18.67 1.5 ["cheese", "eggs", "milk", "bread"]

### Variadic functions
``` javascript
sum(1, 2);
sum(10, 36, 7, 84, 90, 110);
sum(-23, 3000, 575000);
```

Using the `...`rest parameter
``` javascript
function sum(...nums) {
  let total = 0;  
  for(const num of nums) {
    total += num;
  }
  return total;
}
```
