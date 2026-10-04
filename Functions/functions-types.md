## 1. First-Class / First-Order Functions


### First-class functions

JavaScript treats functions as **first-class values**. This means functions can be:

* Assigned to variables
* Passed as arguments
* Returned from other functions
* Stored in objects/arrays

```js
const greet = function () {
  return "Hello";
};

function execute(fn) {
  return fn();
}

execute(greet);
```

Here, `greet` is assigned to a variable and passed as an argument.

> **Interview:** JavaScript has first-class functions.

### First-order function

A **first-order function** is a function that **does not take functions as arguments and does not return a function**.

```js
function add(a, b) {
  return a + b;
}
```

It operates only on ordinary values.

This is different from a **higher-order function (HOF)**, which takes a function as an argument or returns a function.

```js
function multiplyBy(factor) {
  return function (value) {
    return value * factor;
  };
}
```


## 2. Higher-Order Function (HOF)

A function is a **higher-order function** if it:

1. Takes one or more functions as arguments, **or**
2. Returns a function.

### Takes a function

```js
function calculate(a, b, operation) {
  return operation(a, b);
}

calculate(10, 20, (a, b) => a + b);
// 30
```

`calculate()` is a HOF because it accepts `operation`, which is a function.

### Returns a function

```js
function multiplier(x) {
  return function (y) {
    return x * y;
  };
}

const double = multiplier(2);

double(5);
// 10
```

### Common JavaScript HOFs

```js
[1, 2, 3].map(x => x * 2);

[1, 2, 3].filter(x => x > 1);

[1, 2, 3].reduce((sum, x) => sum + x, 0);
```



## 3. Unary Function

A **unary function** is a function that accepts **exactly one argument**.

```js
function square(x) {
  return x * x;
}

square(5);
// 25
```

Another example:

```js
const double = x => x * 2;
```

Here `double` is unary.

### Comparison

```js
function zero() {}          // 0 arguments
function unary(a) {}        // 1 argument
function binary(a, b) {}    // 2 arguments
function ternary(a, b, c) {} // 3 arguments
```


## 4. Pure Function

A **pure function** has two important properties:

### 1. Same input → same output

```js
function add(a, b) {
  return a + b;
}

add(2, 3); // 5
add(2, 3); // 5
```

### 2. No side effects

It doesn't modify external state, perform I/O, modify arguments, etc.

```js
let count = 0;

function increment() {
  count++; // modifies external state
}
```

`increment()` is **not pure**.

### Pure example

```js
function multiply(a, b) {
  return a * b;
}
```

### Impure example

```js
let tax = 0.18;

function calculateTax(price) {
  return price * tax;
}
```

If `tax` can change externally, the same input may produce different results.

Another common example:

```js
function addItem(items, item) {
  items.push(item); // mutates input
  return items;
}
```

This has a side effect because it mutates the original array.

A pure alternative:

```js
function addItem(items, item) {
  return [...items, item];
}
```



## 5. IIFE

**IIFE = Immediately Invoked Function Expression**

It is a function expression that is **created and immediately executed**.

```js
(function () {
  console.log("Hello");
})();
```

Output:

```text
Hello
```

Another syntax:

```js
(() => {
  console.log("Hello");
})();
```

### Why use IIFE?

Historically, IIFEs were commonly used to create a **private scope** and avoid polluting the global scope.

```js
(function () {
  const secret = "private";

  console.log(secret);
})();

console.log(secret);
// ReferenceError
```

The `secret` variable exists only inside the IIFE.

Before ES modules and `let`/`const` became common, IIFEs were frequently used for this purpose.

---

## Quick Interview Comparison

| Concept                   | Meaning                                      | Example               |
| ------------------------- | -------------------------------------------- | --------------------- |
| **First-class function**  | Functions can be treated as values           | `const fn = () => {}` |
| **First-order function**  | Doesn't accept/return functions              | `add(a, b)`           |
| **Higher-order function** | Accepts or returns a function                | `map()`, `filter()`   |
| **Unary function**        | Takes exactly one argument                   | `x => x * 2`          |
| **Pure function**         | Same input → same output, no side effects    | `add(a, b)`           |
| **IIFE**                  | Function executed immediately after creation | `(() => {})()`        |

### Easy way to remember

```text
First-class → Functions are values

First-order → Doesn't deal with functions

Higher-order → Takes/returns functions

Unary → Takes 1 argument

Pure → No side effects + deterministic

IIFE → Executes immediately
```
