# Built-ins
  * [Symbols](#symbols)
  * [Iteration & Iterable Protocols](#iteration---iterable-protocols)
    + [The Iterable Protocol](#the-iterable-protocol)
      - [How it Works](#how-it-works)
    + [The Iterator Protocol](#the-iterator-protocol)
      - [How it Works](#how-it-works-1)
  * [Sets](#sets)
    + [How to Create a Set](#how-to-create-a-set)
    + [Modifying Sets](#modifying-sets)
    + [Working With Sets](#working-with-sets)
      - [Checking The Length](#checking-the-length)
      - [Checking If An Item Exists](#checking-if-an-item-exists)
      - [Retrieving All Values](#retrieving-all-values)
      - [Sets & Iterators](#sets---iterators)
  * [WeakSets](#weaksets)
    + [Garbage Collection](#garbage-collection)
  * [Maps](#maps)
    + [How to Create a Map](#how-to-create-a-map)
    + [Modifying Maps](#modifying-maps)
    + [Working with Maps](#working-with-maps)
    + [Looping Through Maps](#looping-through-maps)
  * [Weak Map](#weak-map)
    + [Garbage Collection](#garbage-collection-1)
  * [Promises](#promises)
    + [Indicated a Successful Request or a Failed Request](#indicated-a-successful-request-or-a-failed-request)
    + [Promises Return Immediately](#promises-return-immediately)
  * [Proxies](#proxies)
    + [A Pass Through Proxy](#a-pass-through-proxy)
    + [`Get` Trap](#-get--trap)
    + [Accessing the Target object from inside the proxy](#accessing-the-target-object-from-inside-the-proxy)
    + [Having the proxy return info, directly](#having-the-proxy-return-info--directly)
    + [`Set` trap](#-set--trap)
    + [Other Traps](#other-traps)
  * [Generators](#generators)
    + [The `Yield` Keyword](#the--yield--keyword)
    + [Yielding Data to the "Outside" World](#yielding-data-to-the--outside--world)
    + [Sending Data into/out of a Generator](#sending-data-into-out-of-a-generator)

## Symbols
A **symbol** is a unique and immutable data type that is often used to identify object properties.

To create a symbol, you write `Symbol()` with an optional string as its **description**.

``` javascript
const sym1 = Symbol('apple');
console.log(sym1);
```
> `Symbol(apple)`


if you compare two symbols with the same description…
``` javascript
const sym2 = Symbol('banana');
const sym3 = Symbol('banana');
console.log(sym2 === sym3);
```
> `false`

…then the result is `false` because the description is *only* used to described the symbol. It’s not used as part of the symbol itself—each time a new symbol is created, regardless of the description.

The bowl contains fruit which are objects that are properties of the bowl. But, we run into a problem when the second banana gets added.

``` javascript
const bowl = {
  'apple': { color: 'red', weight: 136.078 },
  'banana': { color: 'yellow', weight: 183.151 },
  'orange': { color: 'orange', weight: 170.097 },
  'banana': { color: 'yellow', weight: 176.845 }
};
console.log(bowl);
```

> `Object {apple: Object, banana: Object, orange: Object}`

Instead of adding another banana to the bowl, our previous banana is overwritten by the new banana being added to the bowl. To fix this problem, we can use symbols.
``` javascript
const bowl = {
  [Symbol('apple')]: { color: 'red', weight: 136.078 },
  [Symbol('banana')]: { color: 'yellow', weight: 183.15 },
  [Symbol('orange')]: { color: 'orange', weight: 170.097 },
  [Symbol('banana')]: { color: 'yellow', weight: 176.845 }
};
console.log(bowl);
```

> `Object {Symbol(apple): Object, Symbol(banana): Object, Symbol(orange): Object, Symbol(banana): Object}`

By changing the bowl’s properties to use symbols, each property is a unique Symbol and the first banana doesn’t get overwritten by the second banana.


## Iteration & Iterable Protocols

### The Iterable Protocol
The **iterable protocol** is used for defining and customizing the iteration behavior of objects. What that really means is you now have the *flexibility* in ES6 to specify a way for iterating through values in an object. For some objects, they already come built-in with this behavior. For example, strings and arrays are examples of built-in iterables.


#### How it Works
In order for an object to be iterable, it must implement the **iterable interface**.

The **iterator method**, which is available via the constant `[Symbol.iterator]`, is a zero arguments function that returns an iterator object. An iterator object is an object that conforms to the iterator protocol.


### The Iterator Protocol
The **iterator protocol** is used to define a standard way that an object produces a sequence of values. What that really means is you now have a process for defining how an object will iterate. This is done through implementing the `.next()` method.

#### How it Works
An object becomes an iterator when it implements the `.next()` method. The `.next()` method is a zero arguments function that returns an object with two properties:

- `value` : the data representing the next value in the sequence of values within the object
- `done` : a boolean representing if the iterator is done going through the sequence of values
    - If done is *true*, then the iterator has reached the end of its sequence of values.
    - If done is *false*, then the iterator is able to produce another value in its sequence of values.


Here’s the example from earlier, but instead we are using the array’s default iterator to step through the each value in the array.

``` javascript
const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
const arrayIterator = digits[Symbol.iterator]();

console.log(arrayIterator.next());
console.log(arrayIterator.next());
console.log(arrayIterator.next());
```

> `Object {value: 0, done: false}`  
`Object {value: 1, done: false}`  
`Object {value: 2, done: false}`

## Sets
In ES6, there’s a new built-in object that behaves like a mathematical set and works similarly to an array. This new object is conveniently called a "Set". The biggest differences between a set and an array are:

- Sets are not indexed-based - you do not refer to items in a set based on their position in the set
- items in a Set can’t be accessed individually


### How to Create a Set
There’s a couple of different ways to create a Set. The first way, is pretty straightforward:

``` javascript
const games = new Set();
console.log(games);         // Set {}
```

``` javascript
const games = new Set(['Super Mario Bros.', 'Banjo-Kazooie', 'Mario Kart', 'Super Mario Bros.']);
console.log(games);         // Set {'Super Mario Bros.', 'Banjo-Kazooie', 'Mario Kart'}
```

### Modifying Sets
After you’ve created a Set, you can use the appropriately named, `.add()` and `.delete()` methods to add and delete items from the Set:

``` javascript
const games = new Set(['Super Mario Bros.', 'Banjo-Kazooie', 'Mario Kart', 'Super Mario Bros.']);

games.add('Banjo-Tooie');
games.add('Age of Empires');
games.delete('Super Mario Bros.');

console.log(games);         // Set {'Banjo-Kazooie', 'Mario Kart', 'Banjo-Tooie', 'Age of Empires'}
```

On the other hand, if you want to delete all the items from a Set, you can use the `.clear()` method.
``` javascript
games.clear()
console.log(games);         // Set {}
```

> **TIP:** If you attempt to `.add()` a duplicate item to a Set, you won’t receive an error, but the item will not be added to the Set. Also, if you try to `.delete()` an item that is not in a Set, you won’t receive an error, and the Set will remain unchanged. Both methods return `true` if an item is successfully added or deleted from the Set and `false` if unsuccessful.


### Working With Sets

#### Checking The Length
Use the `.size` property to return the number of items in a Set:

``` javascript
const months = new Set(['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']);
console.log(months.size);           // 12
```

#### Checking If An Item Exists
Use the `.has()` method to check if an item exists in a Set. If the item is in the Set, then `.has()` will return `true`. If the item doesn’t exist in the Set, then `.has()` will return `false`.

``` javascript
console.log(months.has('September'));       // true
```

#### Retrieving All Values
Finally, use the `.values()` method to return the values in a Set. The return value of the `.values()` method is a `SetIterator` object.

``` javascript
console.log(months.values());               // SetIterator {'January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'}
```

> **TIP:** The .keys() method will behave the exact same way as the .values() method by returning the values of a Set within a new Iterator Object. The .keys() method is an alias for the .values() method for similarity with maps.

#### Sets & Iterators
looping:
- use the Set’s default iterator to step through each item in a Set, one by one.
- use the new `for...of` loop to loop through each item in a Set.

**Using the SetIterator**
Because the `.values()` method returns a new iterator object (called SetIterator), you can store that iterator object in a variable and loop through each item in the Set using `.next()`.

``` javascript
const iterator = months.values();
iterator.next();            // Object {value: 'January', done: false}
```

**Using a `for...of` Loop**
An easier method to loop through the items in a Set is the `for...of` loop.
``` javascript
const colors = new Set(['red', 'orange', 'yellow', 'green', 'blue', 'violet', 'brown', 'black']);
for (const color of colors) {
  console.log(color);
}
```

## WeakSets
A WeakSet is just like a normal Set with a few key differences:

- a WeakSet can only contain objects
- a WeakSet is not iterable which means it can’t be looped over
- a WeakSet does not have a `.clear()` method


``` javascript
const student1 = { name: 'James', age: 26, gender: 'male' };
const student2 = { name: 'Julia', age: 27, gender: 'female' };
const student3 = { name: 'Richard', age: 31, gender: 'male' };

const roster = new WeakSet([student1, student2, student3]);
console.log(roster);                
// WeakSet {Object {name: 'Julia', age: 27, gender: 'female'}, Object {name: 'Richard', age: 31, gender: 'male'}, Object {name: 'James', age: 26, gender: 'male'}}
```

### Garbage Collection
WeakSets take advantage of this by exclusively working with objects. If you set an object to `null`, then you’re essentially deleting the object. And when JavaScript’s garbage collector runs, the memory that object previously occupied will be freed up to be used later in your program.

``` javascript
student3 = null;
console.log(roster);
// WeakSet {Object {name: 'Julia', age: 27, gender: 'female'}, Object {name: 'James', age: 26, gender: 'male'}}
```

## Maps
If Sets are similar to Arrays, then Maps are similar to Objects because Maps store key-value pairs similar to how objects contain named properties with values.

Essentially, a Map is an object that lets you store key-value pairs where both the keys and the values can be objects, primitive values, or a combination of the two.


### How to Create a Map
To create a Map, simply type:

``` javascript
const employees = new Map();
console.log(employees);         // Map {}
```

### Modifying Maps
add key-values by using the Map’s `.set()` method
``` javascript
const employees = new Map();

employees.set('james.parkes@udacity.com', { 
    firstName: 'James',
    lastName: 'Parkes',
    role: 'Content Developer' 
});
employees.set('julia@udacity.com', {
    firstName: 'Julia',
    lastName: 'Van Cleve',
    role: 'Content Developer'
});
employees.set('richard@udacity.com', {
    firstName: 'Richard',
    lastName: 'Kalehoff',
    role: 'Content Developer'
});

console.log(employees);
// Map {'james.parkes@udacity.com' => Object {...}, 'julia@udacity.com' => Object {...}, 'richard@udacity.com' => Object {...}}
```

To remove key-value pairs, simply use the `.delete()` method.
``` javascript
employees.delete('julia@udacity.com');
employees.delete('richard@udacity.com');
console.log(employees);
// Map {'james.parkes@udacity.com' => Object {firstName: 'James', lastName: 'Parkes', role: 'Course Developer'}}
```

use the .clear() method to remove all key-value pairs from the Map.
``` javascript
employees.clear()
console.log(employees);         // Map {}
```

> TIP: If you `.set()` a key-value pair to a Map that already uses the same key, you won’t receive an error, but the key-value pair will overwrite what currently exists in the Map. Also, if you try to `.delete()` a key-value that is not in a Map, you won’t receive an error, and the Map will remain unchanged.    
Both methods return `true` if a key-value pair is successfully added or deleted from the Map and `false` if unsuccessful.


### Working with Maps

use the `.has()` method to check if a key-value pair exists in your Map by passing it a key.

``` javascript
const members = new Map();

members.set('Evelyn', 75.68);
members.set('Liam', 20.16);
members.set('Sophia', 0);
members.set('Marcus', 10.25);

console.log(members.has('Xavier'));
console.log(members.has('Marcus'));
```

retrieve values from a Map, by passing a key to the `.get()` method.
``` javascript
console.log(members.get('Evelyn'));         // 75.68
```

### Looping Through Maps

**Using the `MapIterator`**

Using both the `.keys()` and `.values()` methods on a Map will return a new iterator object called `MapIterator`. You can store that iterator object in a new variable and use `.next()` to loop through each key or value. Depending on which method you use, will determine if your iterator has access to the Map’s keys or the Map’s values.

``` javascript
let iteratorObjForKeys = members.keys();
iteratorObjForKeys.next();
// Object {value: 'Evelyn', done: false}
```

On the flipside, use the `.values()` method to access the Map’s values, and then repeat the same process.

``` javascript
let iteratorObjForValues = members.values();
iteratorObjForValues.next();
// Object {value: 75.68, done: false}
```

**Using a `for...of` Loop**
``` javascript
for (const member of members) {
  console.log(member);
}
```
**Using a forEach Loop**

``` javascript
members.forEach((value, key) => console.log(value, key));
```

## Weak Map
> **TIP:** WeakMaps exhibit the same behavior as a WeakSets, except WeakMaps work with key-values pairs instead of individual items.

A WeakMap is just like a normal Map with a few key differences:

- a WeakMap can only contain objects as keys,
- a WeakMap is not iterable which means it can’t be looped and
- a WeakMap does not have a .clear() method.

You can create a WeakMap just like you would a normal Map, except that you use the WeakMap constructor.


``` javascript
const book1 = { title: 'Pride and Prejudice', author: 'Jane Austen' };
const book2 = { title: 'The Catcher in the Rye', author: 'J.D. Salinger' };
const book3 = { title: 'Gulliver’s Travels', author: 'Jonathan Swift' };

const library = new WeakMap();
library.set(book1, true);
library.set(book2, false);
library.set(book3, true);

console.log(library);
```

### Garbage Collection
WeakMaps take advantage of this by exclusively working with objects as keys. If you set an object to `null`, then you’re essentially deleting the object. And when JavaScript’s garbage collector runs, the memory that object previously occupied will be freed up to be used later in your program.

``` javascript
book1 = null;
console.log(library);
```

## Promises
A JavaScript Promise is created with the new Promise constructor function - `new Promise()`. A promise will let you start some work that will be done **asynchronously** and let you get back to your regular work. When you create the promise, you must give it the code that will be run asynchronously. You provide this code as the argument of the constructor function:

``` javascript
new Promise(function () {
    window.setTimeout(function createSundae(flavor = 'chocolate') {
        const sundae = {};
        // request ice cream
        // get cone
        // warm up ice cream scoop
        // scoop generous portion into cone!
    }, Math.random() * 2000);
});
```
This code creates a promise that will start in a few seconds after I make the request. Then there are a number of steps that need to be made in the `createSundae` function.

### Indicated a Successful Request or a Failed Request
The function gets passed to the function we provide the Promise constructor - typically the word "resolve" is used to indicate that this function should be called when the request completes successfully.


``` javascript
new Promise(function (resolve, reject) {
    window.setTimeout(function createSundae(flavor = 'chocolate') {
        const sundae = {};
        // request ice cream
        // get cone
        // warm up ice cream scoop
        // scoop generous portion into cone!
        resolve(sundae);
    }, Math.random() * 2000);
});
```
The `resolve` method is used to indicate that the **request is complete and that it completed successfully**.

If there is a problem with the request and it couldn't be completed, then we could use the second function that's passed to the function. Typically, this function is stored in an identifier called "reject" to indicate that this function should be used if the request fails for some reason.
``` javascript
new Promise(function (resolve, reject) {
    window.setTimeout(function createSundae(flavor = 'chocolate') {
        const sundae = {};
        // request ice cream
        // get cone
        // warm up ice cream scoop
        // scoop generous portion into cone!
        if ( /* iceCreamConeIsEmpty(flavor) */ ) {
            reject(`Sorry, we're out of that flavor :-(`);
        }
        resolve(sundae);
    }, Math.random() * 2000);
});
```
The `reject` method is used when the **request could not be completed**. Notice that even though the request fails, we can still return data - in this case we're just returning text that says we don't have the desired ice cream flavor.


### Promises Return Immediately
The first thing to understand is that a Promise will immediately return an object.

``` javascript
const myPromiseObj = new Promise(function (resolve, reject) {
    // sundae creation code
});
```


That object has a `.then()` method on it that we can use to have it notify us if the request we made in the promise was either successful or failed. The `.then()` method takes **two functions**:

- the function to run if the request completed successfully
- the function to run if the request failed to complete

``` javascript
mySundae.then(function(sundae) {
    console.log(`Time to eat my delicious ${sundae}`);
}, function(msg) {
    console.log(msg);
    self.goCry(); // not a real method
});
```

The first function that's passed to `.then()` will be called and passed the data that the Promise's `resolve` function used. In this case, the function would receive the `sundae` object. The second function will be called and passed the data that the Promise's `reject` function was called with. In this case, the function receives the error message `"Sorry, we're out of that flavor :-("` that the `reject` function was called with in the Promise code above.


## Proxies
To create a proxy object, we use the Proxy constructor - `new Proxy();`. The proxy constructor takes two items:
- the object that it will be the proxy for
- an object containing the list of methods it will handle for the proxied object

The second object is called the **handler**.

### A Pass Through Proxy
The simplest way to create a proxy is to provide an object and then an empty handler object.

``` javascript
var richard = {status: 'looking for work'};
var agent = new Proxy(richard, {});

agent.status; // returns 'looking for work'
```
The above doesn't actually do anything special with the proxy - it just passes the request directly to the source object! If we want the proxy object to actually intercept the request, that's what the handler object is for!
The key to making Proxies useful is the handler object that's passed as the second object to the Proxy constructor. 

### `Get` Trap
The `get` trap is used to "intercept" calls to properties:
``` javascript
const richard = {status: 'looking for work'};
const handler = {
    get(target, propName) {
        console.log(target); // the `richard` object, not `handler` and not `agent`
        console.log(propName); // the name of the property the proxy (`agent` in this case) is checking
    }
};
const agent = new Proxy(richard, handler);
agent.status; // logs out the richard object (not the agent object!) and the name of the property being accessed (`status`)
```

### Accessing the Target object from inside the proxy
If we wanted to actually provide the real result, we would need to return the property on the target object:
``` javascript
const richard = {status: 'looking for work'};
const handler = {
    get(target, propName) {
        console.log(target);
        console.log(propName);
        return target[propName];
    }
};
const agent = new Proxy(richard, handler);
agent.status; 
// (1)logs the richard object, (2)logs the property being accessed, (3)returns the text in richard.status
```
Notice we added the `return target[propName];` as the last line of the `get` trap. This will access the property on the `target` object and will return it.


### Having the proxy return info, directly
Alternatively, we could use the proxy to provide direct feedback:
``` javascript
const richard = {status: 'looking for work'};
const handler = {
    get(target, propName) {
        return `He's following many leads, so you should offer a contract as soon as possible!`;
    }
};
const agent = new Proxy(richard, handler);
agent.status; 
// returns the text `He's following many leads, so you should offer a contract as soon as possible!`
```

### `Set` trap
The `set` trap is used for intercepting code that will change a property. The `set` trap receives: 
- the object it proxies 
- the property that is being set 
- the new value for the proxy

``` javascript
const richard = {status: 'looking for work'};
const handler = {
    set(target, propName, value) {
        if (propName === 'payRate') { // if the pay is being set, take 15% as commission
            value = value * 0.85;
        }
        target[propName] = value;
    }
};
const agent = new Proxy(richard, handler);
agent.payRate = 1000; // set the actor's pay to $1,000
agent.payRate; // $850 the actor's actual pay
```

In the code above, notice that the `set` trap checks to see if the `payRate` property is being set. If it is, then the proxy (the `agent`) takes 15 percent off the top for her own commission! Then, when the actor's pay is set to one thousand dollars, since the `payRate` property was used, the code took 15% off the top and set the actual `payRate` property to 850;

### Other Traps
there are actually a total of 13 different traps that can be used in a handler!

- the `get` trap - lets the proxy handle calls to property access
- the `set` trap - lets the proxy handle setting the property to a new value
- the `apply` trap - lets the proxy handle being invoked (the object being proxied is a function)
- the `has` trap - lets the proxy handle the using `in` operator
- the `deleteProperty` trap - lets the proxy handle if a property is deleted
- the `ownKeys` trap - lets the proxy handle when all keys are requested
- the `construct` trap - lets the proxy handle when the proxy is used with the `new` keyword as a constructor
- the `defineProperty` trap - lets the proxy handle when defineProperty is used to create a new property on the object
- the `getOwnPropertyDescriptor` trap - lets the proxy handle getting the property's descriptors
- the `preventExtenions` trap - lets the proxy handle calls to `Object.preventExtensions()` on the proxy object
- the `isExtensible` trap - lets the proxy handle calls to `Object.isExtensible` on the proxy object
- the `getPrototypeOf` trap - lets the proxy handle calls to `Object.getPrototypeOf` on the proxy object
- the `setPrototypeOf` trap - lets the proxy handle calls to `Object.setPrototypeOf` on the proxy object


## Generators
Valid generators
``` javascript
function* names() { /* ... */ }
function * names() { /* ... */ }
function *names() { /* ... */ }
```

### The `Yield` Keyword
The `yield` keyword is new and was introduced with ES6. It can only be used inside generator functions. `yield` is what causes the generator to pause. 

``` javascript
function* getEmployee() {
    console.log('the function has started');

    const names = ['Amanda', 'Diego', 'Farrin', 'James', 'Kagure', 'Kavita', 'Orit', 'Richard'];

    for (const name of names) {
        console.log(name);
        yield;
    }

    console.log('the function has ended');
}
```

Notice that there's now a yield inside the `for...of` loop. If we invoke the generator (which produces an iterator) and then call `.next()`, we'll get the following output:

``` javascript
const generatorIterator = getEmployee();
generatorIterator.next();
```

Logs the following to the console:

```
the function has started
Amanda
```
It's paused!

### Yielding Data to the "Outside" World
Instead of logging the names to the console and then pausing, let's have the code "return" the name and then pause.

``` javascript
function* getEmployee() {
    console.log('the function has started');

    const names = ['Amanda', 'Diego', 'Farrin', 'James', 'Kagure', 'Kavita', 'Orit', 'Richard'];

    for (const name of names) {
        yield name;
    }

    console.log('the function has ended');
}
```

Notice that now instead of `console.log(name);` that it's been switched to `yield name;`. With this change, when the generator is run, it will "yield" the name back out to the function and then pause its execution. Let's see this in action:

``` javascript
const generatorIterator = getEmployee();
let result = generatorIterator.next();
result.value                        // is "Amanda"

generatorIterator.next().value      // is "Diego"
generatorIterator.next().value      // is "Farrin"
```

### Sending Data into/out of a Generator

``` javascript
function* displayResponse() {
    const response = yield;
    console.log(`Your response is "${response}"!`);
}

const iterator = displayResponse();

iterator.next(); // starts running the generator function
iterator.next('Hello Udacity Student'); // send data into the generator
// the line above logs to the console: Your response is "Hello Udacity Student"!
```

Calling `.next()` with data (i.e. `.next('Richard')`) will send data into the generator function where it last left off. It will "replace" the `yield` keyword with the data that you provided.

So the `yield` keyword is used to pause a generator and used to send data outside of the generator, and then the `.next()` method is used to pass data into the generator. Here's an example that makes use of both of these to cycle through a list of names one at a time:

``` javascript
function* getEmployee() {
    const names = ['Amanda', 'Diego', 'Farrin', 'James', 'Kagure', 'Kavita', 'Orit', 'Richard'];
    const facts = [];

    for (const name of names) {
        // yield *out* each name AND store the returned data into the facts array
        facts.push(yield name); 
    }

    return facts;
}

const generatorIterator = getEmployee();

// get the first name out of the generator
let name = generatorIterator.next().value;

// pass data in *and* get the next name
name = generatorIterator.next(`${name} is cool!`).value; 

// pass data in *and* get the next name
name = generatorIterator.next(`${name} is awesome!`).value; 

// pass data in *and* get the next name
name = generatorIterator.next(`${name} is stupendous!`).value; 

// you get the idea
name = generatorIterator.next(`${name} is rad!`).value; 
name = generatorIterator.next(`${name} is impressive!`).value;
name = generatorIterator.next(`${name} is stunning!`).value;
name = generatorIterator.next(`${name} is awe-inspiring!`).value;

// pass the last data in, generator ends and returns the array
const positions = generatorIterator.next(`${name} is magnificent!`).value; 

// displays each name with description on its own line
positions.join('\n');
```