## Polyfill in JavaScript

A **polyfill** is code that provides an implementation of a **modern JavaScript feature** in environments that don't natively support it.

> In simple terms: **If the browser doesn't support a feature, a polyfill provides an alternative implementation so your code can still use that feature.**

### Example: `Array.prototype.includes()`

Suppose an older browser doesn't support:

```js
const numbers = [1, 2, 3];

numbers.includes(2);
// true
```

We could provide a polyfill:

```js
if (!Array.prototype.includes) {
  Array.prototype.includes = function (value) {
    return this.indexOf(value) !== -1;
  };
}
```

Now older environments can use:

```js
[1, 2, 3].includes(2);
// true
```

---

## Why do we need polyfills?

JavaScript features are added over time.

For example:

```text
New JavaScript feature
        ↓
Modern browsers support it
        ↓
Older browsers may not
        ↓
       Polyfill
        ↓
Older browsers can use it
```

This is particularly useful when supporting older browsers or runtimes.

---

## Polyfill vs Transpiler

These are commonly confused in interviews.

| Polyfill                               | Transpiler                            |
| -------------------------------------- | ------------------------------------- |
| Adds missing **runtime functionality** | Converts **syntax**                   |
| Usually JavaScript code                | Usually a build-time transformation   |
| Example: `Array.prototype.includes()`  | Example: converting optional chaining |
| Runs in the target environment         | Runs during the build                 |

### Example

A transpiler can transform:

```js
const name = user?.profile?.name;
```

into older-compatible JavaScript.

But a transpiler generally **cannot magically provide a missing API** such as:

```js
Promise
```

A **Promise polyfill** can provide that runtime functionality.

---

## Important Interview Point

Not every new JavaScript feature requires a polyfill.

For example:

```js
const add = (a, b) => a + b;
```

An older environment doesn't understand arrow-function syntax. This is a **transpilation** problem, not something a typical polyfill solves.

But:

```js
Promise.resolve(10);
```

If the environment doesn't provide `Promise`, a **polyfill** can implement it.

### Interview Answer

> **A polyfill is code that provides the functionality of a modern JavaScript API in environments where that API isn't natively supported. Polyfills solve runtime API compatibility, while transpilers mainly transform newer JavaScript syntax into older syntax.**
