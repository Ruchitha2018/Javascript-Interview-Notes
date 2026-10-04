# JavaScript Accessors

**Accessors** are special object properties that allow you to control what happens when a property is **read** or **written**.

There are two types:

* **Getter (`get`)** → runs when a property is read.
* **Setter (`set`)** → runs when a property is assigned.

---

### 1. Getter

```js
const user = {
  firstName: "John",
  lastName: "Doe",

  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
};

console.log(user.fullName);
// John Doe
```

Notice that we access `fullName` like a normal property:

```js
user.fullName
```

not:

```js
user.fullName()
```

The getter executes automatically.

---
### 2. Setter

A setter runs when you assign a value:

```js
const user = {
  firstName: "John",

  set name(value) {
    this.firstName = value;
  }
};

user.name = "Mike";

console.log(user.firstName);
// Mike
```

---

### 3. Getter + Setter

A common pattern is to use an internal property:

```js
const user = {
  _name: "",

  get name() {
    return this._name;
  },

  set name(value) {
    if (value.length < 3) {
      throw new Error("Name is too short");
    }

    this._name = value;
  }
};

user.name = "John";

console.log(user.name);
// John
```

Here:

```text
user.name = "John"
      ↓
setter executes
      ↓
this._name = "John"


user.name
      ↓
getter executes
      ↓
returns this._name
```
---
### 4. Accessors in Classes

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
---
### 5. Important Interview Points

* `get` defines a **getter**.
* `set` defines a **setter**.
* Getters execute when a property is **read**.
* Setters execute when a property is **assigned**.
* Accessors look like normal properties to the caller.
* A getter can compute a value dynamically.
* A setter can perform **validation/transformation** before storing a value.
* Getters/setters can be defined in **objects, classes, and prototypes**.

**Simple definition for interviews:**

> JavaScript accessors are properties defined using getters and setters that allow us to customize the behavior of reading and writing object properties.
