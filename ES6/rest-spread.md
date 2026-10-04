# Rest and Spread Operators 

Both use the same syntax: `...`, but they do different jobs.

> **Spread = expand/unpack values**  
> **Rest = collect/pack values**


## 1. Spread Operator `...`

The **spread operator expands** an iterable or object into individual elements/properties.

### Array example

```js
const numbers = [1, 2, 3];

const newNumbers = [...numbers];

console.log(newNumbers);
// [1, 2, 3]
```

Think:

```text
[1, 2, 3]
    ↓ spread
1, 2, 3
```


### 2. Spread with Arrays

### Combine arrays

```js
const a = [1, 2];
const b = [3, 4];

const result = [...a, ...b];

console.log(result);
// [1, 2, 3, 4]
```

### Add values while copying

```js
const numbers = [2, 3];

const result = [1, ...numbers, 4];

console.log(result);
// [1, 2, 3, 4]
```


### 3. Spread with Objects

```js
const user = {
  name: "John",
  age: 30
};

const copy = {
  ...user
};

console.log(copy);
// { name: "John", age: 30 }
```

### Merge objects

```js
const user = {
  name: "John"
};

const details = {
  age: 30
};

const result = {
  ...user,
  ...details
};

console.log(result);
// { name: "John", age: 30 }
```


### 4. Property Overriding

This is a common interview question.

```js
const user = {
  name: "John",
  age: 30
};

const updatedUser = {
  ...user,
  age: 35
};

console.log(updatedUser);
```

Output:

```js
{
  name: "John",
  age: 35
}
```

The **later property wins**.

```js
const result = {
  ...obj1,
  ...obj2
};
```

If both contain the same property:

```text
obj2 value wins
```



### 5. Spread Is Shallow

This is very important.

```js
const user = {
  name: "John",
  address: {
    city: "Mumbai"
  }
};

const copy = {
  ...user
};

copy.name = "Jane";
copy.address.city = "Pune";

console.log(user);
```

Result:

```js
{
  name: "John",
  address: {
    city: "Pune"
  }
}
```

Why?

The spread operator creates a **shallow copy**.

```text
user
 ├── name ───────→ "John"
 └── address ────→ Object A
                         ↑
                         │
copy.address ────────────┘
```

Both objects reference the **same nested object**.



### 6. Spread and Function Arguments

Spread can convert an array into individual function arguments.

```js
const numbers = [10, 20, 30];

Math.max(...numbers);
```

Equivalent to:

```js
Math.max(10, 20, 30);
```

Another example:

```js
function add(a, b, c) {
  return a + b + c;
}

const numbers = [10, 20, 30];

console.log(add(...numbers));
// 60
```



### 7. Spread with Strings

Strings are iterable.

```js
const name = "John";

console.log([...name]);
```

Output:

```js
["J", "o", "h", "n"]
```


### 8. Spread with Sets

```js
const set = new Set([1, 2, 3]);

const array = [...set];

console.log(array);
// [1, 2, 3]
```

This is also commonly used to remove duplicate values:

```js
const numbers = [1, 2, 2, 3, 3];

const unique = [...new Set(numbers)];

console.log(unique);
// [1, 2, 3]
```


## 9. Rest Operator `...`

The **rest operator collects multiple values into a single array/object**.

### Function parameters

```js
function sum(...numbers) {
  console.log(numbers);
}

sum(1, 2, 3, 4);
```

Output:

```js
[1, 2, 3, 4]
```

Think:

```text
1, 2, 3, 4
      ↓ rest
[1, 2, 3, 4]
```


### 10. Rest Parameters

```js
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3));
// 6

console.log(sum(10, 20, 30, 40));
// 100
```

Important:

> Rest parameters collect remaining arguments into an **actual array**.



### 11. Rest with Normal Parameters

You can have regular parameters before rest.

```js
function test(first, second, ...remaining) {
  console.log(first);
  console.log(second);
  console.log(remaining);
}

test(1, 2, 3, 4, 5);
```

Output:

```text
1
2
[3, 4, 5]
```

### Important rule

Rest parameter must be the **last parameter**.

Valid:

```js
function test(a, b, ...rest) {}
```

Invalid:

```js
function test(...rest, a) {}
```


### 12. Rest in Array Destructuring

```js
const numbers = [1, 2, 3, 4, 5];

const [first, second, ...rest] = numbers;

console.log(first);
// 1

console.log(second);
// 2

console.log(rest);
// [3, 4, 5]
```


