# Generators 

A **generator** is a special type of JavaScript function that can:

* Pause its execution.
* Return an intermediate value.
* Resume execution later.
* Maintain its internal state between executions.
* Produce a sequence of values lazily.

A generator is created using `function*`.

```js
function* numbers() {
  yield 1;
  yield 2;
  yield 3;
}
```

Calling `numbers()` **does not execute the function body immediately**.

Instead, it returns a **Generator object**.

```js
const generator = numbers();

console.log(generator);
```

The generator starts executing when you call:

```js
generator.next();
```

---

### 1. Basic Generator Function

Syntax:

```js
function* generatorName() {
  yield value;
}
```

Example:

```js
function* numbers() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = numbers();

console.log(gen.next());
console.log(gen.next());
console.log(gen.next());
console.log(gen.next());
```

Output:

```js
{ value: 1, done: false }
{ value: 2, done: false }
{ value: 3, done: false }
{ value: undefined, done: true }
```

Each call to `.next()` returns an object:

```js
{
  value: ...,
  done: ...
}
```

### `value`

Contains the value produced by `yield`.

### `done`

Indicates whether the generator has finished.

```text
done: false
    ↓
Generator still has work

done: true
    ↓
Generator has completed
```

---

### 2. Generator Execution Flow

Consider:

```js
function* test() {
  console.log("Start");

  yield 10;

  console.log("Middle");

  yield 20;

  console.log("End");

  yield 30;
}

const gen = test();

console.log("A");

console.log(gen.next());

console.log("B");

console.log(gen.next());

console.log("C");

console.log(gen.next());
```

```
A
Start
{ value: 10, done: false }
B
Middle
{ value: 20, done: false }
C
End
{ value: 30, done: false }
```

This is the key characteristic of generators:

> **Execution can be suspended and resumed.**

---

### 3. `yield`

`yield` is used to pause a generator and produce a value.

```js
function* test() {
  yield 10;
  yield 20;
  yield 30;
}
```

Each `yield` pauses execution.

```text
yield 10
   ↓
pause

next()
   ↓
yield 20
   ↓
pause

next()
   ↓
yield 30
   ↓
pause
```

---

### 4. `return` in Generator

A generator can also use `return`.

```js
function* test() {
  yield 1;
  yield 2;
  return 3;
  yield 4;
}
```

```js
const gen = test();

console.log(gen.next());
console.log(gen.next());
console.log(gen.next());
console.log(gen.next());
```

Output:

```js
{ value: 1, done: false }

{ value: 2, done: false }

{ value: 3, done: true }

{ value: undefined, done: true }
```



### Important interview difference

| `yield`                     | `return`               |
| --------------------------- | ---------------------- |
| Pauses generator            | Terminates generator   |
| `done: false`               | `done: true`           |
| Can resume with `.next()`   | Cannot resume normally |
| Produces intermediate value | Produces final value   |

---

### 5. Generator Function with Parameters

Generators can accept parameters just like normal functions.

```js
function* calculate(x) {
  yield x + 10;
  yield x * 2;
  yield x * x;
}

const gen = calculate(5);

console.log(gen.next().value); // 15
console.log(gen.next().value); // 10
console.log(gen.next().value); // 25
```

The parameter is passed when the generator is created:

```js
calculate(5)
```

---

### 6. Passing Values Back into a Generator

This is one of the most important generator features.

The `.next()` method can accept a value.

```js
function* test() {
  const value = yield "Give me a value";

  yield value * 2;
}

const gen = test();

console.log(gen.next());
console.log(gen.next(10));
```

Output:

```js
{ value: "Give me a value", done: false }

{ value: 20, done: false }
```


Conceptually:

```text
Generator
   │
   │ yield "Give me a value"
   ↓
Paused
   │
   │ next(10)
   ↓
yield expression becomes 10
   │
   ↓
value = 10
   │
   ↓
yield 20
```

---

### 7. Generator States

A generator can be thought of as having four important states.

```text
                next()
   ┌─────────────────────────┐
   │                         ↓
Suspended Start ────────► Running
                             │
                             │ yield
                             ↓
                         Suspended
                             │
                             │ next()
                             ↓
                         Running
                             │
                             │ return/end
                             ↓
                         Completed
```

In JavaScript, you can inspect the state using:

```js
generator.next();
```

The returned `done` tells you whether it has completed.

---

### 8. Generator Object

Calling a generator function returns a **generator object**.

```js
function* test() {
  yield 1;
}

const gen = test();
```

`gen` is not the value `1`.

It is an iterator/generator object.

It provides methods such as:

```js
gen.next()
gen.return()
gen.throw()
```

