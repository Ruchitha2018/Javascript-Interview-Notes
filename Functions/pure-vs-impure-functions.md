
# Pure vs Impure Functions

| Feature                   | Pure Function | Impure Function   |
| ------------------------- | ------------- | ----------------- |
| Same input → same output  | ✅ Always      | ❌ Not necessarily |
| Side effects              | ❌ No          | ✅ May have        |
| Modifies external state   | ❌ No          | ✅ May             |
| Depends on external state | ❌ No          | ✅ May             |
| Predictable               | ✅             | ❌                 |
| Easy to test              | ✅             | Usually harder    |
| Caching/memoization       | Easy          | Difficult/unsafe  |
| Modifies arguments        | ❌ Should not  | May               |
| Example                   | `add(2, 3)`   | `console.log()`   |

---

### 1. Pure function

A pure function:

1. Produces the same output for the same inputs.
2. Does not cause side effects.

```js
function add(a, b) {
  return a + b;
}

add(2, 3); // 5
add(2, 3); // 5
```

There is no dependency on anything outside the function.

#### Another example

```js
function multiply(price, quantity) {
  return price * quantity;
}
```

```text
Same inputs
    ↓
Same output
    ↓
No external state changed
```

---

### 2. Impure function

An impure function can depend on or modify external state.

```js
let count = 0;

function increment() {
  count++;
  return count;
}
```

```js
increment(); // 1
increment(); // 2
```

Same arguments (none), but different results.

The function depends on external state.

---

### 3. Side effect

A **side effect** means the function does something beyond simply calculating and returning a value.

Examples:

```js
console.log("Hello");        // side effect
document.body.innerHTML = ""; // side effect
localStorage.setItem("x", "1"); // side effect
fetch("/api/users");         // side effect
```

For example:

```js
function saveUser(user) {
  localStorage.setItem("user", JSON.stringify(user));
}
```

This is impure because it modifies external state.

---

### 4. Mutating an argument

This is an important interview example.

### Impure

```js
function updateUser(user) {
  user.name = "John";
  return user;
}
```

The original object is modified.

```js
const user = {
  name: "Mike"
};

updateUser(user);

console.log(user.name);
// John
```

### Pure approach

Create a new object:

```js
function updateUser(user) {
  return {
    ...user,
    name: "John"
  };
}
```

Now:

```js
const user = {
  name: "Mike"
};

const updatedUser = updateUser(user);

console.log(user.name);
// Mike

console.log(updatedUser.name);
// John
```

---

### 5. Important: `const` does NOT make objects immutable

This is still possible:

```js
const user = {
  name: "John"
};

user.name = "Mike";
```

`const` prevents reassignment of the variable; it doesn't make the object immutable.

---

### 6. Why pure functions are useful

Pure functions are particularly useful with:

* Functional programming
* React state updates
* Redux reducers
* Memoization
* Unit testing
* Predictable application logic

For example, a Redux-style reducer should avoid mutating the existing state:

```js
function reducer(state, action) {
  if (action.type === "INCREMENT") {
    return {
      ...state,
      count: state.count + 1
    };
  }

  return state;
}
```

---

### Senior interview answer

> **A pure function always produces the same output for the same inputs and has no side effects. An impure function may depend on external state, modify external state, perform I/O, or produce different results for the same inputs. Pure functions are easier to test, reason about, cache, and compose.**

**One important nuance:** a function doesn't have to be mathematically simple to be pure; what matters is **deterministic output based only on its inputs and no observable side effects**.
