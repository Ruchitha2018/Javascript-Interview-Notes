
# Primitive vs Non-Primitive

| Feature              | Primitive                                                              | Non-Primitive                                             |
| -------------------- | ---------------------------------------------------------------------- | --------------------------------------------------------- |
| Meaning              | Basic/atomic value                                                     | Complex/reference value                                   |
| Types                | `string`, `number`, `bigint`, `boolean`, `undefined`, `null`, `symbol` | `Object`, `Array`, `Function`, `Date`, `Map`, `Set`, etc. |
| Stores               | Value                                                                  | Reference to an object                                    |
| Mutable              | **Immutable**                                                          | Generally **mutable**                                     |
| Can have properties? | No*                                                                    | Yes                                                       |
| Comparison           | Compared by value                                                      | Compared by reference                                     |
| Copy behavior        | Value is copied                                                        | Reference is copied                                       |
| `typeof`             | Returns primitive-specific type                                        | Usually `"object"` or `"function"`                        |
| Examples             | `"hello"`, `42`, `true`                                                | `{}`, `[]`, `function(){}`                                |

* JavaScript may temporarily **box** primitives when accessing methods such as `"hello".toUpperCase()`, but the primitive itself remains a primitive.

### Primitive examples

```js
const name = "John";
const age = 30;
const active = true;
const value = undefined;
const empty = null;
const id = 123n;
const key = Symbol("id");
```

The **7 primitive types** are:

```text
string
number
bigint
boolean
undefined
null
symbol
```

### Non-primitive examples

```js
const user = {
  name: "John"
};

const numbers = [1, 2, 3];

const greet = function () {
  console.log("Hello");
};
```



### Most important interview difference: copying

#### Primitive → value is copied

```js
let a = 10;
let b = a;

b = 20;

console.log(a); // 10
console.log(b); // 20
```

`a` and `b` contain independent values.

#### Object → reference is copied

```js
const user1 = {
  name: "John"
};

const user2 = user1;

user2.name = "Mike";

console.log(user1.name);
// Mike
```

Both variables refer to the **same object**.

```text
user1 ──┐
        ├──> { name: "Mike" }
user2 ──┘
```

### Important nuance

It's better to say:

> **Primitive variables contain primitive values, while variables holding objects contain references to objects.**

Avoid saying simply **"primitives are stored in stack and objects are stored in heap."** That's a common interview oversimplification; JavaScript doesn't specify memory layout that way.
