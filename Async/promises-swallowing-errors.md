# Promises from Swallowing Errors

To prevent **Promises from swallowing errors**, make sure you **return/rethrow errors from `.catch()`** and always handle the final rejected promise.

### 1. The common mistake

```js
getUser()
  .catch((error) => {
    console.error(error);
  })
  .then((user) => {
    console.log(user);
  });
```

The problem is that `.catch()` **returns `undefined`**.

So the chain effectively becomes:

```text
getUser()
   ↓
rejected
   ↓
catch()
   ↓
returns undefined
   ↓
Promise becomes fulfilled
   ↓
then(undefined) executes
```

So the error has been **swallowed**.

---

### 2. Rethrow the error

If you want the error to continue propagating:

```js
getUser()
  .catch((error) => {
    console.error("Logging error:", error);
    throw error;
  })
  .then((user) => {
    console.log(user);
  })
  .catch((error) => {
    console.error("Final error:", error);
  });
```

Now:

```text
getUser()
   ↓
reject
   ↓
catch()
   ↓
throw error
   ↓
reject
   ↓
final catch()
```

### 3. Return a rejected Promise

Another way:

```js
getUser()
  .catch((error) => {
    console.error(error);
    return Promise.reject(error);
  })
  .then((user) => {
    console.log(user);
  });
```

Usually, `throw error` is simpler.

---

### 4. `async/await`

With `async/await`, use `try/catch`:

```js
async function loadUser() {
  try {
    const user = await getUser();
    return user;
  } catch (error) {
    console.error("Failed to get user:", error);
    throw error; // Don't swallow it
  }
}
```

Then handle it at the appropriate boundary:

```js
loadUser()
  .catch((error) => {
    console.error("Handled at boundary:", error);
  });
```

---

### Important interview point

**A `.catch()` handler does not automatically rethrow an error.**

```js
.catch(error => {
  console.error(error);
});
```

means:

> "I handled this error, and unless I throw/return a rejected Promise, the chain continues as fulfilled."

Whereas:

```js
.catch(error => {
  console.error(error);
  throw error;
});
```

means:

> "Log the error, but keep the Promise rejected."

### Rule of thumb

| Situation               | What to do                   |
| ----------------------- | ---------------------------- |
| Error is fully handled  | Return normally              |
| Error should propagate  | `throw error`                |
| Need to transform error | `throw new CustomError(...)` |
| Need fallback value     | `return fallbackValue`       |
| Final error boundary    | `.catch(...)`                |

**Senior interview one-liner:**

> "Promises swallow errors when a rejection handler catches the error and returns normally. To preserve propagation, rethrow the error or return a rejected Promise, and make sure there's a final error boundary."
