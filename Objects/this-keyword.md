# this keyword

`this` is a special keyword that refers to the **object associated with the current function invocation**.

The most important rule:

> **The value of `this` is determined by how a function is called, not where the function is defined** — with an important exception: **arrow functions don't have their own `this`; they capture it lexically.**



## 1. Global Context

In a browser's classic script:

```js
console.log(this);
// window
```

In an ES module:

```js
console.log(this);
// undefined
```

So the value depends on the execution environment and whether the code is a script or module.



## 2. Object Method

When a function is called as an object method, `this` refers to the object before the dot.

```js
const user = {
  name: "John",

  greet() {
    console.log(this.name);
  }
};

user.greet();
// John
```

Here:

```text
user.greet()
    ↑
  this = user
```



## 3. Standalone Function

In strict mode:

```js
"use strict";

function greet() {
  console.log(this);
}

greet();
// undefined
```

In a non-strict browser script, a standalone regular function call can have `this === window`.



## 4. Constructor with `new`

When a function is called with `new`, `this` refers to the **newly created object**.

```js
function User(name) {
  this.name = name;
}

const user = new User("John");

console.log(user.name);
// John
```

Conceptually:

```text
new User("John")
       ↓
new object created
       ↓
this → new object
       ↓
this.name = "John"
```



## 5. `call()`

`call()` allows you to explicitly specify `this`.

```js
function greet() {
  console.log(this.name);
}

const user = {
  name: "John"
};

greet.call(user);
// John
```

Arguments are passed individually:

```js
function add(a, b) {
  return this.value + a + b;
}

const obj = { value: 10 };

add.call(obj, 2, 3);
// 15
```



## 6. `apply()`

`apply()` is similar to `call()`, but arguments are passed as an array-like value.

```js
add.apply(obj, [2, 3]);
// 15
```

### `call()` vs `apply()`

```text
call(obj, arg1, arg2)

apply(obj, [arg1, arg2])
```



## 7. `bind()`

`bind()` creates a **new function** with `this` permanently bound to the specified object.

```js
const user = {
  name: "John"
};

function greet() {
  console.log(this.name);
}

const boundGreet = greet.bind(user);

boundGreet();
// John
```

Important:

```js
bind()
```

**doesn't immediately execute the function.**



## Arrow Functions and `this`

Arrow functions behave differently.

They **do not have their own `this`**.

Instead, they capture `this` from their surrounding lexical scope.

```js
const user = {
  name: "John",

  greet() {
    const inner = () => {
      console.log(this.name);
    };

    inner();
  }
};

user.greet();
// John
```

The arrow function gets `this` from `greet()`.



## Common Interview Trap

```js
const user = {
  name: "John",

  greet: () => {
    console.log(this.name);
  }
};

user.greet();
```

This does **not** make `this` refer to `user`.

Why?

Because the arrow function doesn't create its own `this`.

For object methods, use:

```js
const user = {
  name: "John",

  greet() {
    console.log(this.name);
  }
};
```



## `this` in Event Handlers

With a regular function used as a DOM event handler:

```js
button.addEventListener("click", function () {
  console.log(this);
});
```

`this` generally refers to the element on which the handler was registered.

With an arrow function:

```js
button.addEventListener("click", () => {
  console.log(this);
});
```

`this` is inherited from the surrounding scope instead.


## `this` in Classes

Inside a class method, `this` normally refers to the instance when called as a method.

```js
class User {
  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log(this.name);
  }
}

const user = new User("John");

user.greet();
// John
```

But be careful when extracting the method:

```js
const greet = user.greet;

greet();
// this is undefined in strict mode
```

You can bind it:

```js
const greet = user.greet.bind(user);

greet();
// John
```


## `this` Quick Reference

| Invocation                            | `this`                     |
| ------------------------------------- | -------------------------- |
| `obj.method()`                        | `obj`                      |
| `func()` in strict mode               | `undefined`                |
| `func()` in non-strict browser script | `window`                   |
| `new Func()`                          | Newly created object       |
| `func.call(obj)`                      | `obj`                      |
| `func.apply(obj)`                     | `obj`                      |
| `func.bind(obj)`                      | Bound to `obj`             |
| Arrow function                        | Lexically inherited `this` |

### Interview Answer

> **`this` refers to the value associated with the current function invocation. For regular functions, its value is determined by how the function is called. It can be set through method invocation, `new`, `call`, `apply`, or `bind`. Arrow functions are different because they don't have their own `this`; they lexically inherit it from the surrounding scope.**
