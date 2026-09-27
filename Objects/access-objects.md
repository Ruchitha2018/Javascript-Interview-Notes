# Different ways to access object properties

There are **2 primary ways** to access object properties in JavaScript.

### 1. Dot notation

Use dot notation when the property name is a valid JavaScript identifier and is known beforehand.

```js
const user = {
  name: "John",
  age: 30
};

console.log(user.name); // John
console.log(user.age);  // 30
```

---

### 2. Bracket notation

Use bracket notation when the property name is **dynamic**, contains special characters/spaces, or isn't a valid identifier.

```js
const user = {
  name: "John",
  "first-name": "John",
  age: 30
};

console.log(user["name"]);       // John
console.log(user["first-name"]); // John
```

#### Dynamic property access

This is one of the most important differences:

```js
const key = "name";

console.log(user[key]); // John
```

With dot notation:

```js
console.log(user.key);
```

JavaScript looks for a property literally named `"key"` rather than using the value of the variable.

---

### 3. Optional chaining

For safely accessing nested properties that might not exist:

```js
const user = {
  profile: {
    name: "John"
  }
};

console.log(user.profile?.name);
// John

console.log(user.address?.city);
// undefined
```

Without optional chaining:

```js
console.log(user.address.city);
// TypeError
```

You can also combine it with brackets:

```js
console.log(user["profile"]?.["name"]);
```

---

### 4. Destructuring

Destructuring is another way to extract/access properties:

```js
const user = {
  name: "John",
  age: 30
};

const { name, age } = user;

console.log(name); // John
console.log(age);  // 30
```

### Quick comparison

| Method            | Example                 | Best for                             |
| ----------------- | ----------------------- | ------------------------------------ |
| Dot notation      | `user.name`             | Known, valid property names          |
| Bracket notation  | `user["name"]`          | Dynamic/special property names       |
| Optional chaining | `user.profile?.name`    | Safely accessing nested properties   |
| Destructuring     | `const { name } = user` | Extracting properties into variables |

**Interview answer:** The standard ways are **dot notation and bracket notation**. Optional chaining and destructuring are useful language features for safely accessing or extracting properties.
