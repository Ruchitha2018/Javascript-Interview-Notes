# Heap in JavaScript

The **heap** is a region of memory where JavaScript stores **objects and dynamically allocated data**.

Think of JavaScript memory roughly as:

```text
JavaScript Memory
├── Stack
│   ├── Execution contexts
│   ├── Primitive values / references
│   └── Function calls
│
└── Heap
    ├── Objects
    ├── Arrays
    ├── Functions
    ├── Maps / Sets
    └── Other dynamically allocated data
```

### Example

```js
const user = {
  name: "John",
  age: 30
};
```

Conceptually:

```text
Stack                    Heap
------                   ----------------
user  ────────────────→  { name: "John",
                           age: 30 }
```

`user` holds a **reference** to the object stored in the heap.

### Another example

```js
const user1 = { name: "John" };
const user2 = user1;

user2.name = "Jane";

console.log(user1.name);
```

Output:

```text
Jane
```

Because both variables refer to the **same heap object**:

```text
user1 ──┐
        ├──→ { name: "Jane" }
user2 ──┘
```
---
### Stack vs Heap

| Stack                                       | Heap                                         |
| ------------------------------------------- | -------------------------------------------- |
| Stores execution context and local bindings | Stores dynamically allocated objects/data    |
| Generally fast                              | Generally more complex to manage             |
| Function calls use stack frames             | Objects/arrays/functions typically live here |
| Automatically managed as calls return       | Managed by Garbage Collector                 |
| Limited in size                             | Typically much larger                        |

### Garbage Collection

JavaScript automatically removes heap objects that are **no longer reachable**.

```js
let user = {
  name: "John"
};

user = null;
```

Conceptually:

```text
Before:

user ──→ { name: "John" }


After:

user ──→ null

{ name: "John" }  ← unreachable
                    ↓
              Garbage Collector
                    ↓
                  removed
```

### Important interview point

> **The heap is memory used for dynamically allocated data such as objects, arrays, and functions. JavaScript's garbage collector identifies unreachable objects in the heap and reclaims their memory.**

**One nuance:** the stack-vs-heap distinction is a useful conceptual model, but ECMAScript does not mandate a specific physical memory layout for implementations.
