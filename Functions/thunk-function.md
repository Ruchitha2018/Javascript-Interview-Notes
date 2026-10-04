## Thunk Functions in JavaScript

A **thunk** is a function that **delays the execution of an operation** until you explicitly call the function.

### Simple Example

Instead of executing immediately:

```js
const result = expensiveOperation();
```

we wrap it in a function:

```js
const thunk = () => expensiveOperation();
```

Now the operation is delayed:

```js
// Nothing executes yet

const result = thunk(); // Executes now
```

Think of it as:

```text
Operation
   ↓
Wrap in function
   ↓
Thunk
   ↓
Call later
   ↓
Operation executes
```

---

## Why is it called a Thunk?

A thunk is essentially a **deferred computation**.

For example:

```js
function calculate(a, b) {
  return a + b;
}

// Normal execution
const result = calculate(10, 20);
```

With a thunk:

```js
function createThunk(a, b) {
  return () => calculate(a, b);
}

const thunk = createThunk(10, 20);

// Calculation hasn't happened yet

console.log(thunk());
// 30
```

---

## Thunks with Callbacks

Thunks are useful when you want to delay an operation until a callback is executed.

```js
function createThunk(value) {
  return () => {
    console.log("Processing:", value);
    return value * 2;
  };
}

const thunk = createThunk(10);

console.log("Thunk created");

const result = thunk();

console.log(result);
```

Output:

```text
Thunk created
Processing: 10
20
```

---

## Thunks and Asynchronous Operations

A thunk can also delay an asynchronous operation:

```js
function fetchUserThunk() {
  return function (callback) {
    setTimeout(() => {
      callback({ name: "John" });
    }, 1000);
  };
}

const thunk = fetchUserThunk();

thunk((user) => {
  console.log(user);
});
```

The important idea is that **creating the thunk doesn't perform the operation**. Calling the thunk does.

---

## Thunk vs Callback

These are related but different concepts.

### Callback

A function passed to another function:

```js
setTimeout(() => {
  console.log("Done");
}, 1000);
```

The arrow function is a **callback**.

### Thunk

A function created specifically to **delay a computation**:

```js
const thunk = () => calculate(10, 20);
```

So:

> **Callback = a function passed to another function.**
> **Thunk = a function that represents a delayed computation.**

A function can be **both** a thunk and a callback depending on how it is used.

---

## Thunks in Redux

You may encounter thunks frequently in frontend interviews because of **Redux**.

A normal Redux action is an object:

```js
dispatch({
  type: "FETCH_USERS"
});
```

With Redux Thunk middleware, you can dispatch a function:

```js
dispatch((dispatch) => {
  fetch("/users")
    .then((response) => response.json())
    .then((users) => {
      dispatch({
        type: "USERS_LOADED",
        payload: users
      });
    });
});
```

The function delays the asynchronous work and gives access to `dispatch`.

### Interview Answer

> **A thunk is a function that wraps an operation or computation so that its execution can be delayed until the function is called. Thunks are useful for deferred computation and are commonly seen in asynchronous programming and Redux middleware.**
