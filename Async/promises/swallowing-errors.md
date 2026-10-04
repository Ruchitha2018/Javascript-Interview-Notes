# How do you prevent Promises from swallowing errors?

A Promise error can be **"swallowed"** when a rejection occurs but there is no proper rejection handler, or when an error is caught and nothing is done with it.

### 1. Always handle rejected Promises

Avoid:

```js
fetch("/api/users")
  .then((response) => response.json())
  .then((data) => {
    console.log(data);
  });
```

If something rejects and nobody handles the rejection, you can end up with an **unhandled promise rejection**.

Use:

```js
fetch("/api/users")
  .then((response) => response.json())
  .then((data) => {
    console.log(data);
  })
  .catch((error) => {
    console.error("Request failed:", error);
  });
```

---

### 2. Don't silently catch errors

This is a common way of swallowing an error:

```js
somePromise()
  .catch((error) => {
    // do nothing
  });
```

The error has effectively disappeared.

Instead:

```js
somePromise()
  .catch((error) => {
    console.error(error);
  });
```

Or handle it and **rethrow** if the caller also needs to know about the failure:

```js
somePromise()
  .catch((error) => {
    console.error("Logging error:", error);
    throw error;
  });
```

---

### 3. Be careful when returning from `.catch()`

Consider:

```js
getUser()
  .catch((error) => {
    console.error(error);
  })
  .then((user) => {
    console.log(user);
  });
```

The `catch()` handler returns `undefined`.

Therefore, the Promise chain becomes **fulfilled with `undefined`**, and the next `.then()` executes.

If you want the error to continue propagating:

```js
getUser()
  .catch((error) => {
    console.error(error);
    throw error;
  })
  .then((user) => {
    console.log(user);
  });
```

---

### 4. Handle errors with `async/await`

With `async/await`, use `try-catch`:

```js
async function getUsers() {
  try {
    const response = await fetch("/api/users");
    const users = await response.json();

    return users;
  } catch (error) {
    console.error("Failed to get users:", error);
    throw error;
  }
}
```

The `throw error` is important if the caller needs to handle the failure.

---

### 5. Handle the final Promise

A useful pattern is to let lower-level functions propagate errors and handle them at an appropriate boundary:

```js
async function getUser() {
  const response = await fetch("/api/user");
  return response.json();
}

async function main() {
  try {
    const user = await getUser();
    console.log(user);
  } catch (error) {
    console.error("Unable to load user:", error);
  }
}
```

Here:

```text
getUser()
   ↓
error occurs
   ↓
Promise rejects
   ↓
main() catches it
   ↓
error handled
```

---

### Common mistake: forgetting `return`

Another source of confusing Promise behavior is forgetting to return a Promise:


```js
function getUser() {
  fetch("/api/user")
    .then((response) => response.json())
    .then((user) => {
      return user;
    });
}
```

`getUser()` returns `undefined`, not the Promise.

Correct:

```js
function getUser() {
  return fetch("/api/user")
    .then((response) => response.json());
}
```

Now callers can handle the error:

```js
getUser()
  .then((user) => console.log(user))
  .catch((error) => console.error(error));
```

---

### `finally()` doesn't handle errors

`finally()` is useful for cleanup:

```js
fetch("/api/users")
  .then(handleUsers)
  .catch(handleError)
  .finally(() => {
    hideLoader();
  });
```

`finally()` runs whether the Promise is fulfilled or rejected. It is **not a replacement for `catch()`**.

---

### Interview Summary

| Practice                                 | Why                             |
| ---------------------------------------- | ------------------------------- |
| Use `.catch()`                           | Handle Promise rejection        |
| Use `try-catch` with `await`             | Handle async errors             |
| Don't leave `.catch()` empty             | Prevent silent failures         |
| `throw error` after logging              | Continue error propagation      |
| Return Promises from functions           | Allow callers to handle errors  |
| Use `finally()` for cleanup              | Doesn't replace error handling  |
| Handle errors at an appropriate boundary | Avoid duplicated error handling |

### Interview Answer

> **To prevent Promises from swallowing errors, every Promise chain should have an appropriate rejection handler, such as `.catch()` or `try-catch` with `async/await`. Avoid empty catch blocks and, if you log an error but still need the caller to know about it, rethrow it. Also make sure functions return their Promises so errors can propagate through the chain.**
