# How do you copy properties from one object to other


There are several ways to copy properties from one object to another in JavaScript.

### 1. Spread operator `...`

The most common modern approach:

```js
const source = {
  name: "John",
  age: 30
};

const target = {
  ...source
};

console.log(target);
// { name: "John", age: 30 }
```

It creates a **shallow copy**.

---

### 2. `Object.assign()`

```js
const source = {
  name: "John",
  age: 30
};

const target = {};

Object.assign(target, source);

console.log(target);
// { name: "John", age: 30 }
```

You can also merge multiple objects:

```js
const result = Object.assign({}, obj1, obj2);
```

If the same property exists in multiple objects, the **later source wins**.

```js
const result = Object.assign(
  {},
  { name: "John" },
  { name: "Mike" }
);

console.log(result.name);
// Mike
```

---

### 3. `Object.keys()`

Useful when you want custom control over what gets copied:

```js
const source = {
  name: "John",
  age: 30
};

const target = {};

Object.keys(source).forEach(key => {
  target[key] = source[key];
});
```

`Object.keys()` returns the object's **own enumerable string-keyed properties**.

---

### 4. `Object.entries()` + `Object.fromEntries()`

Useful when you want to filter or transform properties:

```js
const source = {
  name: "John",
  age: 30
};

const target = Object.fromEntries(
  Object.entries(source)
);

console.log(target);
// { name: "John", age: 30 }
```

For example, copy only properties whose values are numbers:

```js
const target = Object.fromEntries(
  Object.entries(source)
    .filter(([key, value]) => typeof value === "number")
);
```


### Shallow Copy 

Both spread and `Object.assign()` perform **shallow copies**.

```js
const source = {
  name: "John",
  address: {
    city: "Nagpur"
  }
};

const copy = { ...source };

copy.address.city = "Mumbai";

console.log(source.address.city);
// Mumbai
```

Why?

```text
source ──→ address ──→ { city: "Nagpur" }
                         ↑
copy ────────────────────┘
```

The nested object is still shared.


### Deep Copy

For supported data types, modern JavaScript provides:

```js
const copy = structuredClone(source);
```

Now nested objects are cloned too:

```js
copy.address.city = "Mumbai";

console.log(source.address.city);
// Nagpur
```

### Interview Summary

| Method              | Copy           | Use                           |
| ------------------- | -------------- | ----------------------------- |
| `{ ...obj }`        | Shallow        | Modern object copying         |
| `Object.assign()`   | Shallow        | Copy/merge properties         |
| `Object.keys()`     | Shallow/manual | Custom copying                |
| `Object.entries()`  | Shallow        | Transform/filter              |
| `structuredClone()` | Deep           | Deep copy of supported values |

**Most important:**
`{ ...obj }` and `Object.assign()` → **shallow copy**.
`structuredClone()` → **deep copy**.
