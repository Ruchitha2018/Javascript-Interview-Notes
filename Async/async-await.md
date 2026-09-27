## `async` and `await` in JavaScript

`async` and `await` are JavaScript features that make working with **Promises** easier and more readable.

### 1. `async`

An `async` function **always returns a Promise**.

```js
async function greet() {
  return "Hello";
}

const result = greet();

console.log(result);
// Promise
```

Even though we return a normal string:

```js
return "Hello";
```

JavaScript automatically wraps it in a fulfilled Promise.

```js
greet().then((value) => {
  console.log(value);
});
// Hello
```

---

### 2. `await`

`await` is used to wait for a Promise to settle and obtain its fulfilled value.

```js
async function getData() {
  const result = await fetchData();

  console.log(result);
}
```

Conceptually:

```text
fetchData()
    ↓
  Promise
    ↓
  await
    ↓
result
```

`await` **pauses the execution of the current async function**, but it does **not block the JavaScript thread/event loop**.

---

## Example

```js
function fetchUser() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({ name: "John" });
    }, 1000);
  });
}

async function getUser() {
  const user = await fetchUser();

  console.log(user.name);
}

getUser();
```

After the Promise fulfills:

```text
John
```

---

## Error Handling

Use `try...catch` with `await`:

```js
async function getUser() {
  try {
    const user = await fetchUser();

    console.log(user);
  } catch (error) {
    console.log("Error:", error);
  }
}
```

This is similar to:

```js
fetchUser()
  .then((user) => {
    console.log(user);
  })
  .catch((error) => {
    console.log(error);
  });
```

---

## Sequential vs Parallel `await`

This is an important **senior frontend interview** topic.

### ❌ Sequential

```js
const user = await fetchUser();
const posts = await fetchPosts();
```

If both operations are independent, the second starts only after the first completes.

### ✅ Parallel

```js
const [user, posts] = await Promise.all([
  fetchUser(),
  fetchPosts()
]);
```

Both operations can start without waiting for the other.

```text
Sequential:

fetchUser ────────>
                  fetchPosts ────────>


Parallel:

fetchUser  ─────────>
fetchPosts ─────────>
```

---

## Important Interview Points

### `async` always returns a Promise

```js
async function test() {
  return 10;
}

test().then(console.log);
// 10
```

### `await` works with Promises

```js
const result = await somePromise;
```

It can also be used with non-Promise values:

```js
const result = await 10;

console.log(result);
// 10
```

The value is effectively treated as an already-fulfilled Promise.

### `await` doesn't block the event loop

```js
async function test() {
  console.log("A");

  await somePromise;

  console.log("B");
}

test();

console.log("C");
```

Output:

```text
A
C
B
```

The `async` function pauses at `await`, allowing other JavaScript work to continue.

### Interview Answer

> **`async` and `await` provide a cleaner syntax for working with Promises. An `async` function always returns a Promise, while `await` pauses the execution of the current async function until the awaited Promise settles. It does not block the JavaScript event loop. Errors from awaited Promises can be handled using `try...catch`.**
