# Object.keys(), Object.values(), and Object.entries()

### Object.keys()

Returns an array of an object's own enumerable property names.

```js
const user = {
  name: "John",
  age: 30,
  city: "Delhi"
};

console.log(Object.keys(user));
// ["name", "age", "city"]
```

#### Common use

```js
Object.keys(user).forEach(key => {
  console.log(key);
});
```

---
### Object.values()

Returns an array of an object's own enumerable property values.

```js
const user = {
  name: "John",
  age: 30,
  city: "Delhi"
};

console.log(Object.values(user));
// ["John", 30, "Delhi"]
```

#### Common use

```js
const prices = {
  apple: 100,
  banana: 50,
  mango: 150
};

const total = Object.values(prices)
  .reduce((sum, price) => sum + price, 0);

console.log(total);
// 300
```

---
### Object.entries()

Returns an array of `[key, value]` pairs.

```js
const user = {
  name: "John",
  age: 30,
  city: "Delhi"
};

console.log(Object.entries(user));
```

Output:

```js
[
  ["name", "John"],
  ["age", 30],
  ["city", "Delhi"]
]
```

#### Common use

```js
Object.entries(user).forEach(([key, value]) => {
  console.log(key, value);
});
```


## Difference

| Method | Returns |
|---|---|
| `Object.keys(user)` | Keys |
| `Object.values(user)` | Values |
| `Object.entries(user)` | `[key, value]` pairs |

### Memory Trick

```text
keys()     → keys
values()   → values
entries()  → key + value
```

---
### Object.entries() + Object.fromEntries()

Useful for transforming objects.

```js
const prices = {
  apple: 100,
  banana: 50,
  mango: 150
};

const updatedPrices = Object.fromEntries(
  Object.entries(prices).map(([key, value]) => {
    return [key, value * 2];
  })
);

console.log(updatedPrices);
```

Output:

```js
{
  apple: 200,
  banana: 100,
  mango: 300
}
```

### Pattern

```text
Object
  ↓
Object.entries()
  ↓
Array of [key, value]
  ↓
map / filter
  ↓
Object.fromEntries()
  ↓
Object
```
---

### Important Interview Point

These methods work with **own enumerable properties**.

They don't include inherited enumerable properties.

```js
const parent = {
  country: "India"
};

const user = Object.create(parent);

user.name = "John";

console.log(Object.keys(user));
// ["name"]
```

`country` is inherited, so it isn't returned.

```js
const user = {
  name: "John"
};

Object.defineProperty(user, "password", {
  value: "12345",
  enumerable: false
});

console.log(user.password);
// "12345"

console.log(Object.keys(user));
// ["name"]
```

---
### Object.keys() vs for...in

```js
for (const key in user) {
  console.log(key);
}
```

`for...in` can iterate over enumerable inherited properties as well.

`Object.keys()` only returns own enumerable properties.

To restrict `for...in` to own properties:

```js
for (const key in user) {
  if (Object.hasOwn(user, key)) {
    console.log(key);
  }
}
```
---
### Senior Interview Cheat Sheet

```js
const obj = {
  name: "John",
  age: 30
};

Object.keys(obj);
// ["name", "age"]

Object.values(obj);
// ["John", 30]

Object.entries(obj);
// [["name", "John"], ["age", 30]]
```

### Remember

> `keys()` → What properties does the object have?  
> `values()` → What values does it contain?  
> `entries()` → Give me both key and value.