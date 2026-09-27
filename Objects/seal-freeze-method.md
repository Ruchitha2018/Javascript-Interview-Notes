 `seal()` vs `freeze()` 

## 1. `Object.seal()`

`Object.seal()` prevents:
- Adding new properties
- Deleting existing properties

Existing properties **can still be modified**.

```js
const user = {
  name: "John",
  age: 30
};

Object.seal(user);

user.name = "Jane";   // Allowed
user.email = "x";     // Not allowed
delete user.age;      // Not allowed
```


## 2. `Object.freeze()`

`Object.freeze()` is stricter. It prevents:
- Adding properties
- Deleting properties
- Modifying existing properties

```js
const user = {
  name: "John",
  age: 30
};

Object.freeze(user);

user.name = "Jane";  // Not allowed
user.email = "x";    // Not allowed
delete user.age;     // Not allowed
```


## 3. Main Difference

| Operation | `Object.seal()` | `Object.freeze()` |
|---|---|---|
| Add property | No | No |
| Delete property | No | No |
| Modify existing property | Yes | No |
| Object structure | Fixed | Fixed |
| Values | Can change | Cannot change |



## 4. Both Are Shallow

This is a common senior-level interview question.

```js
const user = {
  name: "John",
  address: {
    city: "Mumbai"
  }
};

Object.freeze(user);

user.name = "Jane";         // Not allowed
user.address.city = "Pune"; // Allowed
```

Why?

`Object.freeze()` is **shallow**. It freezes the outer object, but not nested objects.

```text
user
 ├── name       locked
 └── address    reference locked
       └── city     NOT locked
```


## 5. `Object.isSealed()`

Check whether an object is sealed:

```js
const user = { name: "John" };

Object.seal(user);

console.log(Object.isSealed(user));
// true
```


## 6. `Object.isFrozen()`

Check whether an object is frozen:

```js
const user = { name: "John" };

Object.freeze(user);

console.log(Object.isFrozen(user));
// true
```


## 7. Strict Mode Behavior

In strict mode, invalid modifications can throw a `TypeError`.

```js
"use strict";

const user = {
  name: "John"
};

Object.freeze(user);

user.name = "Jane";
// TypeError
```

Without strict mode, such modifications may fail silently.



## 8. `preventExtensions()` vs `seal()` vs `freeze()`

### `Object.preventExtensions()`

Prevents adding new properties, but existing properties can be modified or deleted.

```js
const user = {
  name: "John",
  age: 30
};

Object.preventExtensions(user);

user.name = "Jane";  // Allowed
delete user.age;     // Allowed
user.email = "x";    // Not allowed
```

### Comparison

| Operation | `preventExtensions()` | `seal()` | `freeze()` |
|---|---:|---:|---:|
| Add | No | No | No |
| Delete | Yes | No | No |
| Modify | Yes | Yes | No |

Easy memory trick:

```text
preventExtensions
→ Can't ADD

seal
→ Can't ADD or DELETE

freeze
→ Can't ADD, DELETE or MODIFY
```

## 9. Deep Freeze

If nested objects also need to be frozen:

```js
function deepFreeze(obj) {
  Object.freeze(obj);

  Object.values(obj).forEach(value => {
    if (
      value &&
      typeof value === "object" &&
      !Object.isFrozen(value)
    ) {
      deepFreeze(value);
    }
  });

  return obj;
}
```

Usage:

```js
const user = {
  name: "John",
  address: {
    city: "Mumbai"
  }
};

deepFreeze(user);

user.address.city = "Pune";
```

Now the nested object is also frozen.

> A production-grade deep-freeze utility may need to handle circular references, arrays, Maps, Sets, and other special objects.



## 12. Senior Interview Cheat Sheet

```text
Object.preventExtensions()
        ↓
Cannot ADD
        ↓
Can DELETE
        ↓
Can MODIFY
```

```text
Object.seal()
        ↓
Cannot ADD
        ↓
Cannot DELETE
        ↓
Can MODIFY
```

```text
Object.freeze()
        ↓
Cannot ADD
        ↓
Cannot DELETE
        ↓
Cannot MODIFY
```

## Most Important Points

- `seal()` → **structure locked, values can change**
- `freeze()` → **structure and values locked**
- Both are **shallow**
- `Object.isSealed()` → check sealed status
- `Object.isFrozen()` → check frozen status
- `preventExtensions()` only prevents adding properties
- In strict mode, invalid modifications can throw `TypeError`
- For nested immutability, use a **deep freeze** approach
