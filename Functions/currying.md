# Currying Functions

**Currying** is the technique of converting a function that takes **multiple arguments** into a sequence of functions where each function takes **one argument**.

### Normal function

```js
function add(a, b, c) {
  return a + b + c;
}

add(1, 2, 3);
// 6
```

### Curried function

```js
function add(a) {
  return function (b) {
    return function (c) {
      return a + b + c;
    };
  };
}

add(1)(2)(3);
// 6
```

Think of it as:

```text
add(1)
  ↓
function(b)
  ↓
function(c)
  ↓
result
```


## Using Arrow Functions

The same example can be written more concisely:

```js
const add = a => b => c => a + b + c;

console.log(add(1)(2)(3));
// 6
```



## Why is Currying useful?

### 1. Function Reusability

You can create a specialized function by supplying some arguments early.

```js
const multiply = a => b => a * b;

const multiplyBy10 = multiply(10);

console.log(multiplyBy10(5));
// 50

console.log(multiplyBy10(8));
// 80
```

Here:

```text
multiply(10)
     ↓
multiplyBy10
     ↓
accepts the remaining argument
```

This is closely related to **partial application**.



## Currying vs Partial Application

These are often confused.

### Currying

Transforms:

```js
f(a, b, c)
```

into:

```js
f(a)(b)(c)
```

### Partial application

Fixes some arguments and returns a function for the remaining arguments:

```js
const add = (a, b, c) => a + b + c;

const add10 = (b, c) => add(10, b, c);

add10(2, 3);
// 15
```

So:

> **Currying changes the function's argument structure, while partial application fixes some arguments.**



## Generic Curry Function

In interviews, you may be asked to implement a generic `curry()`:

```js
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn(...args);
    }

    return (...nextArgs) => {
      return curried(...args, ...nextArgs);
    };
  };
}
```

Usage:

```js
function add(a, b, c) {
  return a + b + c;
}

const curriedAdd = curry(add);

console.log(curriedAdd(1)(2)(3));
// 6
```

It can also support multiple arguments at each step:

```js
curriedAdd(1, 2)(3);
// 6

curriedAdd(1)(2, 3);
// 6
```

### Interview Answer

> **Currying is a functional programming technique that transforms a function accepting multiple arguments into a sequence of functions, each accepting arguments incrementally. It enables partial application and function composition and can help create reusable specialized functions.**
