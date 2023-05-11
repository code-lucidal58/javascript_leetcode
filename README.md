# JavaScript Leetcode
Leetcode started LC Javascript Challenge in May 2023. This is the list of all problems that were asked each day during the time.
* [Day 1](#day-1)
* [Day 2](#day-2)

## Day 1
Code: [Create Hello World Function](./create_hello_world_function.js)

### Functions
Function can be defined using the word `function`.
#### Basic Syntax
```
function f(a, b) {
  const sum = a + b;
  return sum;
}
console.log(f(3, 4)); // 7
```

#### Anonymous Syntax
```
var f = function(a, b) {
  const sum = a + b;
  return sum;
}
console.log(f(3, 4)); // 7
```

#### Immediately Invoked Function Expression (IIFE)
```
const result = (function(a, b) {
  const sum = a + b;
  return sum;
})(3, 4);
console.log(result); // 7
```

#### Functions with Functions
```
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
```
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
```
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
```
const f = (a, b) => {
  const sum = a + b;
  return sum;
};
console.log(f(3, 4)); // 7
```

#### Omit Return
If function statement is very small, then `return` statement can be omitted.
```
const f = (a, b) => a + b;
console.log(f(3, 4)); // 7
```

### Rest Arguments
`Spread`(...) allows an iterable to be expanded inplace.
```
function f(...args) {
  const sum = args[0] + args[1];
  return sum;
}
console.log(f(3, 4)); // 7
```
This can be used to create generic function factory.
```
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