### 13. Rest with Object Destructuring

Very useful in React and frontend development.

```js
const user = {
  name: "John",
  age: 30,
  city: "Mumbai"
};

const { name, ...otherDetails } = user;

console.log(name);
// John

console.log(otherDetails);
// { age: 30, city: "Mumbai" }
```

This is useful when you want to extract specific properties and keep the remaining properties.



### 14. Rest vs Spread

This is one of the most common interview questions.

Both use:

```js
...
```

But the context determines whether it is rest or spread.

### Spread

**Expands** values.

```js
const numbers = [1, 2, 3];

const result = [...numbers];
```

Think:

```text
[1, 2, 3]
 ↓ ↓ ↓
 1 2 3
```

### Rest

**Collects** values.

```js
function test(...numbers) {}
```

Think:

```text
1, 2, 3
 ↓ ↓ ↓
[1, 2, 3]
```

### Easy memory trick

```text
SPREAD → Spread OUT
REST   → Gather the REST
```


### 15. Side-by-Side Example

### Spread

```js
const numbers = [1, 2, 3];

console.log(Math.max(...numbers));
```

`...numbers` expands:

```text
[1, 2, 3]
     ↓
1, 2, 3
```

### Rest

```js
function test(...numbers) {
  console.log(numbers);
}

test(1, 2, 3);
```

`...numbers` collects:

```text
1, 2, 3
   ↓
[1, 2, 3]
```

### 18. Spread Does Not Deep Clone

This is a very common senior interview discussion.

```js
const original = {
  name: "John",
  address: {
    city: "Mumbai"
  }
};

const copy = { ...original };

console.log(original === copy);
// false

console.log(original.address === copy.address);
// true
```

The top-level object is different:

```text
original !== copy
```

But nested reference is shared:

```text
original.address === copy.address
```

Therefore:

```js
copy.address.city = "Pune";
```

also changes:

```js
original.address.city
```



### 19. Spread vs `Object.assign()`

These are often compared.

### Spread

```js
const result = {
  ...obj1,
  ...obj2
};
```

### `Object.assign()`

```js
const result = Object.assign({}, obj1, obj2);
```

Both create a **shallow copy** in this common usage.

One important difference:

```js
const target = {};

Object.assign(target, source);
```

`Object.assign()` modifies the target object.

Spread creates a new object:

```js
const result = {
  ...source
};
```


### 20. Spread in React

Spread is heavily used with React props and immutable state updates.

### Props

```jsx
<User {...user} />
```

Equivalent to passing the object's properties as props.

### State update

```js
setUser({
  ...user,
  name: "Jane"
});
```

This creates a new top-level object instead of directly mutating the existing state object.



## 21. Rest in React

Rest is useful when passing remaining props.

```jsx
function Button({ title, ...rest }) {
  return (
    <button {...rest}>
      {title}
    </button>
  );
}
```

Usage:

```jsx
<Button
  title="Save"
  className="primary"
  disabled
/>
```

Here:

```js
title
```

is extracted, while:

```js
rest = {
  className: "primary",
  disabled: true
}
```

Then:

```jsx
<button {...rest}>
```

passes those remaining properties to the button.



## 22. Senior Interview Cheat Sheet

```text
             ...
              │
       ┌──────┴──────┐
       ↓             ↓
    SPREAD          REST
       ↓             ↓
   Expand          Collect
       ↓             ↓
[1,2,3] → 1,2,3   1,2,3 → [1,2,3]
```

### Spread

Used for:

- Copying arrays
- Merging arrays
- Copying objects
- Merging objects
- Passing array elements as function arguments
- React props
- Immutable state updates

```js
const copy = [...arr];

const obj = { ...user };

Math.max(...numbers);
```

### Rest

Used for:

- Function parameters
- Array destructuring
- Object destructuring
- Collecting remaining values/properties

```js
function sum(...numbers) {}

const [first, ...rest] = numbers;

const { name, ...details } = user;
```

## Most Important Points

- Both use `...`.
- **Spread expands/unpacks.**
- **Rest collects/packs.**
- Spread copies are **shallow**.
- Rest parameters produce an actual **array**.
- Rest parameter must be the **last function parameter**.
- Object/array destructuring can use rest.
- Spread is heavily used for immutable updates in frontend frameworks like React.
