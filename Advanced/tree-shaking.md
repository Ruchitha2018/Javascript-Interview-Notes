# Tree Shaking in JavaScript

**Tree shaking** is a build-time optimization technique that removes **unused code** from the final JavaScript bundle.

Think of it as:

> 🌳 **Keep the branches of code that are actually used and remove the dead branches.**

### Example

Suppose you have:

```js
// utils.js
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}

export function multiply(a, b) {
  return a * b;
}
```

And you import only:

```js
import { add } from "./utils.js";

console.log(add(2, 3));
```

A bundler such as **Webpack, Rollup, or esbuild** can determine that `subtract()` and `multiply()` are never used and remove them from the production bundle.

Conceptually:

```js
// Final bundle
function add(a, b) {
  return a + b;
}

console.log(add(2, 3));
```

### Why is it called Tree Shaking?

The dependency graph is represented like a tree:

```text
             app.js
                |
             utils.js
          /      |       \
       add    subtract   multiply
        ↑
      used
```

The bundler "shakes" the tree and removes unused branches:

```text
             app.js
                |
             utils.js
                |
               add
```

---

## Tree Shaking and ES Modules

Tree shaking works particularly well with **ES Modules (`import` / `export`)** because imports and exports are statically analyzable.

```js
import { add } from "./math.js";
```

The bundler can determine at build time which exports are actually used.

### Why CommonJS is harder

With CommonJS:

```js
const math = require("./math");

math.add(1, 2);
```

the module system is more dynamic, so determining exactly which exports are unused can be difficult.

For example:

```js
const moduleName = getModuleName();

const module = require(moduleName);
```

The dependency cannot necessarily be determined statically.

Therefore:

> **ESM is much more tree-shaking friendly than CommonJS.**

Modern bundlers can sometimes perform some dead-code elimination on CommonJS, but it is generally less reliable/effective than with ESM.

---

## Tree Shaking vs Dead Code Elimination

These terms are related but not exactly identical.

**Tree shaking:**

> Determines unused modules/exports and removes them.

**Dead Code Elimination (DCE):**

> Removes code that can never be executed or whose result is unnecessary.

Example:

```js
function add(a, b) {
  return a + b;
}

function unused() {
  console.log("Never called");
}

console.log(add(1, 2));
```

A minifier/bundler can remove `unused()` as dead code.

---

## Important Conditions for Tree Shaking

Tree shaking is most effective when:

1. You use **ES Modules**.
2. The bundler can statically analyze imports/exports.
3. Code has no unexpected **side effects**.
4. Production optimization/minification is enabled.

For example:

```js
// utils.js
export const add = (a, b) => a + b;

export const subtract = (a, b) => a - b;
```

```js
import { add } from "./utils.js";
```

The unused `subtract` export can potentially be removed.

### Side Effects

Consider:

```js
// analytics.js
console.log("Analytics initialized");

export function track() {
  // ...
}
```

Even if `track()` isn't imported, the module itself has a side effect:

```js
console.log("Analytics initialized");
```

A bundler cannot blindly remove the entire module because doing so could change application behavior.

This is why packages can declare side-effect information, for example in `package.json`:

```json
{
  "sideEffects": false
}
```

This tells compatible bundlers that importing the package's modules is expected to be free of side effects, allowing more aggressive tree shaking.

---

### Interview Answer

> **Tree shaking is a build-time optimization technique used by modern JavaScript bundlers to remove unused code, especially unused ES module imports and exports, from the production bundle. It relies on static analysis and works most effectively with ES Modules because their dependencies can be determined at build time.**
