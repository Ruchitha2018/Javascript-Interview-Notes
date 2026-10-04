# Anonymous Function

An **anonymous function** is a function that **does not have a name**.

### 1. Basic Example

```js
const greet = function () {
  console.log("Hello");
};

greet();
```

The function itself has no name:

```js
function () {
  console.log("Hello");
}
```

It is assigned to the variable `greet`.

---

### 2. Anonymous Function as a Callback

Anonymous functions are commonly used as callbacks:

```js
setTimeout(function () {
  console.log("Hello");
}, 1000);
```

Here, the function:

```js
function () {
  console.log("Hello");
}
```

is anonymous and passed directly to `setTimeout()`.

Another example:

```js
const numbers = [1, 2, 3];

numbers.map(function (num) {
  return num * 2;
});
```

---

### 3. Anonymous Arrow Function

Arrow functions are frequently used as anonymous functions:

```js
const numbers = [1, 2, 3];

numbers.map((num) => num * 2);
```

The arrow function:

```js
(num) => num * 2
```

has no explicit name.

---

### 4. Anonymous Function vs Named Function

**Named:**

```js
function greet() {
  console.log("Hello");
}
```

**Anonymous:**

```js
const greet = function () {
  console.log("Hello");
};
```

The first function has the name `greet`.

The second function expression itself doesn't explicitly have a name, although JavaScript can often infer its function `name` property from the variable it's assigned to:

```js
console.log(greet.name);
// "greet"
```

This is called **name inference**.

---

### 5. IIFE and Anonymous Functions

An anonymous function is often used with an IIFE:

```js
(function () {
  const secret = "private";

  console.log(secret);
})();
```

The function is created and immediately executed.

---

### Interview Answer

> **An anonymous function is a function without an explicit name. It is commonly used as a callback, assigned to a variable, or immediately invoked as an IIFE.**