Generator objects are also **iterators** and **iterables**.

---

### 9. Generator as an Iterator

Generators automatically implement the iterator protocol.

An iterator has a:

```js
next()
```

method.

Example:

```js
function* numbers() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = numbers();

console.log(gen.next());
```

Therefore:

```text
Generator
    ↓
Iterator
    ↓
next()
    ↓
{ value, done }
```

---

### 10. Generator and `for...of`

Because generators are iterable, they can be used with `for...of`.

```js
function* numbers() {
  yield 1;
  yield 2;
  yield 3;
}

for (const number of numbers()) {
  console.log(number);
}
```

Output:

```text
1
2
3
```

The `for...of` loop internally keeps calling `.next()`.

Conceptually:

```js
const gen = numbers();

let result = gen.next();

while (!result.done) {
  console.log(result.value);
  result = gen.next();
}
```

---

### 11. Spread Operator with Generator

You can also use the spread operator.

```js
function* numbers() {
  yield 1;
  yield 2;
  yield 3;
}

const result = [...numbers()];

console.log(result);
```

Output:

```js
[1, 2, 3]
```

The spread operator consumes the iterator until:

```js
done === true
```

---

### 12. Generator Expression

A generator function can be assigned to a variable.

```js
const numbers = function* () {
  yield 1;
  yield 2;
  yield 3;
};

const gen = numbers();

console.log(gen.next().value);
```

This is called a **generator function expression**.

Compare:

### Generator declaration

```js
function* numbers() {
  yield 1;
}
```

### Generator expression

```js
const numbers = function* () {
  yield 1;
};
```

---

### 13. Generator Method in an Object

You can define a generator directly as an object method.

```js
const obj = {
  *numbers() {
    yield 1;
    yield 2;
    yield 3;
  }
};

const gen = obj.numbers();

console.log(gen.next().value);
```

The `*` goes before the method name:

```js
*numbers()
```

---

### 14. Generator Method in a Class

Generators can also be defined inside classes.

```js
class Numbers {
  *generate() {
    yield 1;
    yield 2;
    yield 3;
  }
}

const numbers = new Numbers();

const gen = numbers.generate();

console.log(gen.next().value);
```

Output:

```text
1
```

---

### 15. `yield*` — Generator Delegation

`yield*` is used to delegate to another iterable or generator.

Example:

```js
function* first() {
  yield 1;
  yield 2;
}

function* second() {
  yield* first();

  yield 3;
}

console.log([...second()]);
```

Output:

```js
[1, 2, 3]
```

Without `yield*`:

```js
function* second() {
  yield first();
  yield 3;
}
```

The result would contain the generator object itself rather than its yielded values.

With:

```js
yield* first();
```

JavaScript delegates iteration to `first()`.

---

### 16. Multiple Generator Delegation

You can delegate to multiple generators.

```js
function* numbers1() {
  yield 1;
  yield 2;
}

function* numbers2() {
  yield 3;
  yield 4;
}

function* combined() {
  yield* numbers1();
  yield* numbers2();
  yield 5;
}

console.log([...combined()]);
```

Output:

```js
[1, 2, 3, 4, 5]
```

Flow:

```text
combined()
    │
    ├── yield* numbers1()
    │       ├── 1
    │       └── 2
    │
    ├── yield* numbers2()
    │       ├── 3
    │       └── 4
    │
    └── yield 5
```

---

### 17. `generator.return()`

A generator has a `return()` method.

It forces the generator to complete.

```js
function* test() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = test();

console.log(gen.next());
console.log(gen.return("Finished"));
console.log(gen.next());
```

Output:

```js
{ value: 1, done: false }

{ value: "Finished", done: true }

{ value: undefined, done: true }
```

After `return()`:

```text
Generator
    ↓
Completed
```

---

### 18. `generator.throw()`

A generator also has a `throw()` method.

It throws an exception at the point where the generator is currently paused.

Example:

```js
function* test() {
  try {
    yield 1;
    yield 2;
  } catch (error) {
    console.log("Error:", error.message);
  }
}

const gen = test();

console.log(gen.next());

gen.throw(new Error("Something went wrong"));
```

Output:

```text
{ value: 1, done: false }

Error: Something went wrong
```

This can be useful for controlling generator execution and handling errors inside generator workflows.

---

### 19. `try...finally` with Generators

Generators work with `try...finally`.

```js
function* test() {
  try {
    yield 1;
    yield 2;
  } finally {
    console.log("Cleanup");
  }
}

const gen = test();

console.log(gen.next());

gen.return();
```

Output:

```text
{ value: 1, done: false }
Cleanup
```

The `finally` block executes when the generator is closed.

