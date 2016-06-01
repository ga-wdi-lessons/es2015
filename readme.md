# ES6 / ES2015

### Learning Objectives

- Understand what ES2015 is and how to use it
- Exposure to many of the most useful new features introduced in ES2015
- Update an existing react app to use ES2015 features

### ES2015 Background

ES2015? ES2015 (sometimes called ES6, ESHarmony) is the new version of Javascript that was released in June 2015. It adds a ton of new syntax and functionality aimed at making writing complex applications easier

Cool! Can I use it? The short answer is yes_ (cross your fingers).  _*After the lesson__ [check out this awesome table of features and where they are supported.](https://kangax.github.io/compat-table/es6/)

**TL;DR:** Decent desktop support of Firefox, Edge (MS finally ditched IE and made something awesome), Chrome (with 'use strict'). Somethings work on Node, Safari is still 'meh' and there is limited ES6 support on mobile browsers.

The current solution to improve compatibility is to transpile your ES6 code to ES5 using something like [Babel](https://babeljs.io/) or [Traceur](https://github.com/google/traceur-compiler). I use Babel because at the moment it has slightly better feature support and an awesome REPL.

### What are it's features?
- Template literals & Multi-line strings
- Let and Const
- Concise Object Methods / Object literals
- Arrows
- Destructing
- Modules / Loaders
- Classes

Bonus
- new static methods - String, Array, Math,
- Symbols
- Default Parameters
- Spread
- Rest parameters
- Promises
- Iterators
- Map / Set / WeakMap / WeakSet
- Generators
- Proxies
- + probably some more

### Where / How can I use its features?

`"use strict";`

### Set-up

You are going to be using these features to refactor an existing app. The app
we're using today is [React OMDB](https://github.com/ga-wdi-exercises/react-omdb), which was also used in
the the second react lesson.

Steps:
1. Clone down react-omdb if you don't already have it
2. `cd` into it
3. Switch to the es6-starter branch
  - `$ git checkout es6-starter`
4. Make sure current dependencies are installed
  - `$ npm install`
5. Install additional babel dev dependencies
  - `$ npm install --save-dev babel-preset-es2015`
6. Open the repo in atom
  - `$ atom .`
7. Add the new babel preset to your `.babelrc` file
  ```json
  {
    "presets": [
      "react",
      "babel-preset-es2015"
    ]
  }
  ```

### App walkthrough

Start the webpack-dev-server by running `npm start` and go to 127.0.0.1:8080.
Spend a few minutes playing trying to figure out how the app works. Play with
the app in the browser and looking at the code in atom responsible for it.

# Template Strings

### Multi-line strings

You're probably familiar with this mess:

```js
// ES5
"<h1>❤ unicorns</h1>\n" +
"<p>Unicorn pegasus pony rainbows pegasus pony kittens. Pop pigeon rainbows pony delight kittens kittens surprise. Wereunicorn delight pony pony social unicorn surprise.</p>\n" +
"<ol>\n" +
"<li>Yea! Yeah!</li>\n" +
"<li>Yeah, woo-hoo!</li>\n" +
"</ol>"
```

In ES2015 you can place strings between back-ticks (the symbol below the `esc` key) to use multi-line strings

```js
// ES2015
`<h1>❤ unicorns</h1>
<p>Unicorn pegasus pony rainbows pegasus pony kittens. Pop pigeon rainbows pony delight kittens kittens surprise. Wereunicorn delight pony pony social unicorn surprise.</p>
<ol>
<li>Yea! Yeah!</li>
<li>Yeah, woo-hoo!</li>
</ol>`
```

### Template literals

Another painful thing in Javascript is string interpolation

```js
// ES5
var name = "bob", age = 71;
"Hello my name is "+ name +" and I'm "+ age +" years old!"
```

ES2015 introduces template literals which make the process much less painful. Back-ticks are also required and this also supports multi-line strings.

```js
// ES2015
var name = "bob", age = 71;
`Hellow my name is ${name} and I'm ${age} years old!`
```

Notice how similar this looks to JSX? That's no coincidence! JSX leverages this with a little extra syntactic sugar to allow us to write code that looks more like html in our javascript. This is also why we need babel to use JSX.

[MDN Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

# Let and Const

`let` is the new `var`

### Let

The `var` statement declares a variable globally, or locally to an entire function, optionally initializing it to a value.

```js
// ES5
function varTest() {
  var x = 31;
  if (true) {
    var x = 71;  // same variable!
    console.log(x);  // 71
  }
  console.log(x);  // 71
}
```

The `let` statement declares a **block scope** local variable, optionally initializing it to a value.

```js
// ES2015
function letTest() {
  let x = 31;
  if (true) {
    let x = 71;  // different variable
    console.log(x);  // 71
  }
  console.log(x);  // 31
}
```

Another example:
```js
"use strict";

let a = [];
(function () {
   for (let i = 0; i < 5; ++i) { // *** `let` works as expected ***
     a.push( function() {return i;} );
   }
} ());
console.log(a.map( function(f) {return f();} ));
// prints [0, 1, 2, 3, 4]

// Start over, but change `let` to `var`.
// prints [5, 5, 5, 5, 5]
```

[MDN let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)

### Const

Everything `let` does `const` also does, except that `const` can't be reassigned. The const declaration creates a read-only reference to a value. It does not mean the value it holds is immutable. Const requires an initial value.

```js
// ES5

```

```js
// ES2015

```

[MDN const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)

Caveats

### You do:

Replace var with let and const

[Solution](https://github.com/ga-wdi-exercises/react-omdb/commit/6405abfa36d333781d42d7cf54c6cc285255897f)

# Concise Object Methods / Object literals

'Concise Object Methods' are a new syntax for writing functions with objects that allow you to skip writing ': function'

```js
// ES5
var doStuff = {
  sayHello: function() {
    console.log("Hello");
  },
  eatPizza: function(pizza) {
    pizza = 0;
    return "thanks!"
  }
}
```

```js
// ES2015
let doStuff = {
  sayHello() {
    console.log("Hello");
  },
  eatPizza(pizza) {
    pizza = 0;
    return "thanks!"
  }
}
```

### You do:

Convert all object methods to use new syntax by removing `: function`

[Solution](https://github.com/ga-wdi-exercises/react-omdb/commit/e089b6f6a697c02ca2d1f3e5dd88e10ea2404b41)

# Arrows

According to [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) the two biggest motivations for arrows were **shorter functions** and **lexical `this`**. Arrows can make your code cleaner and more intuitive.

### Exercise - You do!
1. Go to the [Babel REPL](https://babeljs.io/repl/).
2. Paste in the code below:

```js
var add = (x , y) => x + y;
```

What did you notice? Compare the syntax between ES6 (left) and ES5 (right).

---

Arrows save having to write `function` & `return` for simple functions like this. When only one variable is used, the parenthesis can also be omitted `x => x * x`, I always keep the parens for consistency.

Arrows can be used as [IIFEs](http://benalman.com/news/2010/11/immediately-invoked-function-expression/). To return object literals the object needs to be wrapped in parenthesis. Arrows are anonymous by default, or can be assigned to a variable just like normal functions.
- Paste the code below into the [Babel REPL](https://babeljs.io/repl/).

```js
(() => "I'm an IIFE")() //IIFE

var foobar = () => ({foo: 'bar'}) // Object Literal
```

> **Solution / Easy reference** - Arrow's IIFE and returning object literals syntax [transpiled to  ES5](https://goo.gl/qD64JO).

Arrows can also be used with block statements It should be noted that in blocks the `return` is not automatic and needs to be explicitly stated. This still saves having to write `function`.
- Paste the code below into the [Babel REPL](https://babeljs.io/repl/).

```js
$('#pizza-btn').click( (event) => {
  preheatOven();
  pizzaInOven();
  return 'I love za!'
});
```

> **Solution / Easy reference** - ES6 arrows with blocks [transpiled to ES5](https://goo.gl/nSfBOH):

Shorter syntax is awesome, especially because I'm always misspelling `funciton`.

### Arrows: Lexical This
The rest of the section assumes you are comfortable with scope, context, and closures in javascript. Detailed reviews of these concepts can be found [here](https://toddmotto.com/everything-you-wanted-to-know-about-javascript-scope/), [here](http://ryanmorr.com/understanding-scope-and-context-in-javascript/), and [here](http://javascriptissexy.com/understand-javascripts-this-with-clarity-and-master-it/). The TL;DR:  **Context** refers to the object that the currently executing function is attached to - determined in most cases by how a function is called. The value of `this` references whatever the current context is. **Scope** is where a variable can be referenced (where it's defined).

Where arrows really shine is by binding `this`. The `this` in an arrow  is set to the this value of the enclosing execution context.

Here we have a function, in this case an Object constructor function. Inside our function is a method that executes a callback function. We want the ovenTemp of the Oven to increase every 1000 ms.

```js
function Oven () {
  this.ovenTemp = 0; // here `this` references instance of Oven constructor

  setInterval(function preheatOven() {
    // preheatOven() redefines context of `this`
    this.ovenTemp++;  // here `this` references the global object, undesired action
  }, 1000);
}
```

What does `this` refer to in both spots?
- The Oven.ovenTemp doesn't change because our callback changed the context of `this`.

How can we fix the problem?
- One way we can remedy the problem by [binding](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) `this`.

```js
function Oven () {
  this.ovenTemp = 0; // here `this` references instance of Oven constructor

  setInterval(function preheatOven() {
    this.ovenTemp++;  
  }, 1000).bind(this);
}
```

Another, more performant, way to fix the problem is to store the top level `this` reference/context in a variable, commonly seen as `self = this` or `that = this`.

```js
function Oven () {
  var self = this; // stores top level `this` reference
  self.ovenTemp = 0;

  setInterval(function preheatOven() {
    self.ovenTemp++; // performs desired action
  }, 1000);
}
```

### Arrows to the rescue!
A better solution is to leverage the lexical scoping of arrow functions.

```js
function Oven () {
  this.ovenTemp = 0;

  setInterval( preheatOven = () => {
    this.ovenTemp++;  // here `this` references instance of Oven constructor, as desired
  }, 1000);
}
```

In this particular example we can use an anonymous callback and refactor to one line. Wow that's pretty!

```js
function Oven () {
  this.ovenTemp = 0;
  setInterval( () => { this.ovenTemp++; }, 1000);
}
```

This trivial example shows a non trivial use case - arrows can simplify callbacks and/or closures and allow for more intuitive use of context.

### You do:

Convert remaining functions to arrow syntax

[Solution](https://github.com/ga-wdi-exercises/react-omdb/commit/fc83ca87348cc370182faaef7e6a92f32e4d799b)

# Destructing

Destructing is a new syntax that makes it easier to extract data from objects and arrays. Destructing matches patterns to assign values to variables.

```js
// ES2015
let [a, b] = ["cats", "dogs"]

console.log(a); // "cats"
console.log(b); // "dogs"

let x = {y: "pizza", z: 100}
let {y, z} = x

console.log(y); // "pizza"
console.log(z); // 100
```

For destructing objects the assigned name needs to match the keys of the object

```js
// ES2015
let x = {y: "pizza", z: 100}
let {foo, bar} = x // not useful: neither foo or bar are in 'x'

console.log(foo); // undefined
console.log(bar); // undefined
```

### Learn more
- nested object and array destructing
- combine with default parameters
- combine with default spread
- combine with iteration

[MDN Destructing](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

# Modules / Loaders

Modules are just self contained sections of code. They make it easier to break a  big system into smaller pieces. Prior to ES2015 there was no standard way of creating modules, some patterns use IIFEs, CommonJS syntax (the one used in node), AMD modules, plus more.


Here's an example of the CommonJS syntax you have seen in node:
```js
// ES5

// someFile
function sayHello() {
  console.log("Hello");
}

var cats = "cats"

module.exports = {
  cats: cats,
  sayHello: sayHello
};

////////////////////
// differentFile
var pizza = require("whatever/path/to/someFile");

pizza.sayHello()
// > "Hello"
```

You can export multiple items and are able to specify exactly what you want to import.
```js
// ES2015

// someFile
export function sayHello() {
  console.log("Hello");
}

export const cats = "cats"

////////////////////
// differentFile
import sayHello from "whatever/path/to/someFile"

sayHello()
// > "Hello"
```

#### Learn more:
- importing everything from a module
- default exports and imports
- aliasing imports

### You do:

Convert imports to ES2015 syntax

[Solution](https://github.com/ga-wdi-exercises/react-omdb/commit/a6a59fbc06d05c4a2525c435f32528493a909e6b) (disregard the updates to .babelrc and package.json, you should have already done that)

# Classes


```js
// ES5
function Pizza(name, temperature) {
  this.name = name;
  this.temperature = temperature
}

Pizza.prototype.heatUp = function () {
  return this.temperature + 20;
};
```

---

Classes provided syntactic sugar over JavaScript's existing prototype-based inheritance. A class body can only methods and not data properties. Putting data in prototypes is generally an anti-pattern, so this enforces a best practice. Data is instead attached to classes using the `constructor` method. Instance methods (defined with concise object syntax) are automatically connected through prototypical inheritance.

```js
// ES2015
class Pizza {
  constructor(name, temperature) {
    this.name = name;
    this.temperature = temperature;
  }
  static sayCool() {
    console.log("cool!");
  }
  heatUp() {
    return this.temperature + 20;
  }
}
```

The `static` keyword defines a static method for a class. Static methods are called without instantiating their class and are also not callable when the class is instantiated. Static methods are often used to create utility functions for an application.
[MDN static](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static)

```js
// ES2015
Pizza.sayCool();
// > "cool!"
let za = new Pizza("cheese", "hot")

za.sayCool();
// TypeError: za.sayCool is not a function
```

### Extends
The `extends` keyword is used in a class declaration (or class expression) to create a class with a child of another class. [MDN extends](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/extends)

The `super` keyword is used to call functions on an objects parent. [MDN extends](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/super)

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes
http://www.2ality.com/2015/02/es6-classes-final.html

```js
// ES2015
class JumboSlice extends Pizza {
  constructor(name, temperature) {
    super(cheese, lavaHot); // calls the constructor of the class being extended
    this.size = "jumbo";
  }
  cheesyRegret() {
    return "penitent stomach"
  }
}
```

### You do:

Update to use classes

We will no longer be using `React.createClass` and are instead creating classes with the new syntax that extend `React.Component`.

1. For the components that use
  `const <ComponentName> = React.createClass({` and just contain `render() {`
  update them to `class <ComponentName> extends React.Component{`.
  This also involves removing the parenthesis that came from `createClass()`
2. `React.createClass` automatically bound `this` to the methods to keep context when the methods are called we can use arrow functions:
  - Update
  ```js
  onUpdateSearch={this.handleUpdateSearch}
  onSubmitSearch={this.handleSubmitSearch}
  ```
  to:
  ```js
  onUpdateSearch={(event) => this.handleUpdateSearch(event)}
  onSubmitSearch={(event) => this.handleSubmitSearch(event)}
  ```
3. Update the `SearchContainer` component to also use class syntax.
  - Remove the commas between methods
  - change `getInitialState()` to `constructor()`
  - include `super()` to call the constructor of React.Component (the thing we're extending)
  ```js
  -  getInitialState() {
  -    return {
  +  constructor() {
  +    super();
  +    this.state = {
  ```

[Solution](https://github.com/ga-wdi-exercises/react-omdb/commit/0d736321d7d803d21e286c62f618633090facd93)

#### Learn more:
- class expressions vs class declarations
- getters and setters
- mixins
- class hoisting

# Bonus
### new static methods - Math, Number, String, Array, Object

[Math, Number, String, Array, Object](https://github.com/lukehoban/es6features#math--number--string--array--object-apis)

### Symbols

A symbol is a unique and immutable data type and may be used as an identifier for object properties. The Symbol object is an implicit object wrapper for the symbol primitive data type.

[MDN Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)

### Default Parameters

Default function parameters allow formal parameters to be initialized with default values if no value or undefined is passed.

```js
// ES2015
function multiply(a, b = 2) {
  return a*b;
}

multiply(5); // 10
```

[MDN default parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)

### Spread

```js
var parts = ['shoulders', 'knees'];
var lyrics = ['head', ...parts, 'and', 'toes']; // ["head", "shoulders", "knees", "and", "toes"]
```
[MDN Spread](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator)

### Rest parameters

The rest parameter syntax allows us to represent an indefinite number of arguments as an array. If the last named argument of a function is prefixed with `...`, it becomes an array with the remaining arguments passed to the function.

```js
// ES2015
function(a, b, ...theArgs) {
  // ...
}
```

[MDN Rest Parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)

### Promises

Offical support for promises with an api that is similar to ones that already exist in many libraries.

```js
// ES2015
function timeout(duration = 0) {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, duration);
    })
}

var p = timeout(1000).then(() => {
    return timeout(2000);
}).then(() => {
    throw new Error("hmm");
}).catch(err => {
    return Promise.all([timeout(100), timeout(200)]);
})
```

[Promise example](https://github.com/lukehoban/es6features#promises)

### Iterators

```js
// ES2015
let iterable = [10, 20, 30];

for (let value of iterable) {
  console.log(value);
}
// 10
// 20
// 30
```

[MDN iterators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)

### Map / Set / WeakMap / WeakSet

Data structures that make it easier to complicated things.

```js
// ES2015

// Sets
var s = new Set();
s.add("hello").add("goodbye").add("hello");
s.size === 2;
s.has("hello") === true;

// Maps
var m = new Map();
m.set("hello", 42);
m.set(s, 34);
m.get(s) == 34;

// Weak Maps
var wm = new WeakMap();
wm.set(s, { extra: 42 });
wm.size === undefined

// Weak Sets
var ws = new WeakSet();
ws.add({ data: 42 });
```

[Map / Set / WeakMap / WeakSet Example](https://github.com/lukehoban/es6features#map--set--weakmap--weakset)

### Generators

Make it easier to use `function*` and `yield`

[MDN iteration protocols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)

### Proxies

The Proxy object is used to define custom behavior for fundamental operations (e.g. property lookup, assignment, enumeration, function invocation, etc).

[MDN Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

### + probably some more

- [ES2015 features and compatibility](http://kangax.github.io/compat-table/es6/)
- [MDN list of Firefox compatibile features with links to docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_6_support_in_Mozilla)
