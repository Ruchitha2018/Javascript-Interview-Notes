# JSON in JavaScript

**JSON (JavaScript Object Notation)** is a lightweight **text-based data format** used to exchange and store structured data.

It is commonly used when communicating between a **frontend and backend through APIs**.

### Example

```json
{
  "name": "John",
  "age": 30,
  "isActive": true,
  "skills": ["JavaScript", "React"]
}
```

### JSON Data Types

JSON supports only these data types:

| Type    | Example              |
| ------- | -------------------- |
| String  | `"John"`             |
| Number  | `30`                 |
| Boolean | `true`               |
| Null    | `null`               |
| Object  | `{ "name": "John" }` |
| Array   | `["JS", "React"]`    |

It does **not** support JavaScript-specific values such as:

* `undefined`
* Functions
* `Symbol`
* `BigInt`

---

## `JSON.stringify()`

Converts a JavaScript value/object into a **JSON string**.

```js
const user = {
  name: "John",
  age: 30
};

const json = JSON.stringify(user);

console.log(json);
// {"name":"John","age":30}

console.log(typeof json);
// string
```

This is commonly used when sending data over HTTP.

---

## `JSON.parse()`

Converts a JSON string into a JavaScript value/object.

```js
const json = '{"name":"John","age":30}';

const user = JSON.parse(json);

console.log(user.name);
// John

console.log(typeof user);
// object
```

### Remember

```text
JavaScript Object
       |
       | JSON.stringify()
       ↓
   JSON String
       |
       | JSON.parse()
       ↓
JavaScript Object
```

---

## Important Interview Differences

### JSON vs JavaScript Object

**JavaScript object:**

```js
const user = {
  name: "John",
  greet() {
    console.log("Hello");
  }
};
```

**JSON:**

```json
{
  "name": "John"
}
```

In JSON:

* Property names must use **double quotes**.
* Strings must use **double quotes**.
* Functions are not allowed.
* `undefined` is not a valid JSON value.
* Comments are not allowed.

### Handling invalid JSON

```js
try {
  const data = JSON.parse('{"name":}');
} catch (error) {
  console.log(error.name);
  // SyntaxError
}
```

### Interview Answer

> **JSON is a text-based data-interchange format used to represent structured data. In JavaScript, `JSON.stringify()` converts a JavaScript value into a JSON string, while `JSON.parse()` converts a valid JSON string back into a JavaScript value.**
