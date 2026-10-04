# Observables in JavaScript

An **Observable** is a pattern for handling a **stream of values over time**.

Unlike a Promise, which generally produces **one final result**, an Observable can produce **zero, one, or many values** over time.

### Simple example

```js
const observable = new Observable((subscriber) => {
  subscriber.next(1);
  subscriber.next(2);
  subscriber.next(3);
});

observable.subscribe((value) => {
  console.log(value);
});
```

Output:

```text
1
2
3
```

> Note: `Observable` is **not built into standard JavaScript**. Libraries such as **RxJS** provide Observable implementations.

---

### Observable vs Promise

| Feature      | Observable                  | Promise                                             |
| ------------ | --------------------------- | --------------------------------------------------- |
| Values       | Multiple values             | Usually one value                                   |
| Execution    | Often lazy                  | Executor starts immediately when Promise is created |
| Cancellation | Can unsubscribe             | No built-in cancellation                            |
| Operators    | Many operators available    | `.then()`, `.catch()`, `.finally()`                 |
| Streaming    | Excellent                   | Not designed for streams                            |
| Errors       | Can emit errors             | Can reject                                          |
| Completion   | Has completion signal       | Settles once                                        |
| Typical use  | Events, WebSockets, streams | HTTP request, one-time async operation              |

### Example: Promise

```js
const promise = fetch("/api/users");

promise.then((response) => {
  console.log(response);
});
```

It produces one eventual result.

### Example: Observable

```js
const clicks$ = fromEvent(button, "click");

clicks$.subscribe(() => {
  console.log("Button clicked");
});
```

Every click can produce a new value:

```text
click → value
click → value
click → value
...
```

---

### Observable lifecycle

An Observable can have three important notifications:

```text
next(value)
   ↓
next(value)
   ↓
next(value)
   ↓
complete()
```

Or:

```text
next(value)
   ↓
next(value)
   ↓
error(error)
```

Once `complete()` or `error()` occurs, the Observable is **closed** and cannot emit further values.

---

### Subscription

You normally consume an Observable using `subscribe()`:

```js
observable.subscribe({
  next: (value) => console.log(value),

  error: (error) => console.error(error),

  complete: () => console.log("Completed")
});
```

---

### Unsubscribe

One important feature is cancellation.

```js
const subscription = observable.subscribe({
  next: (value) => console.log(value)
});

subscription.unsubscribe();
```

After unsubscribing, the subscriber no longer receives values.

This is especially useful for things like:

* WebSocket streams
* DOM events
* timers
* continuous data streams

---

### Observable operators

Libraries such as RxJS provide operators for transforming and combining streams.

Common operators:

| Operator                 | Purpose                             |
| ------------------------ | ----------------------------------- |
| `map()`                  | Transform each value                |
| `filter()`               | Keep values matching a condition    |
| `debounceTime()`         | Wait before emitting                |
| `distinctUntilChanged()` | Ignore duplicate consecutive values |
| `take()`                 | Take a specific number of values    |
| `merge()`                | Combine streams                     |
| `switchMap()`            | Switch to a new inner Observable    |
| `catchError()`           | Handle errors                       |

Example:

```js
const numbers$ = of(1, 2, 3, 4, 5);

numbers$
  .pipe(
    filter((n) => n % 2 === 0),
    map((n) => n * 10)
  )
  .subscribe(console.log);
```

Output:

```text
20
40
```
---
### Coding

#### Example 1
```js
import { fromEvent } from "rxjs";
import {
  map,
  debounceTime,
  distinctUntilChanged,
  filter,
  switchMap
} from "rxjs/operators";

const searchInput = document.querySelector("#search");

const search$ = fromEvent(searchInput, "input").pipe(
  // Get the input value
  map((event) => event.target.value.trim()),

  // Wait until user stops typing for 500ms
  debounceTime(500),

  // Ignore duplicate searches
  distinctUntilChanged(),

  // Don't search for empty strings
  filter((query) => query.length > 0),

  // Make API request
  switchMap((query) =>
    fetch(`/api/users?search=${query}`).then((response) =>
      response.json()
    )
  )
);

// Subscribe to the stream
const subscription = search$.subscribe({
  next: (users) => {
    console.log("Search results:", users);
  },

  error: (error) => {
    console.error("Search failed:", error);
  },

  complete: () => {
    console.log("Search completed");
  }
});
```
---
#### Example 2
```js
const search$ = fromEvent(searchInput, "input").pipe(
  map((event) => event.target.value),
  take(5)
);

search$.subscribe({
  next: (value) => console.log(value),

  complete: () => {
    console.log("Search completed");
  }
});
```
```
input 1 → next
input 2 → next
input 3 → next
input 4 → next
input 5 → next
             ↓
         complete()
```

---
Observable can be synchronous or asynchronous. It depends on how the Observable is created and what it does.
| Observable           | Sync/Async                |
| -------------------- | ------------------------- |
| `of(1, 2, 3)`        | Synchronous               |
| `from([1, 2, 3])`    | Synchronous               |
| `fromEvent()`        | Asynchronous/event-driven |
| `interval()`         | Asynchronous              |
| `timer()`            | Asynchronous              |
| HTTP Observable      | Asynchronous              |
| WebSocket Observable | Asynchronous              |

---

| Observable source               | Scheduling                        | Queue              |
| ------------------------------- | --------------------------------- | ------------------ |
| `of(1, 2, 3)`                   | Synchronous by default            | **Neither**        |
| `from([1, 2, 3])`               | Synchronous by default            | **Neither**        |
| `fromEvent()`                   | Browser event                     | **Task/macrotask** |
| `setTimeout()` based Observable | Timer                             | **Macrotask**      |
| `interval()`                    | Timer                             | **Macrotask**      |
| `Promise` inside Observable     | Promise callback                  | **Microtask**      |
| `queueScheduler`                | Synchronous-like queued execution | Scheduler-specific |
| `asapScheduler`                 | Uses microtask-like scheduling    | **Microtask-like** |

---

### Key interview point

> **An Observable represents a lazy stream of values that can arrive over time. It can emit multiple values, supports cancellation through unsubscription, and provides operators for transforming and combining streams.**

### Observable vs Promise — easiest way to remember

```text
Promise     → one future value
Observable  → stream of future values
```

For example:

```text
HTTP request      → Promise
Button clicks     → Observable
WebSocket messages → Observable
Timer stream      → Observable
```
