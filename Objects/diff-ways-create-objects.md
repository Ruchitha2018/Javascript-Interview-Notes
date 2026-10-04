# Different Ways to Create Objects in JavaScript

There are several ways to create objects in JavaScript. These are commonly asked in frontend interviews.

### 1. Object Literal

The simplest and most common way.

```js
const user = {
  name: "John",
  age: 30
};
```



### 2. `new Object()`

Using the built-in `Object` constructor:

```js
const user = new Object();

user.name = "John";
user.age = 30;
```

This is valid but generally less concise than an object literal.



### 3. `Object.create()`

Creates an object with a specified prototype.

```js
const personPrototype = {
  greet() {
    console.log("Hello");
  }
};

const user = Object.create(personPrototype);

user.name = "John";

user.greet();
```

You can also create an object with **no prototype**:

```js
const obj = Object.create(null);
```

---

### 4. Constructor Function

Before ES6 classes, constructor functions were commonly used.

```js
function User(name, age) {
  this.name = name;
  this.age = age;
}

const user = new User("John", 30);
```

Methods can be placed on the prototype:

```js
User.prototype.greet = function () {
  console.log(`Hello ${this.name}`);
};
```

---

### 5. ES6 Class

Classes provide cleaner syntax for constructor/prototype-based object creation.

```js
class User {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    console.log(`Hello ${this.name}`);
  }
}

const user = new User("John", 30);
```

The `greet()` method is stored on `User.prototype`.

---

### 6. Factory Function

A function that creates and returns an object.

```js
function createUser(name, age) {
  return {
    name,
    age,

    greet() {
      console.log(`Hello ${this.name}`);
    }
  };
}

const user = createUser("John", 30);
```

Unlike a constructor function, you don't need `new`.

---

### 7. Using `Object.assign()`

You can create an object by copying properties into a new object:

```js
const user = Object.assign(
  {},
  {
    name: "John",
    age: 30
  }
);
```

This is particularly useful for **merging objects**.

---

### 8. Using Spread Syntax

A modern way to create a new object from existing properties:

```js
const userInfo = {
  name: "John",
  age: 30
};

const user = {
  ...userInfo
};
```

This creates a **shallow copy**.

---

### Summary

| Method                   | Example                  | Typical use                   |
| ------------------------ | ------------------------ | ----------------------------- |
| **Object literal**       | `{ name: "John" }`       | Simple objects                |
| **`new Object()`**       | `new Object()`           | Built-in constructor          |
| **`Object.create()`**    | `Object.create(proto)`   | Prototype control             |
| **Constructor function** | `new User()`             | Constructor/prototype pattern |
| **Class**                | `new User()`             | Modern OOP syntax             |
| **Factory function**     | `createUser()`           | Object creation without `new` |
| **`Object.assign()`**    | `Object.assign({}, obj)` | Copy/merge                    |
| **Spread syntax**        | `{ ...obj }`             | Shallow copy/merge            |

### Interview Answer

> **Objects can be created using object literals, `new Object()`, `Object.create()`, constructor functions, ES6 classes, factory functions, `Object.assign()`, and object spread syntax. The most common approaches are object literals, classes/constructor functions, factory functions, and `Object.create()` when explicit prototype control is needed.**
