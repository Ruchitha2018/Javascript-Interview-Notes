## Promises in JavaScript

A **Promise** is an object that represents the **eventual completion or failure of an asynchronous operation**.

In simple terms:

> A Promise represents a value that may be available **now, later, or never**.

### Promise states

A Promise has 3 states:

| State         | Meaning                          |
| ------------- | -------------------------------- |
| **Pending**   | Operation is still in progress   |
| **Fulfilled** | Operation completed successfully |
| **Rejected**  | Operation failed                 |

```text
                 ┌─── Fulfilled
Pending ─────────┤
                 └─── Rejected
```

Once a Promise becomes fulfilled or rejected, it is **settled** and cannot change to another state.



## Creating a Promise

```js
const promise = new Promise((resolve, reject) => {
  const success = true;

  if (success) {
    resolve("Success!");
  } else {
    reject("Something went wrong");
  }
});
```

* `resolve()` → fulfills the Promise
* `reject()` → rejects the Promise



## Consuming a Promise

### `.then()`

Runs when the Promise is fulfilled.

```js
promise.then((result) => {
  console.log(result);
});
```

### `.catch()`

Runs when the Promise is rejected.

```js
promise.catch((error) => {
  console.log(error);
});
```

### `.finally()`

Runs regardless of whether the Promise succeeds or fails.

```js
promise.finally(() => {
  console.log("Operation completed");
});
```



## Example

```js
const fetchUser = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve({ name: "John" });
  }, 1000);
});

fetchUser
  .then((user) => {
    console.log(user.name);
  })
  .catch((error) => {
    console.log(error);
  })
  .finally(() => {
    console.log("Done");
  });
```

After approximately 1 second:

```text
John
Done
```



## Promise Chaining

One of the most important features of Promises is **chaining**.

```js
fetchUser()
  .then((user) => {
    return fetchPosts(user.id);
  })
  .then((posts) => {
    console.log(posts);
  })
  .catch((error) => {
    console.log(error);
  });
```

The value returned from one `.then()` becomes the input to the next `.then()`.

```text
Promise
   ↓
.then()
   ↓
returned Promise/value
   ↓
.then()
   ↓
returned Promise/value
```



## Promise with `async/await`

Promises are commonly consumed using `async/await`:

```js
async function getUser() {
  try {
    const user = await fetchUser();
    console.log(user);
  } catch (error) {
    console.log(error);
  }
}
```

`await` pauses the execution of that **async function** until the Promise settles. It does not block the JavaScript thread.



## Important Promise Methods

### `Promise.all()`

Waits for **all** Promises to fulfill.

```js
const result = await Promise.all([
  fetchUser(),
  fetchPosts(),
  fetchComments()
]);
```

If any Promise rejects, `Promise.all()` rejects.

---

### `Promise.allSettled()`

Waits for **all** Promises to settle, regardless of success or failure.

```js
const result = await Promise.allSettled([
  fetchUser(),
  fetchPosts()
]);
```

Useful when you want the result of every operation.

---

### `Promise.race()`

Settles when the **first Promise settles**—whether fulfilled or rejected.

```js
const result = await Promise.race([
  request1(),
  request2()
]);
```

---

### `Promise.any()`

Fulfills when the **first Promise fulfills**.

```js
const result = await Promise.any([
  request1(),
  request2(),
  request3()
]);
```

If all Promises reject, it rejects with an **`AggregateError`**.

---

### Quick comparison

| Method                 | Completes when | Rejects when                                 |
| ---------------------- | -------------- | -------------------------------------------- |
| `Promise.all()`        | All fulfill    | Any rejects                                  |
| `Promise.allSettled()` | All settle     | Doesn't reject because of individual results |
| `Promise.race()`       | First settles  | First settled Promise rejects                |
| `Promise.any()`        | First fulfills | All reject                                   |

### Interview Answer

> **A Promise is an object representing the eventual result of an asynchronous operation. It has three states—pending, fulfilled, and rejected—and can be handled using `.then()`, `.catch()`, and `.finally()`. Promises can also be consumed using `async/await` and coordinated using methods such as `Promise.all()`, `allSettled()`, `race()`, and `any()`.**
