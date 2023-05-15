n=n+1# JavaScript Leetcode
Leetcode started LC Javascript Challenge in May 2023. This is the list of all problems that were asked each day during the time.
* [Day 1](#day-1)
* [Day 2](#day-2)
* [Day 3](#day-3)
* [Day 4](#day-4)
* [Day 5](#day-5)
* [Day 6](#day-6)
* [Day 7](#day-7)
* [Day 8](#day-8)
* [Day 9](#day-9)
* [Day 10](#day-10)

***Javascript Guide***: [MDN webdocs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
## Day 1
Code: [Create Hello World Function](./create_hello_world_function.js)

### Functions
Function can be defined using the word `function`.
#### Basic Syntax
```js
function f(a, b) {
  const sum = a + b;
  return sum;
}
console.log(f(3, 4)); // 7
```

#### Anonymous Syntax
```js
var f = function(a, b) {
  const sum = a + b;
  return sum;
}
console.log(f(3, 4)); // 7
```

#### Immediately Invoked Function Expression (IIFE)
```js
const result = (function(a, b) {
  const sum = a + b;
  return sum;
})(3, 4);
console.log(result); // 7
```

#### Functions with Functions
```js
function createFunction() {
  function f(a, b) {
    const sum = a + b;
    return sum;
  }
  return f;
}
const f = createFunction();
console.log(f(3, 4)); // 7
```

#### Function Hoisting
A function can be used before it is initialized. Only done when declaration of functions is with the `function` (basic) syntax. However, this is considered bad practice as it reduces readability.
```js
function createFunction() {
  return f;
  function f(a, b) {
    const sum = a + b;
    return sum;
  }
}
const f = createFunction();
console.log(f(3, 4)); // 7
```

#### Closures
When a function is created, it has access to a reference to all the variables declared around it (in the scope), also known as it's `lexical environment`. The combination of the function and its environment is called a `closure`.
```js
function createAdder(a) {
  function f(b) {
    const sum = a + b;
    return sum;
  }
  return f;
}
const f = createAdder(3);
console.log(f(4)); // 7
```
In this example, `createAdder` passes the first parameter `a` and the inner function has access to it. This way, `createAdder` serves as a factory of new functions, each time it is called with a different value of `a`. The returned function `f` will act differently each time.

### Arrow syntax
It is a preferred way of declaring a function. These cannot be used as constructors. Calling them with `new` will throw `TypeError`. They do not support `yield`, hence cannot be generator functions.
#### Basic Syntax
```js
const f = (a, b) => {
  const sum = a + b;
  return sum;
};
console.log(f(3, 4)); // 7
```

#### Omit Return
If function statement is very small, then `return` statement can be omitted.
```js
const f = (a, b) => a + b;
console.log(f(3, 4)); // 7
```

### Rest Arguments
`Spread`(...) allows an iterable to be expanded inplace.
```js
function f(...args) {
  const sum = args[0] + args[1];
  return sum;
}
console.log(f(3, 4)); // 7
```
This can be used to create generic function factory.
```js
function log(inputFunction) {
  return function(...args) {
     console.log("Input", args);
     const result = inputFunction(...args);
     console.log("Output", result);
     return result;
  }
}
const f = log((a, b) => a + b);
f(1, 2); // Logs: Input [1, 2] Output 3
```

## Day 2
Code: [Counter](./counter.js)

Discussed earlier, a function has reference to all variables inside it and any other variable/function in its outer scope. This is called lexical environment. This promotes `encapsulation`. Each time the outer function is called, a separate copy of the function statements and variables inside it is maintained.

## Day 3
Code: [Counter II](./counter_ii.js)

### JavaScript Objects
Objects are strings mapped to other objects. The string is called the key. The value can be any object, i.e., string, boolean, other object etc.
```js
const d = {
  "num": 1,
  "str": "Hello World",
  "obj": {
    "x": 5
  }
};
```
There are three ways of accessing the object items:
* **Dot Notation**: `d.obj.x // 5`
* **Bracket Notation**: `d["obj"]["x"] // 5`
* **Destructuring Notation**: `const {num, str} = d // 1 "Hello World"` This syntax is used to access multiple values in one go. The name of the variables should be same as the key name in the object. The variables can be declared using any keyword of variable declaration.

### Classes and Prototypes
```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    console.log("My name is", this.name);
  }
}

const alice = new Person("Alice", 25);
alice.greet(); // Logs: "My name is Alice"
```
This is an example of a class. The constructor creates an object. Every time an instance of the class is created, a copy of the function method is also created. An efficient way of the above can be having just a single copy of the class method. This is possible if the function is a `prototype`.
```js
const alice = {
  name: "Alice",
  age: 25,
  __proto__: {
    greet: function() {
      console.log("My name is", this.name);
    }
  }
};
alice.greet(); // Logs: "My name is Alice"
```
JavaScript will first search for the function `greet`. If it cannot find it, it will search it in the function's prototype. Then, it will search in the prototype's prototype, and so on. This is how **inheritance** works. Irrespective of the number of instances of the class are created, only single prototyped object is created.

### Proxies
Proxy is a powerful feature that allows overriding the default behaviour of objects. It allows you to create an object that can be used in place of the original object, but which may redefine fundamental Object operations like getting, setting, and defining properties.
```js
const alice = new Proxy({name: "Alice", age: 25}, {
  get: (target, key) => {
    if (key === 'greet') {
      return () => console.log("My name is", target.name);
    } else {
      return target[key];
    }
  },
});
alice.greet(); // Logs: "My name is Alice"
```
It has two parameters:
* **target**: the original object which you want to proxy
* **handler**: an object that defines which operations will be intercepted and how to redefine intercepted operations.

Some of the usages are as listed below:
* Perform validation
```js
const validator = {
  set: (obj, prop, value) => {
    if (prop === "age") {
      if (typeof value !== "number" || value < 0) {
        throw new TypeError("Age must be a positive number");
      }
    }
    obj[prop] = value;
  },
};

const person = new Proxy({}, validator);
person.age = 25; // Works fine
person.age = -5; // Throws an error
```
* Create logs when a key is accessed
```js
const object = {
  "num": 1,
  "str": "Hello World",
  "obj": {
    "x": 5
  }
};
const proxiedObject = new Proxy(object, {
  get: (target, key) => {
    console.log("Accessing", key);
    return target[key];
  }
});
proxiedObject.num; // Logs: Accessing num
```
* Throw error when an attempt to write a read-only value is made
```js
const READONLY_KEYS = ['name'];

const person = new Proxy({ name: "Alice", age: 25 }, {
  set: (target, key, value) => {
    if (READONLY_KEYS.includes(key)) {
      throw Error("Cannot write to key");
    }
    target[key] = value;
    return true;
  }
});
person.name = "Bob"; // Throws Error
```
* Create a modified version of an immutable object by writing to it's proxy. This is implemented with the popular library `immer`.

## Day 4
Code: [Apply Transform Over Each Element in Array](apply-transform-over-each-element-in-array.js)

Arrays can be declared and accessed in a number of ways.
```js
const newArr = [];
for (let i = 0; i < arr.length; ++i) {
    newArr[i] = i; // or newArr.push(i);
}

const newArr1 = new Array(newArr.length);https://www.w3schools.com/jsref/jsref_map.asp

const newArr = new Int32Array(arr.length);
```
### Callbacks
A callback is defined as a function passed as an argument to another function. It is used to write code that can be reused across different use-cases. One simple example is `Array.map`. It creates a new array from calling a function on every array element. It does not execute on empty elements and does not change the original array.
```js
// The following code squares each element of the array and create a new array with the result
const numbers = [4, 9, 16, 25];
const newArr = numbers.map(Math.sqrt);
```

## Day 5
Code: [Function Composition](function_composition.js)
TODO

## Day 6
Code: [Function Composition](function_composition.js)
TODO

## Day 7
Code: [Function Composition](function_composition.js)

`Function composition`is a concept in functional programming where the output of one function is used as the input of another function. In other words, it's the process of chaining two or more functions together so that the result of one function becomes the input to the next.
The notation (f ∘ g)(x) is used in mathematics to represent function composition. It is read as "f composed with g" or "f of g", or f(g(x)).

## Day 8
Code: [Function Composition](function_composition.js)
TDOD

## Day 9
Code: [Function Composition](function_composition.js)
TDOD

## Day 10
Code: [Function Composition](function_composition.js)
TDOD
