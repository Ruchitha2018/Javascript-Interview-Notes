# JavaScript Modules

A **module** is a reusable, self-contained piece of JavaScript code that can **export** functionality and **import** functionality from other modules.

Modules help us organize large applications into smaller files.

```text
app.js
 ├── imports → user.js
 ├── imports → api.js
 └── imports → utils.js
```


## 1. ES Modules (ESM)

Modern JavaScript uses `export` and `import`.

### Export

```js
// math.js

export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}
```

### Import

```js
// app.js

import { add, subtract } from "./math.js";

console.log(add(2, 3));
console.log(subtract(5, 2));
```



## 2. Named Export

You can export multiple things from a module.

```js
// math.js

export const PI = 3.14;

export function add(a, b) {
  return a + b;
}
```

Import using the same names:

```js
import { PI, add } from "./math.js";
```

You can rename them using `as`:

```js
import { add as sum } from "./math.js";

console.log(sum(2, 3));
```


## 3. Default Export

A module can have **one default export**.

```js
// user.js

export default class User {
  constructor(name) {
    this.name = name;
  }
}
```

Import:

```js
import User from "./user.js";

const user = new User("John");
```

With default exports, the importing name doesn't have to match the exported name:

```js
import MyUser from "./user.js";
```

---

## 4. Named vs Default Export

| Feature                 | Named Export        | Default Export       |
| ----------------------- | ------------------- | -------------------- |
| Number per module       | Multiple            | One                  |
| Import syntax           | `{ add }`           | `add`                |
| Import name must match? | Yes, unless aliased | No                   |
| Example                 | `export { add }`    | `export default add` |

---

## 5. Re-exporting

A module can import something and export it again.

```js
// index.js

export { add } from "./math.js";
export { User } from "./user.js";
```

Then consumers can simply do:

```js
import { add, User } from "./index.js";
```

This pattern is commonly used for **barrel files**.



## 6. Dynamic `import()`

Modules can also be loaded dynamically.

```js
const module = await import("./math.js");

console.log(module.add(2, 3));
```

Unlike static:

```js
import { add } from "./math.js";
```

dynamic `import()` returns a **Promise** and is useful for **lazy loading/code splitting**.



## 7. ES Modules vs CommonJS

You may encounter two module systems in frontend/backend JavaScript.

### ES Modules

```js
import { add } from "./math.js";

export { add };
```

### CommonJS

```js
const { add } = require("./math");

module.exports = { add };
```

| Feature      | ESM                         | CommonJS                              |
| ------------ | --------------------------- | ------------------------------------- |
| Import       | `import`                    | `require()`                           |
| Export       | `export`                    | `module.exports`                      |
| Loading      | Static + dynamic `import()` | Traditionally synchronous `require()` |
| Tree shaking | Excellent support           | More difficult                        |
| Standard     | JavaScript standard         | Traditionally Node.js module system   |

### Important interview point: Tree Shaking

ES Modules are **statically analyzable**, which makes them well suited for tree shaking.

```js
import { add } from "./math.js";
```

A bundler can determine which exports are being used and potentially remove unused exports.

CommonJS can be more difficult to analyze:

```js
const math = require("./math");
```

especially when `require()` is used dynamically.

---

## 8. Module Scope

Variables declared inside a module are **not automatically global**.

```js
// math.js

const secret = 123;

export function add(a, b) {
  return a + b;
}
```

`secret` is private to the module unless explicitly exported.

```text
Module
 ├── secret      ← private
 └── add()       ← exported
```

This gives modules a natural way to create **encapsulation**.

---

## 9. Important ESM Characteristics

### Imports are live bindings

```js
// counter.js
export let count = 0;

export function increment() {
  count++;
}
```

```js
// app.js
import { count, increment } from "./counter.js";

console.log(count); // 0

increment();

console.log(count); // 1
```

The imported binding reflects the exported value rather than being an independent copy.

### Modules are evaluated once

If multiple parts of an application import the same module, the module is generally evaluated once and its module instance is reused.

---

### Interview Answer

> **A JavaScript module is a self-contained unit of code that encapsulates variables and functionality and can expose selected functionality through exports. Modern JavaScript uses ES Modules with `import` and `export`. ESM provides module scope, static dependency analysis, live bindings, and strong support for tree shaking and code splitting.**
