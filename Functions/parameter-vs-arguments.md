# Parameter Vs Argument
| Feature                | Parameter                                  | Argument                                      |
| ---------------------- | ------------------------------------------ | --------------------------------------------- |
| Meaning                | Variable defined in a function declaration | Actual value passed when calling the function |
| Where it appears       | Function definition                        | Function call                                 |
| Purpose                | Receives the value                         | Supplies the value                            |
| Exists when            | Function is defined                        | Function is invoked                           |
| Example                | `a`, `b`                                   | `10`, `20`                                    |
| Can have default value | ✅ Yes                                      | ❌ Not applicable                              |
| Example syntax         | `function add(a, b)`                       | `add(10, 20)`                                 |

#### Example

```js
function add(a, b) {
  return a + b;
}

add(10, 20);
```

Here:

```text
a, b     → Parameters
10, 20   → Arguments
```

Visual:

```text
Function definition

function add(a, b) {
             ↑  ↑
        parameters
}


Function call

add(10, 20);
    ↑   ↑
  arguments
```


Note: **arrow functions don't have their own `arguments` object.**

---
### Easy way to remember

- **Parameter = placeholder in the function definition.**
- **Argument = actual value passed during the function call.**

```text
function add(a, b)  → parameters

add(10, 20)         → arguments
```
