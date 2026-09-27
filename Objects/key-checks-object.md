## How to check if a key exists in an object

There are several ways to check whether a property exists in a JavaScript object.

### 1. `Object.hasOwn()` — Recommended

- Checks whether the property is an **own property** of the object.
- It does NOT check the prototype chain.

```js
const user = {
  name: "John",
  age: 30
};

console.log(Object.hasOwn(user, "name"));
// true

console.log(Object.hasOwn(user, "email"));
// false
```

This is generally the preferred modern approach.

---

### 2. `hasOwnProperty()`

```js
console.log(user.hasOwnProperty("name"));
// true
```

However, calling it directly can be unsafe if the object has its own property named `hasOwnProperty`:

```js
const user = {
  hasOwnProperty: "something"
};

user.hasOwnProperty("name");
// ❌ TypeError
```

A safer form is:

```js
Object.prototype.hasOwnProperty.call(user, "name");
// true
```

---

### 3. `in` operator

The `in` operator checks the **entire prototype chain**, not just the object's own properties.

```js
const user = {
  name: "John"
};

console.log("name" in user);
// true

console.log("toString" in user);
// true
```

Why is `toString` true?

Because it is inherited from `Object.prototype`.

```text
user
  ↓ [[Prototype]]
Object.prototype
  └── toString
```

---

### 4. Checking with `undefined` ⚠️

You may see:

```js
if (user.name !== undefined) {
  // exists
}
```

But this is **not a reliable way** to check whether a key exists.

```js
const user = {
  name: undefined
};

console.log(user.name !== undefined);
// false
```

The property exists, but its value is `undefined`.

---

### Comparison

| Method                                   | Own property? | Prototype properties? | Recommended                       |
| ---------------------------------------- | ------------: | --------------------: | --------------------------------- |
| `Object.hasOwn(obj, key)`                |             ✅ |                     ❌ | ⭐ Yes                             |
| `obj.hasOwnProperty(key)`                |             ✅ |                     ❌ | Yes, with caution                 |
| `Object.prototype.hasOwnProperty.call()` |             ✅ |                     ❌ | Yes                               |
| `key in obj`                             |             ✅ |                     ✅ | When prototype chain should count |
| `obj[key] !== undefined`                 |             ❌ |                     — | ❌ Don't use for existence         |

### Interview Answer

> **Use `Object.hasOwn(object, key)` to check whether an object directly owns a property. Use the `in` operator when you also want to consider properties inherited through the prototype chain.**