This is useful when a generator controls resources or needs cleanup.

---

### 20. Infinite Generator

One of the powerful features of generators is that they can represent an infinite sequence.

```js
function* infiniteNumbers() {
  let number = 1;

  while (true) {
    yield number++;
  }
}

const gen = infiniteNumbers();

console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
console.log(gen.next().value); // 3
console.log(gen.next().value); // 4
```

The function doesn't generate all numbers at once.

It generates one number when requested.

This is called **lazy evaluation**.

---

### 21. Lazy Evaluation

Normal array:

```js
const numbers = [1, 2, 3, 4, 5];
```

The values are already stored.

A generator:

```js
function* numbers() {
  yield 1;
  yield 2;
  yield 3;
}
```

produces values only when requested.

```text
Request value
      ↓
 generator produces value
      ↓
 pause
      ↓
request next value
      ↓
generator produces next value
```

This can save memory when dealing with large or potentially infinite sequences.

---

### 22. Generator for Large Data

Imagine generating one million numbers.

Using an array:

```js
const numbers = [];

for (let i = 0; i < 1000000; i++) {
  numbers.push(i);
}
```

The entire collection is stored in memory.

A generator can produce values one at a time:

```js
function* numbers() {
  for (let i = 0; i < 1000000; i++) {
    yield i;
  }
}
```

Then:

```js
for (const number of numbers()) {
  // Process one number
}
```

You don't need to create the entire array first.

---

### 23. Async Generators

JavaScript also supports **asynchronous generators**.

Syntax:

```js
async function* generator() {
  yield value;
}
```

Example:

```js
async function* getData() {
  yield await Promise.resolve("Data 1");
  yield await Promise.resolve("Data 2");
  yield await Promise.resolve("Data 3");
}
```

An async generator produces values asynchronously.

---

### 24. Consuming Async Generators

Use:

```js
for await...of
```

Example:

```js
async function main() {
  for await (const data of getData()) {
    console.log(data);
  }
}

main();
```

Output:

```text
Data 1
Data 2
Data 3
```

---

### 25. Async Generator and `next()`

With a normal generator:

```js
const result = gen.next();
```

returns:

```js
{
  value: 1,
  done: false
}
```

With an async generator:

```js
const result = asyncGen.next();
```

returns a **Promise**.

Conceptually:

```js
asyncGen.next()
    ↓
Promise
    ↓
{
  value: ...,
  done: ...
}
```



---

### 26. Normal Generator vs Async Generator

| Feature             | Generator               | Async Generator        |
| ------------------- | ----------------------- | ---------------------- |
| Syntax              | `function*`             | `async function*`      |
| `yield`             | Yes                     | Yes                    |
| `next()`            | Returns iterator result | Returns Promise        |
| Iteration           | `for...of`              | `for await...of`       |
| Asynchronous values | Not directly            | Yes                    |
| Async iterable      | No                      | Yes                    |
| Useful for          | Lazy synchronous data   | Streams/API/async data |

---

### 27. Generator vs Normal Function

| Normal Function               | Generator                          |
| ----------------------------- | ---------------------------------- |
| `function`                    | `function*`                        |
| Executes normally when called | Doesn't execute body immediately   |
| Returns once                  | Can produce multiple values        |
| Cannot pause with `yield`     | Can pause with `yield`             |
| Execution finishes            | Execution can be suspended/resumed |
| Returns a value               | Returns a Generator object         |

Example:

### Normal function

```js
function numbers() {
  return [1, 2, 3];
}
```

### Generator

```js
function* numbers() {
  yield 1;
  yield 2;
  yield 3;
}
```

---

### 28. Generator vs Iterator

These concepts are related but different.

### Iterator

An object that follows the iterator protocol:

```js
{
  next() {
    return {
      value: ...,
      done: ...
    };
  }
}
```

### Generator

A convenient way to create an iterator.

```js
function* numbers() {
  yield 1;
  yield 2;
}
```

So:

```text
Generator
    ↓
creates
    ↓
Generator Object
    ↓
implements
    ↓
Iterator + Iterable
```

---

### 29. Generator vs Async Generator

Think of them like this:

```text
Generator
    ↓
Synchronous values
    ↓
next()
    ↓
{ value, done }
```

Whereas:

```text
Async Generator
    ↓
Asynchronous values
    ↓
next()
    ↓
Promise
    ↓
{ value, done }
```

---

### 30. Practical Example — Pagination

Generators can be useful when processing paginated data.

For example:

```js
async function* fetchPages() {
  let page = 1;

  while (page <= 3) {
    const response = await fetch(`/api/users?page=${page}`);
    const data = await response.json();

    yield data;

    page++;
  }
}
```

