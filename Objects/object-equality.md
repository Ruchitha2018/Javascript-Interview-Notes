# JavaScript Object Equality

## 1. `===` compares object references

```js
const obj1 = { name: "John" };
const obj2 = { name: "John" };

console.log(obj1 === obj2);
// false
```

Even though the contents are identical, they are two different objects in memory.

```text
obj1 ──→ { name: "John" }
obj2 ──→ { name: "John" }

Different references → false
```



## 2. Same reference → `true`

```js
const obj1 = { name: "John" };
const obj2 = obj1;

console.log(obj1 === obj2);
// true
```

Both variables point to the same object.

```text
        ┌──────────────┐
obj1 ──→│ { name:John }│
obj2 ──→│              │
        └──────────────┘
```


## 3. Comparing Object Contents

JavaScript does not have a general built-in `Object.equals()` method for deep object comparison.

For simple objects, you can compare keys and values.

```js
function isEqual(obj1, obj2) {
  const keys1 = Object.keys(obj1);
  const keys2 = Object.keys(obj2);

  if (keys1.length !== keys2.length) {
    return false;
  }

  return keys1.every(key =>
    obj1[key] === obj2[key]
  );
}
```

Example:

```js
const obj1 = {
  name: "John",
  age: 30
};

const obj2 = {
  name: "John",
  age: 30
};

console.log(isEqual(obj1, obj2));
// true
```


## 4. Shallow Comparison

The above comparison is only a **shallow comparison**.

```js
const obj1 = {
  name: "John",
  address: {
    city: "Mumbai"
  }
};

const obj2 = {
  name: "John",
  address: {
    city: "Mumbai"
  }
};

console.log(obj1.address === obj2.address);
// false
```

The nested objects are different references.


## 5. Deep Equality

For nested objects, you need a **deep comparison**.

A simple recursive implementation:

```js
function deepEqual(a, b) {
  if (a === b) {
    return true;
  }

  if (
    typeof a !== "object" ||
    typeof b !== "object" ||
    a === null ||
    b === null
  ) {
    return false;
  }

  const keysA = Object.keys(a);
  const keysB = Object.keys(b);

  if (keysA.length !== keysB.length) {
    return false;
  }

  for (const key of keysA) {
    if (
      !Object.prototype.hasOwnProperty.call(b, key) ||
      !deepEqual(a[key], b[key])
    ) {
      return false;
    }
  }

  return true;
}
```

Example:

```js
const obj1 = {
  name: "John",
  address: {
    city: "Mumbai"
  }
};

const obj2 = {
  name: "John",
  address: {
    city: "Mumbai"
  }
};

console.log(deepEqual(obj1, obj2));
// true
```

> Note: This is a simple educational implementation. Production-grade deep equality needs to account for cases such as circular references, `Date`, `Map`, `Set`, arrays, symbols, and special objects.



## 6. `JSON.stringify()` — Common but Limited

You may see:

```js
JSON.stringify(obj1) === JSON.stringify(obj2)
```

Example:

```js
const obj1 = {
  name: "John",
  age: 30
};

const obj2 = {
  name: "John",
  age: 30
};

console.log(
  JSON.stringify(obj1) === JSON.stringify(obj2)
);
// true
```

But this is **not a general-purpose deep equality solution**.

For example:

```js
const obj1 = {
  name: "John",
  age: 30
};

const obj2 = {
  age: 30,
  name: "John"
};

console.log(
  JSON.stringify(obj1) === JSON.stringify(obj2)
);
```

This can be `false` because the property insertion order can differ.

It also has limitations with:

- `undefined`
- Functions
- `Symbol`
- `Date`
- `Map`
- `Set`
- Circular references
- Other special JavaScript values



## 7. `Object.is()`

`Object.is()` performs JavaScript's **SameValue** comparison.

For object references:

```js
const obj1 = {};
const obj2 = obj1;

console.log(Object.is(obj1, obj2));
// true

console.log(Object.is({}, {}));
// false
```

For objects, it still checks whether the references are the same.

It also differs from `===` for some primitive edge cases:

```js
Object.is(NaN, NaN);
// true

NaN === NaN;
// false
```

And:

```js
Object.is(+0, -0);
// false

+0 === -0;
// true
```


## 8. Shallow vs Deep Equality

```text
Shallow Equality
       ↓
Compare top-level values
       ↓
Nested objects are compared by reference
```

```text
Deep Equality
       ↓
Compare top-level values
       ↓
Recursively compare nested objects/arrays
       ↓
Compare actual contents
```

Example:

```js
const a = {
  user: {
    name: "John"
  }
};

const b = {
  user: {
    name: "John"
  }
};

console.log(a.user === b.user);
// false

console.log(deepEqual(a, b));
// true
```

---

# 9. Common Interview Question

What is the output?

```js
const a = { x: 1 };
const b = { x: 1 };
const c = a;

console.log(a === b);
console.log(a === c);
```

Output:

```text
false
true
```

Why?

```text
a and b → different objects
a and c → same object reference
```


## 10. Another Interview Question

```js
const a = {
  user: {
    name: "John"
  }
};

const b = {
  user: {
    name: "John"
  }
};

console.log(a === b);
console.log(a.user === b.user);
```

Output:

```text
false
false
```

Both the outer objects and nested `user` objects are separate references.



## 11. Senior Interview Cheat Sheet

| Comparison | Meaning |
|---|---|
| `obj1 === obj2` | Same object reference |
| `Object.is(obj1, obj2)` | SameValue/reference comparison |
| Shallow comparison | Compare top-level properties |
| Deep comparison | Recursively compare nested values |
| `JSON.stringify()` | Quick/simple comparison with limitations |

## Most Important Point

> **In JavaScript, objects are compared by reference, not by their contents.**

`===` returns `true` only when both variables reference the **same object**.

If you need to determine whether two separate objects contain the same data, use a **shallow or deep equality comparison** depending on the structure.



## Quick Memory Trick

```text
Primitive:
10 === 10
→ true

Object:
{} === {}
→ false

Same object:
const a = {};
const b = a;

a === b
→ true
```

```text
Same reference → true
Same contents  → not necessarily true
```
