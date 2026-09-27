# `setTimeout()` vs `setInterval()`

Both are **Web APIs** used to schedule code to run later. They are asynchronous and work with the **event loop**.

### 1. `setTimeout()`

`setTimeout()` executes a function **once after a minimum delay**.

```js
setTimeout(() => {
  console.log("Hello");
}, 2000);
```

The callback is scheduled after approximately **2 seconds**.

> Important: `2000ms` does **not** guarantee that the callback runs exactly after 2 seconds. It means the callback becomes eligible to run after that delay, and the event loop executes it when the call stack is available.

#### Cancel `setTimeout`

```js
const timerId = setTimeout(() => {
  console.log("Hello");
}, 2000);

clearTimeout(timerId);
```

---

### 2. `setInterval()`

`setInterval()` executes a function **repeatedly at approximately the specified interval**.

```js
const intervalId = setInterval(() => {
  console.log("Hello");
}, 2000);
```

The callback is scheduled repeatedly at approximately every 2 seconds.

#### Cancel `setInterval`

```js
clearInterval(intervalId);
```

---

### Comparison

| Feature     | `setTimeout()`    | `setInterval()`   |
| ----------- | ----------------- | ----------------- |
| Execution   | Once              | Repeatedly        |
| API         | `setTimeout()`    | `setInterval()`   |
| Cancel      | `clearTimeout()`  | `clearInterval()` |
| Typical use | Delayed execution | Repeated tasks    |
| Returns     | Timer ID          | Timer ID          |

### Common interview question: Is `setTimeout(fn, 0)` immediate?

**No.**

```js
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

console.log("C");
```

Output:

```text
A
C
B
```

Even with `0ms`, the callback is placed into the task queue and runs only after the current synchronous code finishes.

### `setInterval()` gotcha

Consider:

```js
setInterval(() => {
  // Some long-running work
}, 1000);
```

The interval doesn't mean **"wait for the previous callback to finish, then wait 1 second."** The scheduling is based on the interval, and callbacks are subject to the event loop.

For sequential repeated work, a recursive `setTimeout()` is often easier to control:

```js
function run() {
  // Do work

  setTimeout(run, 1000);
}

run();
```

This schedules the **next execution after the current work completes**.

### Interview answer

> **`setTimeout()` schedules a callback to run once after a specified delay, while `setInterval()` schedules a callback repeatedly at a specified interval. Both are asynchronous scheduling mechanisms and their callbacks execute through the event loop when the call stack is available.**
