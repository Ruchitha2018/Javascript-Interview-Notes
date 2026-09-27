# Define Properties in Functions

Yes. **Functions are objects in JavaScript**, so you can define properties on them just like other objects.

### 1. Directly adding a property

```js
function greet() {
  console.log("Hello");
}

greet.language = "JavaScript";
greet.version = 1;

console.log(greet.language);
// JavaScript

console.log(greet.version);
// 1
```

Here, `greet` is both:

* A **function** that can be called: `greet()`
* An **object** that can have properties: `greet.language`

---

### 2. Using `Object.defineProperty()`

You can also define properties with descriptors:

```js
function greet() {
  console.log("Hello");
}

Object.defineProperty(greet, "language", {
  value: "JavaScript",
  writable: false,
  enumerable: true,
  configurable: false
});

console.log(greet.language);
// JavaScript
```

---

### 3. Functions can have methods too

```js
function counter() {
  console.log("counter");
}

counter.count = 0;

counter.increment = function () {
  counter.count++;
};

counter.increment();
counter.increment();

console.log(counter.count);
// 2
```

This pattern is sometimes used to maintain state associated with a function.

### Important distinction

```js
function Person() {}

Person.prototype.name = "John";
```

`Person.prototype` is a **property of the function object**.

```js
console.log(Person.prototype);
```

So functions have their own properties such as:

```text
Person
 ├── name
 ├── length
 ├── prototype
 └── ...
```

### Interview Answer

> **Yes. Functions are first-class objects in JavaScript, so they can have properties and methods. We can add properties using dot/bracket notation or `Object.defineProperty()`.**
