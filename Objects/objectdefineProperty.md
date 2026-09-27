## `Object.defineProperty()`

`Object.defineProperty()` is used to **create or modify a property on an object with fine-grained control** over its behavior.

### Syntax

```js
Object.defineProperty(object, propertyName, descriptor);
```

Example:

```js
const user = {};

Object.defineProperty(user, "name", {
  value: "John",
  writable: true,
  enumerable: true,
  configurable: true
});

console.log(user.name); // John
```

### Property Descriptors

A property can have these important attributes:

```js
{
  value,
  writable,
  enumerable,
  configurable,
  get,
  set
}
```

#### `value`

Defines the property's value.

```js
Object.defineProperty(user, "name", {
  value: "John"
});
```

#### `writable`

Controls whether the value can be changed.

```js
Object.defineProperty(user, "name", {
  value: "John",
  writable: false
});

user.name = "Mike";

console.log(user.name);
// John
```

#### `enumerable`

Controls whether the property appears during enumeration such as `Object.keys()`.

```js
Object.defineProperty(user, "name", {
  value: "John",
  enumerable: false
});

console.log(Object.keys(user));
// []
```

#### `configurable`

Controls whether the property descriptor can be changed or the property can be deleted.

```js
Object.defineProperty(user, "name", {
  value: "John",
  configurable: false
});

delete user.name;

console.log(user.name);
// John
```


## Using `get` and `set`

`Object.defineProperty()` can also create **accessors**:

```js
const user = {
  firstName: "John",
  lastName: "Doe"
};

Object.defineProperty(user, "fullName", {
  get() {
    return `${this.firstName} ${this.lastName}`;
  },

  set(value) {
    [this.firstName, this.lastName] = value.split(" ");
  }
});

console.log(user.fullName);
// John Doe

user.fullName = "Mike Smith";

console.log(user.firstName);
// Mike

console.log(user.lastName);
// Smith
```

### Data Property vs Accessor Property

There are two types of property descriptors:

**Data descriptor:**

```js
{
  value: "John",
  writable: true
}
```

**Accessor descriptor:**

```js
{
  get() {
    return this._name;
  },

  set(value) {
    this._name = value;
  }
}
```

You generally **cannot mix** data and accessor descriptors:

```js
// ❌ Don't do this
{
  value: "John",
  get() {
    return "Mike";
  }
}
```

### Important Interview Detail

If you omit descriptor attributes, their defaults are generally `false`:

```js
Object.defineProperty(user, "name", {
  value: "John"
});
```

is effectively:

```js
{
  value: "John",
  writable: false,
  enumerable: false,
  configurable: false
}
```

This is an important difference from normal object literal properties:

```js
const user = {
  name: "John"
};
```

Normal properties created this way are typically:

```text
writable:     true
enumerable:   true
configurable: true
```

### Interview Definition

> **`Object.defineProperty()` allows us to define or modify an object's property while explicitly controlling its value, writability, enumerability, configurability, and getter/setter behavior.**
