## What is ES6?

**ES6 (ECMAScript 2015)** is the **6th edition of the ECMAScript specification** and was released in 2015. It was one of the biggest updates to JavaScript, introducing features that made JavaScript more suitable for building large and complex applications. ([ECMA International][1])

> **JavaScript** is the programming language, while **ECMAScript** is the standard/specification that defines how the language should work.

### ES6 Features

| #  | Feature                          | What it provides                                     | Example                             |
| -- | -------------------------------- | ---------------------------------------------------- | ----------------------------------- |
| 1  | **`let` & `const`**              | Block-scoped variables                               | `let count = 10;`                   |
| 2  | **Arrow Functions**              | Shorter function syntax and lexical `this`           | `const add = (a, b) => a + b;`      |
| 3  | **Template Literals**            | String interpolation and multiline strings           | `` `Hello ${name}` ``               |
| 4  | **Destructuring**                | Extract values from arrays/objects easily            | `const { name } = user;`            |
| 5  | **Default Parameters**           | Default values for function parameters               | `function greet(name = "Guest") {}` |
| 6  | **Rest Parameters**              | Collect multiple arguments into an array             | `function sum(...nums) {}`          |
| 7  | **Spread Operator**              | Expand arrays/objects/iterables                      | `const arr2 = [...arr1];`           |
| 8  | **Enhanced Object Literals**     | Shorthand properties/methods and computed properties | `{ name, greet() {} }`              |
| 9  | **Classes**                      | Cleaner syntax for objects and inheritance           | `class User {}`                     |
| 10 | **Modules**                      | `import` / `export` for modular code                 | `export const x = 10;`              |
| 11 | **Promises**                     | Handle asynchronous operations                       | `new Promise(...)`                  |
| 12 | **`for...of`**                   | Iterate over iterable values                         | `for (const item of items)`         |
| 13 | **Iterators**                    | Define custom iteration behavior                     | `iterator.next()`                   |
| 14 | **Generators**                   | Functions that can pause/resume execution            | `function* gen() { yield 1; }`      |
| 15 | **Map**                          | Key-value collection with arbitrary key types        | `new Map()`                         |
| 16 | **Set**                          | Collection of unique values                          | `new Set()`                         |
| 17 | **WeakMap**                      | Weakly-held object-keyed collection                  | `new WeakMap()`                     |
| 18 | **WeakSet**                      | Weakly-held collection of objects                    | `new WeakSet()`                     |
| 19 | **Symbols**                      | Creates unique primitive values                      | `Symbol("id")`                      |
| 20 | **Proxy**                        | Intercept object operations                          | `new Proxy(obj, handler)`           |
| 21 | **Reflect**                      | API for performing object operations                 | `Reflect.get(obj, "name")`          |
| 22 | **New String/Array/Object APIs** | Useful built-in methods                              | `includes()`, `Object.assign()`     |
| 23 | **Binary & Octal Literals**      | New numeric literal syntax                           | `0b1010`, `0o755`                   |
| 24 | **Unicode Improvements**         | Better Unicode support                               | `u` regex flag, Unicode code points |
| 25 | **Exponentiation Operator**      | Exponent calculation                                 | `2 ** 3`                            |

These are the major ES2015 additions documented by the ECMAScript specification and commonly grouped as ES6 features. ([ECMA International][1])


**Interview one-liner:**

> **ES6, also called ECMAScript 2015, is the sixth edition of the ECMAScript standard that introduced major JavaScript features such as `let/const`, arrow functions, classes, modules, promises, destructuring, spread/rest, iterators, generators, Map, Set, Symbol, Proxy, and Reflect.**

[1]: https://262.ecma-international.org/6.0/?utm_source=chatgpt.com "ECMAScript 2015 Language Specification – ECMA-262 6th Edition"
[2]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions?utm_source=chatgpt.com "Arrow function expressions - JavaScript | MDN"
