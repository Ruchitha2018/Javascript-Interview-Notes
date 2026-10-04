# How do you create an object with a prototype ?

In JavaScript, you can create an object with a specific prototype using **`Object.create()`**.

### 1. Using `Object.create()`

```js
const personPrototype = {
  greet() {
    console.log(`Hello, ${this.name}`);
  }
};

const person = Object.create(personPrototype);

person.name = "John";

person.greet(); // Hello, John
```

Here:

```text
person
  ↓ [[Prototype]]
personPrototype
```

So `person` doesn't have its own `greet()` method. JavaScript finds it through the **prototype chain**.

---
### 2. Verify the prototype

```js
Object.getPrototypeOf(person) === personPrototype;
// true
```
---
### 3. Using a constructor function

Another common way is:

```js
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function () {
  console.log(`Hello, ${this.name}`);
};

const person = new Person("John");

person.greet();
```

With `new`, JavaScript automatically creates the object and sets:

```js
Object.getPrototypeOf(person) === Person.prototype;
// true
```
---
### Interview point

**`Object.create(proto)` creates a new object whose internal `[[Prototype]]` is set to `proto`.**

```js
const obj = Object.create(proto);
```

This is different from:

```js
const obj = {};
```

because `{}` normally gets its prototype from `Object.prototype`.
