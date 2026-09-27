# Call Stack, Event Loop, Event Queue, Microtask, Macrotask Queue & queueMicrotask

## 1. Call Stack

The **call stack** keeps track of functions that are currently executing.

JavaScript executes code **one function at a time**.

### Example

```js
function first() {
  second();
}

function second() {
  console.log("Hello");
}

first();
```

Execution:

```text
Call Stack

first()
  ↓
second()
  ↓
console.log()
```

Once a function finishes, it is **removed (popped)** from the stack.

### Key Point

JavaScript has a **single call stack**, so only one piece of JavaScript code executes at a time.


## 2. Event Loop

The **event loop** continuously checks whether:

1. The call stack is empty.
2. There are pending tasks in the queues.

When the stack becomes empty, the event loop allows queued tasks to execute.

Simplified:

```text
             ┌──────────────┐
             │  Call Stack  │
             └──────┬───────┘
                    ↑
                    │
              Event Loop
                    │
        ┌───────────┴───────────┐
        ↓                       ↓
 Microtask Queue          Macrotask Queue
```


## 3. Event Queue

The **event queue** is a general term for queues containing callbacks or tasks waiting to execute.

In modern JavaScript environments, two important queues are:

- **Microtask queue**
- **Macrotask/task queue**

The event loop decides when queued callbacks can run.


## 4. Microtask Queue

The **microtask queue** contains high-priority asynchronous callbacks.

Common examples:

- `Promise.then()`
- `Promise.catch()`
- `Promise.finally()`
- `queueMicrotask()`
- `MutationObserver`

### Example

```js
console.log("Start");

Promise.resolve().then(() => {
  console.log("Promise");
});

console.log("End");
```

### Output

```text
Start
End
Promise
```

### Why?

```text
1. Start → Call Stack
2. Promise callback → Microtask Queue
3. End → Call Stack
4. Call Stack becomes empty
5. Microtask executes
```


## 5. Macrotask Queue

The **macrotask queue**, often called the **task queue**, contains regular asynchronous tasks.

Common examples include:

- `setTimeout`
- `setInterval`
- DOM events such as `click`
- Some I/O callbacks

### Example

```js
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 0);

console.log("End");
```

### Output

```text
Start
End
Timeout
```

Even with `0ms`, `setTimeout()` does **not execute immediately**.

Its callback is placed into the task/macrotask queue and waits until the current JavaScript execution finishes and the microtask checkpoint is handled.


## 6. `queueMicrotask()`

`queueMicrotask()` allows you to explicitly add a callback to the **microtask queue**.

### Example

```js
console.log("Start");

queueMicrotask(() => {
  console.log("Microtask");
});

console.log("End");
```

### Output

```text
Start
End
Microtask
```

`queueMicrotask()` has the same general scheduling priority as a resolved Promise callback.

### Example

```js
queueMicrotask(() => {
  console.log("A");
});

Promise.resolve().then(() => {
  console.log("B");
});
```

### Output

```text
A
B
```

They are processed in the order they were queued.


## 7. Microtask vs Macrotask

Consider the following example:

```js
console.log("1");

setTimeout(() => {
  console.log("2");
}, 0);

Promise.resolve().then(() => {
  console.log("3");
});

queueMicrotask(() => {
  console.log("4");
});

console.log("5");
```

### Output

```text
1
5
3
4
2
```

### Explanation

First, synchronous code executes:

```text
1
5
```

Then the microtask queue is processed:

```text
3
4
```

Only after the microtask queue is emptied does the next task/macrotask run:

```text
2
```

Therefore, the simplified execution order is:

```text
Synchronous Code
       ↓
Microtasks
       ↓
Next Task/Macrotask
       ↓
Microtasks
       ↓
Next Task/Macrotask
       ↓
...
```



## 8. Important Execution Rule

> **After the current synchronous task finishes, JavaScript drains the microtask queue before moving to the next task/macrotask.**

For example:

```js
setTimeout(() => {
  console.log("Timeout");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise");
});
```

Output:

```text
Promise
Timeout
```

Even though the timeout is registered first, the Promise callback runs first because it is a **microtask**.


## 9. Complete Execution Flow

```text
             JavaScript Execution

                    │
                    ▼
              ┌───────────┐
              │ Call Stack│
              └─────┬─────┘
                    │
             Stack becomes empty
                    │
                    ▼
            ┌───────────────┐
            │Microtask Queue│
            │ Promise       │
            │ queueMicrotask│
            └───────┬───────┘
                    │
              Drain completely
                    │
                    ▼
            ┌───────────────┐
            │  Task Queue   │
            │ setTimeout    │
            │ Events        │
            └───────┬───────┘
                    │
                    ▼
              Back to Event Loop
```


## 10. Quick Comparison

| Concept | Meaning | Examples |
|---|---|---|
| **Call Stack** | Executes current JavaScript functions | Function calls |
| **Event Loop** | Coordinates execution between the stack and queues | — |
| **Event Queue** | General idea of waiting tasks/callbacks | Tasks waiting to execute |
| **Microtask Queue** | High-priority queued callbacks | Promise, `queueMicrotask()` |
| **Macrotask/Task Queue** | Regular asynchronous tasks | `setTimeout`, events |
| **`queueMicrotask()`** | Adds a callback to the microtask queue | `queueMicrotask(fn)` |

### Common Microtasks

```text
Promise.then()
Promise.catch()
Promise.finally()
queueMicrotask()
MutationObserver
```

### Common Tasks/Macrotasks

```text
setTimeout()
setInterval()
DOM events
Some I/O callbacks
```
