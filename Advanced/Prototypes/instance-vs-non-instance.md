# Instance Vs Non-Instance

### 1. Instance properties

Define them using `this` inside the constructor:

```js
class User {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

const user1 = new User("John", 30);
const user2 = new User("Mike", 25);
```

Here:

```text
user1 → name, age
user2 → name, age
```

Each instance has its **own properties**:

```js
console.log(user1.name); // John
console.log(user2.name); // Mike
```

You can also use **class fields**:

```js
class User {
  role = "user";

  constructor(name) {
    this.name = name;
  }
}
```

`role` and `name` are instance properties.

---

### 2. Static properties — non-instance properties

If you want a property to belong to the **class itself**, use `static`:

```js
class User {
  static type = "USER";

  constructor(name) {
    this.name = name;
  }
}
```

Access it through the class:

```js
console.log(User.type);
// USER
```

Not through an instance:

```js
const user = new User("John");

console.log(user.type);
// undefined
```

So:

```text
User.type        → static/class property
user.name        → instance property
```

---

### 3. Prototype properties

You can also put properties on the prototype:

```js
class User {
  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log("Hello", this.name);
  }
}
```

`name` is an **instance property**:

```js
user.hasOwnProperty("name");
// true
```

But `greet` is on the prototype:

```js
user.hasOwnProperty("greet");
// false

User.prototype.hasOwnProperty("greet");
// true
```

Conceptually:

```text
User
 │
 ├── static properties
 │
 └── prototype
       │
       └── greet()

user1
 ├── name
 └── [[Prototype]] ──> User.prototype

user2
 ├── name
 └── [[Prototype]] ──> User.prototype
```

### Summary

| Property type                 | Defined on        | Example                | Shared? |
| ----------------------------- | ----------------- | ---------------------- | ------- |
| **Instance property**         | Instance          | `this.name`            | ❌ No    |
| **Static property**           | Class/constructor | `static count`         | ✅ Yes   |
| **Prototype property/method** | Prototype         | `User.prototype.greet` | ✅ Yes   |

### Interview answer

> **Instance properties are defined on each object, typically using `this` in a constructor or class field. Static properties belong to the class/constructor and are accessed through the class, while prototype properties are shared through the prototype chain.**
