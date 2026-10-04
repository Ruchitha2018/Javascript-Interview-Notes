

# `for...of` vs `for...in`

Both are used for iteration, but they iterate over **different things**.

| Feature            | `for...of`                  | `for...in`                |
| ------------------ | --------------------------- | ------------------------- |
| Iterates over      | **Values**                  | **Keys / property names** |
| Works with arrays  | ✅                           | ✅, but usually avoid      |
| Works with objects | ❌ Not directly              | ✅                         |
| Works with strings | ✅                           | ❌ Not the intended use    |
| Works with Map     | ✅                           | ❌                         |
| Works with Set     | ✅                           | ❌                         |
| Requires iterable  | ✅                           | ❌                         |
| Gets array index?  | Value                       | Index                     |
| Typical use        | Arrays, strings, Maps, Sets | Object properties         |

---

### 1. `for...of`

`for...of` gives you the **values**.

### Array

```js
const numbers = [10, 20, 30];

for (const value of numbers) {
  console.log(value);
}
```

Output:

```text
10
20
30
```

Think:

```text
for...of → values
```

---

### String

Strings are iterable:

```js
const name = "John";

for (const char of name) {
  console.log(char);
}
```

Output:

```text
J
o
h
n
```

---

### Set

```js
const numbers = new Set([10, 20, 30]);

for (const value of numbers) {
  console.log(value);
}
```

Output:

```text
10
20
30
```

---

### Map

With a `Map`, `for...of` gives `[key, value]` pairs:

```js
const users = new Map([
  ["id1", "John"],
  ["id2", "Mike"]
]);

for (const [key, value] of users) {
  console.log(key, value);
}
```

Output:

```text
id1 John
id2 Mike
```

---

### 2. `for...in`

`for...in` gives you the **property keys**.

```js
const user = {
  name: "John",
  age: 30
};

for (const key in user) {
  console.log(key);
}
```

Output:

```text
name
age
```

To get the value:

```js
for (const key in user) {
  console.log(key, user[key]);
}
```

Output:

```text
name John
age 30
```

---

### Array: important difference

Consider:

```js
const numbers = [10, 20, 30];
```

### `for...of`

```js
for (const value of numbers) {
  console.log(value);
}
```

Output:

```text
10
20
30
```

### `for...in`

```js
for (const index in numbers) {
  console.log(index);
}
```

Output:

```text
0
1
2
```

So:

```text
for...of → 10, 20, 30
for...in → 0, 1, 2
```


### Why doesn't `for...of` work on a normal object?

This:

```js
const user = {
  name: "John",
  age: 30
};

for (const value of user) {
  console.log(value);
}
```

throws:

```text
TypeError: user is not iterable
```

A normal object doesn't implement the **iterable protocol**.

You can use:

```js
for (const [key, value] of Object.entries(user)) {
  console.log(key, value);
}
```

Or:

```js
for (const value of Object.values(user)) {
  console.log(value);
}
```

---

### The deeper concept: Iterable

`for...of` works with **iterables**.

Examples:

```text
Array      ✅
String     ✅
Map        ✅
Set        ✅
Generator  ✅
NodeList   ✅
Object     ❌ (by default)
```

An iterable provides:

```js
Symbol.iterator
```

Example:

```js
const numbers = [1, 2, 3];

console.log(numbers[Symbol.iterator]);
```

---


### Easy memory trick

```text
for...of → OF the values
           ↓
         values

for...in → IN the object
           ↓
         keys
```

**One important nuance:** `for...in` can enumerate **inherited enumerable properties** as well, so when using it on objects you may need `Object.hasOwn()` if you only want the object's own properties.
