# Do all objects have a prototype?

No. **Not all JavaScript objects have a prototype.**

Most ordinary objects do, but you can explicitly create an object with **no prototype** using `Object.create(null)`.

### 1. Normal object

```js
const user = {
  name: "John"
};

console.log(Object.getPrototypeOf(user) === Object.prototype);
// true
```

Prototype chain:

```text
user
 ↓
Object.prototype
 ↓
null
```
---
### 2. Object with no prototype

```js
const user = Object.create(null);

user.name = "John";

console.log(Object.getPrototypeOf(user));
// null
```

Its prototype chain is:

```text
user
 ↓
null
```

Therefore, methods inherited from `Object.prototype` are not available:

```js
console.log(user.toString);
// undefined

console.log(user.hasOwnProperty);
// undefined
```

You can still safely check properties with:

```js
Object.hasOwn(user, "name");
// true
```
---
### What about `Object.prototype`?

```js
Object.getPrototypeOf(Object.prototype);
// null
```

So `Object.prototype` itself has **no prototype**.

### Functions

Functions are objects too, and normally have prototypes in their prototype chain:

```js
function greet() {}

console.log(Object.getPrototypeOf(greet) === Function.prototype);
// true
```

But don't confuse:

```js
Object.getPrototypeOf(greet)
```

with:

```js
greet.prototype
```

* `Object.getPrototypeOf(greet)` → the prototype of the **function object**
* `greet.prototype` → the object used as the prototype for objects created with `new greet()`

### Interview Answer

> **No. Most JavaScript objects inherit from a prototype, but an object can have no prototype using `Object.create(null)`. `Object.prototype` is also an object whose prototype is `null`.**
