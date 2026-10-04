# Template Literals in JavaScript

**Template literals** were introduced in **ES6**. They provide an easier way to create strings, especially when the string contains variables, expressions, or multiple lines.

They use **backticks (`)** instead of single or double quotes.


### 1. Basic Template Literal

```js
const name = "John";

const message = `Hello ${name}`;

console.log(message);
// Hello John
```

`${...}` is called a **template substitution**.

You can put expressions inside it:

```js
const a = 10;
const b = 20;

console.log(`Sum = ${a + b}`);
// Sum = 30
```
---
### 2. Multiline Strings

Template literals allow multiline strings without using `\n`.

```js
const message = `
Hello John,
Welcome to JavaScript.
Good luck with your interview!
`;

console.log(message);
```

With normal strings, you would need:

```js
const message = "Hello John,\nWelcome to JavaScript.";
```
---

### Nested Template Literals

A **nested template literal** means using a template literal inside another template literal, usually through an expression.

### Example

```js
const name = "John";
const age = 30;

const message = `User: ${`Name: ${name}, Age: ${age}`}`;

console.log(message);
// User: Name: John, Age: 30
```

A more practical example:

```js
const users = [
  { name: "John", age: 30 },
  { name: "Mike", age: 25 }
];

const result = `
  ${users.map(user => `
    <div>
      <h2>${user.name}</h2>
      <p>${user.age}</p>
    </div>
  `).join("")}
`;

console.log(result);
```

The important point is that `${...}` can contain an **expression**, and that expression can itself contain a template literal.

---

### Tagged Template Literals

A **tagged template** allows a function to process a template literal before the final string is created.

### Basic example

```js
function tag(strings, ...values) {
  console.log(strings);
  console.log(values);
}

const name = "John";
const age = 30;

tag`Name: ${name}, Age: ${age}`;
```

The tag function receives:

```text
strings → ["Name: ", ", Age: ", ""]
values  → ["John", 30]
```

Conceptually:

```text
tag`Name: ${name}, Age: ${age}`
 ↓
tag(strings, ...values)
```

---

### How Tagged Templates Work?

Consider:

```js
function tag(strings, ...values) {
  return `${strings[0]}${values[0]}${strings[1]}${values[1]}${strings[2]}`;
}

const name = "John";
const age = 30;

console.log(tag`Name: ${name}, Age: ${age}`);

// Name: John, Age: 30
```

The tag function receives **two types of information**:

### 1. `strings`

An array containing the static parts.

```js
[
  "Name: ",
  ", Age: ",
  ""
]
```

### 2. `values`

The evaluated expressions.

```js
[
  "John",
  30
]
```

---
### Real-world Use Case

Tagged templates can be used for things such as:

* SQL query builders
* HTML templating
* Localization
* Sanitization
* Styled components
* Custom formatting

For example:

```js
function highlight(strings, ...values) {
  return strings.reduce(
    (result, string, index) =>
      result + string + (values[index] ?? ""),
    ""
  );
}

const name = "John";

console.log(highlight`Hello ${name}!`);
// Hello John!
```

---

### `String.raw`

JavaScript provides a built-in tag called `String.raw`.

It returns the **raw string representation**, which is useful when you don't want escape sequences such as `\n` to be interpreted.

```js
const value = String.raw`Hello\nWorld`;

console.log(value);
// Hello\nWorld
```

Without `String.raw`:

```js
const value = `Hello\nWorld`;

console.log(value);
// Hello
// World
```

---

### Quick Comparison

| Feature            | Example                          | Purpose                       |
| ------------------ | -------------------------------- | ----------------------------- |
| Template literal   | `` `Hello ${name}` ``            | String interpolation          |
| Multiline template | `` `Hello\nWorld` ``             | Multiline strings             |
| Nested template    | `` `User: ${`Name: ${name}`}` `` | Template inside an expression |
| Tagged template    | `tag\`Hello ${name}``            | Custom string processing      |
| `String.raw`       | `String.raw\`a\nb``              | Preserve raw escapes          |

### Interview Answer

> **Template literals, introduced in ES6, use backticks and support string interpolation, expressions, and multiline strings. Nested templates can be created by using a template literal inside a `${...}` expression. Tagged templates allow a function to process the static string portions and interpolated values before producing the final result.**
