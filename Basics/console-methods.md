# Console Methods

The `console` object provides different methods for **debugging, logging, warnings, errors, and inspecting data**.

| Console Method             | Purpose                                         | Example                                    |
| -------------------------- | ----------------------------------------------- | ------------------------------------------ |
| `console.log()`            | General-purpose logging                         | `console.log("Hello")`                     |
| `console.info()`           | Informational message                           | `console.info("Server started")`           |
| `console.warn()`           | Displays a warning                              | `console.warn("Deprecated API")`           |
| `console.error()`          | Displays an error                               | `console.error("Something went wrong")`    |
| `console.debug()`          | Debug-level message                             | `console.debug(user)`                      |
| `console.table()`          | Displays arrays/objects as a table              | `console.table(users)`                     |
| `console.dir()`            | Inspects an object and its properties           | `console.dir(document.body)`               |
| `console.dirxml()`         | Displays an element's XML/DOM representation    | `console.dirxml(document.body)`            |
| `console.group()`          | Starts a collapsible group                      | `console.group("User")`                    |
| `console.groupEnd()`       | Ends a console group                            | `console.groupEnd()`                       |
| `console.groupCollapsed()` | Starts a collapsed group                        | `console.groupCollapsed("Details")`        |
| `console.count()`          | Counts how many times a point is reached        | `console.count("button")`                  |
| `console.countReset()`     | Resets a counter                                | `console.countReset("button")`             |
| `console.time()`           | Starts a timer                                  | `console.time("API")`                      |
| `console.timeEnd()`        | Stops timer and prints duration                 | `console.timeEnd("API")`                   |
| `console.timeLog()`        | Logs current timer duration without stopping it | `console.timeLog("API")`                   |
| `console.assert()`         | Logs a message if condition is false            | `console.assert(age >= 18, "Invalid age")` |
| `console.trace()`          | Prints the current stack trace                  | `console.trace()`                          |
| `console.clear()`          | Clears the console                              | `console.clear()`                          |

### Common interview examples

**`console.table()`**

```js
const users = [
  { id: 1, name: "John", age: 25 },
  { id: 2, name: "Jane", age: 30 }
];

console.table(users);
```

**`console.time()`**

```js
console.time("loop");

for (let i = 0; i < 1000000; i++) {
  // work
}

console.timeEnd("loop");
```

**`console.assert()`**

```js
const age = 15;

console.assert(age >= 18, "User is underage");
```

**`console.group()`**

```js
console.group("User Details");
console.log("Name: John");
console.log("Age: 25");
console.groupEnd();
```

### Quick interview summary

| Method           | Remember it as           |
| ---------------- | ------------------------ |
| `log`            | Normal output            |
| `info`           | Information              |
| `warn`           | Warning                  |
| `error`          | Error                    |
| `table`          | Tabular data             |
| `dir`            | Inspect object           |
| `time/timeEnd`   | Measure execution time   |
| `count`          | Count executions         |
| `assert`         | Log when condition fails |
| `trace`          | Show call stack          |
| `group/groupEnd` | Organize logs            |
| `clear`          | Clear console            |
