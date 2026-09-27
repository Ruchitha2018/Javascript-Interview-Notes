# JavaScript Error Names

JavaScript provides several built-in error types. The `name` property of an error object identifies which type of error occurred.

| Error Name           | When it occurs                                                     | Example                                   |
| -------------------- | ------------------------------------------------------------------ | ----------------------------------------- |
| **`Error`**          | Generic runtime error                                              | `throw new Error("Something went wrong")` |
| **`EvalError`**      | Related to `eval()` errors (rare in modern JS)                     | `throw new EvalError("Invalid eval")`     |
| **`RangeError`**     | A value is outside the allowed range                               | `new Array(-1)`                           |
| **`ReferenceError`** | Referencing a variable that doesn't exist                          | `console.log(x)`                          |
| **`SyntaxError`**    | Invalid JavaScript syntax                                          | `JSON.parse("{")`                         |
| **`TypeError`**      | A value is used in an inappropriate way/type                       | `null.foo`                                |
| **`URIError`**       | Invalid URI encoding/decoding                                      | `decodeURIComponent("%")`                 |
| **`AggregateError`** | Represents multiple errors together, commonly from `Promise.any()` | `new AggregateError([err1, err2])`        |

### Example

```js
try {
  console.log(unknownVariable);
} catch (error) {
  console.log(error.name);
  // ReferenceError

  console.log(error.message);
  // unknownVariable is not defined
}
```

### Error Object Structure

```js
const error = new TypeError("Invalid value");

console.log(error.name);
// TypeError

console.log(error.message);
// Invalid value

console.log(error.stack);
// Stack trace
```

**Interview tip:** The most commonly encountered built-in errors are:

```text
TypeError
ReferenceError
SyntaxError
RangeError
URIError
```

`Error` is the base/general error type, while the others provide more specific error categories.
