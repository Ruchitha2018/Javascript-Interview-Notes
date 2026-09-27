# Promise Executor

Yes — **the Promise constructor's executor function executes synchronously**.

```js
console.log("A");

const promise = new Promise((resolve, reject) => {
  console.log("B");
  resolve("Done");
});

promise.then(() => {
  console.log("C");
});

console.log("D");
```

Output:

```text
A
B
D
C
```

### Why?

When you do:

```js
new Promise((resolve, reject) => {
  console.log("B");
});
```

the function passed to the Promise constructor (called the **executor**) runs **immediately and synchronously**.

But the `.then()` callback is asynchronous:

```js
promise.then(() => {
  console.log("C");
});
```

It is scheduled as a **microtask**, so it runs after the current synchronous code finishes.

### Execution flow

```text
new Promise()
     ↓
Executor runs immediately
     ↓
resolve()
     ↓
.then() callback → Microtask Queue
     ↓
Current synchronous code finishes
     ↓
Microtask executes
```

### Very important interview point

```js
new Promise((resolve) => {
  console.log("Executor");
  resolve();
});
```

**Executor:** synchronous ✅

```js
promise.then(() => {
  console.log("Then");
});
```

**`.then()` callback:** asynchronous/microtask ✅

So the short answer is:

> **Yes. The Promise constructor's executor function executes synchronously, immediately when the Promise is created. The callbacks attached with `.then()`, `.catch()`, and `.finally()` execute asynchronously as microtasks.**
