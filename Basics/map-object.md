# Map vs Object

- **`Object` is primarily for structured data, while `Map` is designed specifically for key-value collections.**

### Map vs Object

| Feature                   | `Map`                   | `Object`                                        |
| ------------------------- | ----------------------- | ----------------------------------------------- |
| Purpose                   | Key-value collection    | Structured data / properties                    |
| Key types                 | **Any type**            | Primarily strings/symbols                       |
| Object as key             | ✅                       | ❌ converted to string                           |
| Maintains insertion order | ✅                       | Mostly, but property ordering has special rules |
| Size                      | `map.size`              | `Object.keys(obj).length`                       |
| Add/update                | `map.set(key, value)`   | `obj[key] = value`                              |
| Get                       | `map.get(key)`          | `obj[key]`                                      |
| Check key                 | `map.has(key)`          | `key in obj` / `hasOwn()`                       |
| Delete                    | `map.delete(key)`       | `delete obj[key]`                               |
| Iterate                   | Directly iterable       | `Object.keys/values/entries`                    |
| Prototype                 | Has `Map.prototype`     | Has `Object.prototype` by default               |
| Serialization             | No direct JSON support  | `JSON.stringify()` works naturally              |
| Frequent add/delete       | Generally better suited | Less specialized                                |
| Data structure semantics  | Explicit                | More general                                    |

---

## 1. Biggest difference: key types

### Object

Object property keys are strings or symbols:

```js
const obj = {};

const key = { id: 1 };

obj[key] = "John";

console.log(obj);
// { "[object Object]": "John" }
```

The object key gets converted to a string.

### Map

`Map` preserves the actual key:

```js
const map = new Map();

const key = { id: 1 };

map.set(key, "John");

console.log(map.get(key));
// John
```

You can use:

```js
map.set("name", "John");
map.set(10, "Ten");
map.set(true, "Yes");
map.set({ id: 1 }, "Object key");
map.set(() => {}, "Function key");
```

---

## 2. `Map` has a built-in `size`

```js
const map = new Map([
  ["a", 1],
  ["b", 2]
]);

console.log(map.size);
// 2
```

With Object:

```js
const obj = {
  a: 1,
  b: 2
};

console.log(Object.keys(obj).length);
// 2
```

---

## 3. Iteration

`Map` is directly iterable:

```js
const map = new Map([
  ["name", "John"],
  ["age", 30]
]);

for (const [key, value] of map) {
  console.log(key, value);
}
```

With Object:

```js
const obj = {
  name: "John",
  age: 30
};

for (const [key, value] of Object.entries(obj)) {
  console.log(key, value);
}
```

---

## 4. Object has prototype-related concerns

Consider:

```js
const obj = {};

console.log(obj.toString);
```

You get a property inherited from `Object.prototype`.

For dictionary-like data, you can avoid this with:

```js
const obj = Object.create(null);
```

`Map` doesn't have this issue because keys are stored in the map itself.

---

## 5. When should you use which?

### Use Object when:

The data represents an **entity/record**:

```js
const user = {
  name: "John",
  age: 30,
  email: "john@example.com"
};
```

Here, `name`, `age`, and `email` describe the user.

### Use Map when:

You need a **dynamic key-value collection**:

```js
const cache = new Map();

cache.set(userId, userData);
cache.set(productId, productData);
```

Especially useful when:

* Keys aren't strings
* You frequently add/remove entries
* You need `.size`
* You need predictable insertion-order iteration
* You need object/function references as keys

---

## Interview trap: "Is Map always faster than Object?"

**No.**

Don't say:

> "Map is faster than Object."

Performance depends on the operation, JavaScript engine, data size, and usage pattern.

A better interview answer:

> **"Map is specifically optimized as a key-value collection and provides collection-oriented APIs such as `size`, `has`, `set`, `delete`, and direct iteration. Object is generally more appropriate for structured records. I wouldn't choose between them purely based on a blanket performance claim."**

### One-line interview answer

> **Object → structured data / records. `Map` → dynamic key-value collections with arbitrary key types.**
