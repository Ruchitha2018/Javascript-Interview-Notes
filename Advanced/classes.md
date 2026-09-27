# Classes

A **class** is a template for creating objects with shared properties and methods.

JavaScript classes are primarily **syntactic sugar over JavaScript's prototype-based inheritance**.

### Basic Class

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    return `Hello, I'm ${this.name}`;
  }
}

const person = new Person("John", 30);

console.log(person.name);  // John
console.log(person.greet()); // Hello, I'm John
```

### What happens with `new`?

```js
const person = new Person("John", 30);
```

Conceptually:

```text
1. A new object is created
        ↓
2. Person.prototype becomes its [[Prototype]]
        ↓
3. constructor() executes with `this` = new object
        ↓
4. The object is returned
```

So:

```js
Object.getPrototypeOf(person) === Person.prototype;
// true
```


## Constructor

The `constructor()` method initializes the object.

```js
class User {
  constructor(name) {
    this.name = name;
  }
}
```

You normally have **one `constructor()` per class**.



## Class Methods

Methods defined inside a class are placed on the class's prototype rather than copied onto every instance.

```js
class Person {
  greet() {
    console.log("Hello");
  }
}

const p1 = new Person();
const p2 = new Person();

console.log(p1.greet === p2.greet);
// true
```

This is because both instances find `greet` through:

```text
p1 ──→ Person.prototype
p2 ──→ Person.prototype
```



## Inheritance

Use `extends` to inherit from another class.

```js
class Animal {
  speak() {
    console.log("Animal speaks");
  }
}

class Dog extends Animal {
  bark() {
    console.log("Woof");
  }
}

const dog = new Dog();

dog.speak(); // Animal speaks
dog.bark();  // Woof
```

Prototype chain:

```text
dog
 ↓
Dog.prototype
 ↓
Animal.prototype
 ↓
Object.prototype
 ↓
null
```



## `super`

`super` is used to access the parent class.

```js
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} speaks`);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }

  speak() {
    super.speak();
    console.log("Woof!");
  }
}
```

Important:

> In a derived class constructor, you must call `super()` before using `this`.



## Static Methods

A `static` method belongs to the **class itself**, not its instances.

```js
class MathUtil {
  static add(a, b) {
    return a + b;
  }
}

console.log(MathUtil.add(2, 3));
// 5
```

But:

```js
const math = new MathUtil();

math.add(2, 3);
// ❌ TypeError
```



## Getters and Setters

Classes support accessors:

```js
class User {
  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name;
  }

  set name(value) {
    this._name = value;
  }
}

const user = new User("John");

console.log(user.name); // John

user.name = "Mike";

console.log(user.name); // Mike
```



## Private Fields

JavaScript supports private fields using `#`.

```js
class BankAccount {
  #balance = 0;

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount();

account.deposit(100);

console.log(account.getBalance());
// 100

console.log(account.#balance);
// ❌ SyntaxError
```

`#balance` can only be accessed from within the class.



## Class vs Constructor Function

Before ES6 classes, constructor functions were commonly used:

```js
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function () {
  console.log(`Hello ${this.name}`);
};
```

With a class:

```js
class Person {
  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log(`Hello ${this.name}`);
  }
}
```

Classes provide cleaner syntax, but JavaScript **still uses prototypes underneath**.

### Important Interview Points

* Classes were introduced in **ES6 / ES2015**.
* Classes are **not a completely new inheritance model**; they use prototypes.
* Class methods are generally stored on the **prototype**.
* `constructor()` initializes instances.
* `extends` provides inheritance.
* `super()` accesses the parent class.
* `static` members belong to the class, not instances.
* `#privateField` creates truly private class fields.
* Classes must be called with `new`.

### Interview Answer

> **A JavaScript class is a syntactic abstraction over the prototype-based object model. It provides a cleaner way to create objects and implement inheritance using features such as constructors, methods, `extends`, `super`, static members, getters/setters, and private fields.**
