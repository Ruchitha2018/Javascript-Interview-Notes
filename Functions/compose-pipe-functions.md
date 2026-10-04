
# Compose vs Pipe

Both **composition** and **piping** combine multiple functions to create a new function.

Suppose we have:

```js
const add10 = (x) => x + 10;
const double = (x) => x * 2;
const square = (x) => x * x;
```

### Composition

**Compose executes functions from right → left.**

```text
square → double → add10
```

```js
compose(add10, double, square)(2);

// square(2)  → 4
// double(4)  → 8
// add10(8)  → 18
```

Mathematically:

```text
add10(double(square(2)))
```

---

### Pipe

**Pipe executes functions from left → right.**

```text
add10 → double → square
```

```js
pipe(add10, double, square)(2);

// add10(2)  → 12
// double(12) → 24
// square(24) → 576
```

Mathematically:

```text
square(double(add10(2)))
```

---

## Implement `compose`

```js
function compose(...fns) {
  return (value) => {
    return fns.reduceRight(
      (result, fn) => fn(result),
      value
    );
  };
}
```

Usage:

```js
const result = compose(
  add10,
  double,
  square
)(2);

console.log(result);
// 18
```

---

## Implement `pipe`

```js
function pipe(...fns) {
  return (value) => {
    return fns.reduce(
      (result, fn) => fn(result),
      value
    );
  };
}
```

Usage:

```js
const result = pipe(
  add10,
  double,
  square
)(2);

console.log(result);
// 576
```

---

## Visual difference

```text
COMPOSE
--------

compose(add10, double, square)

        2
        ↓
     square
        ↓
        4
        ↓
     double
        ↓
        8
        ↓
      add10
        ↓
       18
```

```text
PIPE
----

pipe(add10, double, square)

        2
        ↓
      add10
        ↓
       12
        ↓
     double
        ↓
       24
        ↓
     square
        ↓
      576
```

### Easy way to remember

> **Compose → right to left**
> **Pipe → left to right**

---

## Real-world example

Suppose we want to process a user's name:

```js
const trim = (str) => str.trim();

const lowercase = (str) => str.toLowerCase();

const addGreeting = (str) => `Hello ${str}`;
```

Using `pipe`:

```js
const formatName = pipe(
  trim,
  lowercase,
  addGreeting
);

console.log(formatName("  JOHN  "));
// Hello john
```

This reads naturally:

```text
trim → lowercase → addGreeting
```

---

## Why is this useful?

Composition/pipe is especially useful with **pure functions**:

```text
Input
  ↓
Function 1
  ↓
Function 2
  ↓
Function 3
  ↓
Output
```

Instead of writing:

```js
const result = fn3(fn2(fn1(value)));
```

you can write:

```js
const process = pipe(fn1, fn2, fn3);

process(value);
```

This improves readability when you have a sequence of transformations.

### Senior interview points

* `compose` → **right → left**
* `pipe` → **left → right**
* Both are examples of **function composition**
* Usually implemented using `reduce()` / `reduceRight()`
* Works especially well with **pure functions**
* Functions should generally have compatible input/output types
* Libraries such as **Ramda**, **Lodash/fp**, and **RxJS** use composition/piping concepts extensively.
