# Synchronous vs Asynchronous HTTP Requests in JavaScript

## 1. Synchronous HTTP Request

A **synchronous HTTP request blocks JavaScript execution** until the HTTP response is received.

```js
const xhr = new XMLHttpRequest();

xhr.open("GET", "/api/users", false); // false = synchronous

xhr.send();

console.log(xhr.responseText);
console.log("This runs after the response is received");
```

### Flow

```text
JavaScript
   ↓
Send HTTP request
   ↓
Wait for response
   ↓
Response received
   ↓
Continue execution
```

### Problems with synchronous requests

- Blocks the **main thread**.
- Freezes the UI while waiting.
- User interactions may become unresponsive.
- Can make the application feel slow.
- Synchronous XHR on the main thread is deprecated and should generally be avoided.


## 2. Asynchronous HTTP Request

An **asynchronous HTTP request does not block JavaScript execution**.

The browser starts the request and JavaScript continues executing other code.

### Using `fetch()`

```js
fetch("/api/users")
  .then(response => response.json())
  .then(data => {
    console.log(data);
  });

console.log("This runs immediately");
```

Typical output:

```text
This runs immediately
{ user data }
```

### Flow

```text
JavaScript
   ↓
Send HTTP request
   ↓
Continue executing other code
   ↓
Response arrives later
   ↓
Promise callback executes
```


## 3. Asynchronous Request Using `async/await`

```js
async function getUsers() {
  const response = await fetch("/api/users");
  const data = await response.json();

  console.log(data);
}

getUsers();

console.log("This can run before the response arrives");
```

### Important Interview Point

`await` makes the **current async function wait for the Promise**, but it **does not block the browser's main thread**.


## 4. Key Differences

| Feature | Synchronous | Asynchronous |
|---|---|---|
| Blocks JavaScript execution | Yes | No |
| Blocks main UI thread | Yes, if done on main thread | No |
| UI can remain responsive | No | Yes |
| Recommended for modern web apps | No | Yes |
| `fetch()` | No | Yes |
| `async/await` | No | Yes |
| XMLHttpRequest | Can be either | Can be either |



## 5. Interview Answer

> **A synchronous HTTP request blocks JavaScript execution until the response is received, while an asynchronous HTTP request allows JavaScript to continue executing and handles the response later. In modern web applications, asynchronous requests using `fetch`, Promises, or `async/await` are preferred because they don't block the main thread.**



## 6. Senior-Level Point

Don't confuse **asynchronous HTTP** with **multithreading**.

JavaScript can remain responsive while an HTTP request is in progress because the browser handles networking outside the JavaScript execution stack and schedules the result back through the event loop.


## Quick Cheat Sheet

```text
Synchronous
    ↓
Send request
    ↓
BLOCK / WAIT
    ↓
Response
    ↓
Continue

Asynchronous
    ↓
Send request
    ↓
Continue executing
    ↓
Response arrives
    ↓
Callback / Promise / async-await continues
```

### Remember

**Synchronous = Wait**

**Asynchronous = Don't block; handle the result later**