Consume it:

```js
async function main() {
  for await (const users of fetchPages()) {
    console.log(users);
  }
}
```

Instead of loading every page immediately, the next page is requested as iteration proceeds.

---

### 31. Practical Example — ID Generator

```js
function* idGenerator() {
  let id = 1;

  while (true) {
    yield id++;
  }
}

const ids = idGenerator();

console.log(ids.next().value); // 1
console.log(ids.next().value); // 2
console.log(ids.next().value); // 3
```

This is a good example of lazy generation.

---

### 32. Practical Example — Custom Range

JavaScript has no built-in `range()` function like some languages, but you can create one.

```js
function* range(start, end) {
  for (let i = start; i <= end; i++) {
    yield i;
  }
}

for (const number of range(1, 5)) {
  console.log(number);
}
```

Output:

```text
1
2
3
4
5
```

---

### 33. Practical Example — Fibonacci

Generators are useful for sequences such as Fibonacci.

```js
function* fibonacci() {
  let a = 0;
  let b = 1;

  while (true) {
    yield a;

    [a, b] = [b, a + b];
  }
}

const gen = fibonacci();

console.log(gen.next().value); // 0
console.log(gen.next().value); // 1
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
console.log(gen.next().value); // 3
console.log(gen.next().value); // 5
```

This can generate an infinite sequence without storing the entire sequence.

---

### 34. Types of Generators — Summary

There aren't separate built-in "generator classes"; rather, there are several **forms and patterns** of generators.

| #  | Type / Pattern              | Example                      | Purpose                               |
| -- | --------------------------- | ---------------------------- | ------------------------------------- |
| 1  | Basic generator declaration | `function* gen()`            | Basic pause/resume                    |
| 2  | Generator expression        | `const gen = function*(){}`  | Assign generator function to variable |
| 3  | Generator method            | `*gen() {}`                  | Generator inside object               |
| 4  | Class generator method      | `*gen() {}`                  | Generator inside class                |
| 5  | Delegating generator        | `yield* gen()`               | Delegate iteration                    |
| 6  | Parameterized generator     | `function* gen(x)`           | Accept initial data                   |
| 7  | Two-way generator           | `gen.next(value)`            | Send data into generator              |
| 8  | Infinite generator          | `while(true) yield`          | Lazy/infinite sequences               |
| 9  | Async generator             | `async function*`            | Asynchronous iteration                |
| 10 | Delegated async generator   | `yield*` with async iterable | Compose async iteration               |

---

### 35. Generator Methods to Remember

For interviews, remember these three:

### `next()`

Resume execution.

```js
gen.next();
```

Can also send a value:

```js
gen.next(100);
```

---

### `return()`

Terminate the generator.

```js
gen.return();
```

---

### `throw()`

Throw an error inside the generator.

```js
gen.throw(new Error("Failed"));
```

So:

```text
Generator
   │
   ├── next()   → resume
   │
   ├── return() → terminate
   │
   └── throw()  → throw error
```

---

### 36. Most Important Interview Example

This is a very common interview concept:

```js
function* test() {
  console.log("A");

  const x = yield 10;

  console.log("B", x);

  const y = yield 20;

  console.log("C", y);

  return 30;
}

const gen = test();

console.log(gen.next());
console.log(gen.next(100));
console.log(gen.next(200));
```

Let's execute it carefully.

### First:

```js
gen.next()
```

Execution:

```text
"A"
yield 10
pause
```

Result:

```js
{
  value: 10,
  done: false
}
```

### Second:

```js
gen.next(100)
```

The `100` becomes:

```js
x = 100
```

Then:

```js
console.log("B", x);
```

Then:

```js
yield 20;
```

Result:

```js
{
  value: 20,
  done: false
}
```

### Third:

```js
gen.next(200)
```

Now:

```js
y = 200
```

Then:

```js
console.log("C", y);
```

Then:

```js
return 30;
```

Result:

```js
{
  value: 30,
  done: true
}
```

---

### 37. Key Concept to Remember

The most important mental model is:

```text
function*()
     ↓
Generator Object
     ↓
next()
     ↓
execute
     ↓
yield
     ↓
pause
     ↓
next(value)
     ↓
resume
     ↓
yield
     ↓
pause
     ↓
next()
     ↓
return/end
     ↓
done: true
```

### One-line definition for interviews

> **A generator is a special JavaScript function that can pause and resume execution using `yield`, returning an iterator that produces values lazily through `next()`.**



Generators are closely related to **iterators, iterables, async iterators, async generators, and `for...of` / `for await...of`**, so those are the next concepts worth studying together.
