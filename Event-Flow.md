# Event Flow, Event Targeting & Event Delegation

## 1. Event Flow

Event flow is the **order in which an event travels through the DOM** when an event happens on an element.

For example:

```html
<div id="parent">
  <button id="child">Click me</button>
</div>
```

When the button is clicked, the event travels through the DOM in **3 phases**:

```text
1. Capturing Phase
        ↓
2. Target Phase
        ↓
3. Bubbling Phase
        ↑
```

### 1.1 Capturing Phase

- Event travels **from the top of the DOM → toward the target**.
- Direction:

```text
window
  ↓
document
  ↓
html
  ↓
body
  ↓
div
  ↓
button
```

- Also called the **capture phase**.
- You can register a listener for capturing using:

```js
parent.addEventListener("click", handler, true);
```

The `true` means the listener runs during the capturing phase.

### 1.2 Target Phase

- Event reaches the **actual element that triggered the event**.
- In our example:

```text
button ← target
```

- The target element's event handler executes here.

### 1.3 Bubbling Phase

- After reaching the target, the event travels **back up the DOM**.
- Direction:

```text
button
  ↑
div
  ↑
body
  ↑
html
  ↑
document
  ↑
window
```

- By default, most `addEventListener` handlers run during the **bubbling phase**.

Example:

```js
parent.addEventListener("click", handler);
```

### Example

```js
parent.addEventListener("click", () => {
  console.log("parent");
});

child.addEventListener("click", () => {
  console.log("child");
});
```

When the button is clicked:

```text
child
parent
```

The parent's handler runs because the event **bubbles up** from the button.

### Capturing vs Bubbling

| Phase | Direction | Default? |
|---|---|---|
| Capturing | Parent → Child | No |
| Target | Actual element | — |
| Bubbling | Child → Parent | Yes |

### Important Interview Points

- **Event flow = Capturing → Target → Bubbling**
- Most event handlers work in the **bubbling phase by default**.
- `addEventListener(type, handler, true)` enables **capturing**.
- `event.target` = element where the event actually originated.
- `event.currentTarget` = element whose event handler is currently executing.
- `event.stopPropagation()` prevents the event from continuing through the propagation path.
- Event bubbling is heavily used for **event delegation**.

### Easy Way to Remember

> **Capture goes down, target gets it, bubble goes up.**

---

## 2. `event.target` vs `event.currentTarget`

These two are very commonly asked together with event bubbling.

### `event.target`

- Refers to the **actual element that triggered the event**.
- It remains the same while the event propagates.
- Think:
  > **"Where did the event originate?"**

### `event.currentTarget`

- Refers to the **element whose event handler is currently executing**.
- It can change as the event bubbles through different elements.
- Think:
  > **"Which element's handler am I currently inside?"**

### Example

```html
<div id="parent">
  <button id="child">Click me</button>
</div>
```

```js
const parent = document.querySelector("#parent");

parent.addEventListener("click", (event) => {
  console.log("target:", event.target);
  console.log("currentTarget:", event.currentTarget);
});
```

If you click the button:

```text
target        → button
currentTarget → div
```

Because:

```text
Button generated the event
        ↓
      target
        ↓
Event bubbled to div
        ↓
div's handler is executing
        ↓
 currentTarget
```

### Interview Shortcut

> **`target` = where event started**  
> **`currentTarget` = where handler is running**

---

## 3. Event Delegation

Event delegation is a technique where we attach **one event listener to a parent instead of adding listeners to every child**.

### Without Event Delegation

```js
const buttons = document.querySelectorAll("button");

buttons.forEach((button) => {
  button.addEventListener("click", handleClick);
});
```

If there are 1,000 buttons, we potentially have 1,000 event listeners.

### With Event Delegation

```js
const container = document.querySelector("#container");

container.addEventListener("click", (event) => {
  if (event.target.matches("button")) {
    console.log("Button clicked");
  }
});
```

Now we have **one listener on the parent**.

```text
container
 ├── button
 ├── button
 ├── button
 └── button
       ↓
   click event
       ↓
   bubbles up
       ↓
   container listener
```

### Why Does Event Delegation Work?

Because of **event bubbling**.

The event starts at the button and bubbles up to the parent.

### Benefits

- Fewer event listeners
- Can reduce memory overhead for large lists
- Works well with **dynamically added elements**
- Simplifies event handling for lists and tables

### Important Caveat

`event.target` might be a **nested element**, not necessarily the element you want.

Example:

```html
<button>
  <span>Delete</span>
</button>
```

If the user clicks the `<span>`:

```js
event.target // span
```

So this can be safer:

```js
container.addEventListener("click", (event) => {
  const button = event.target.closest("button");

  if (!button) return;

  console.log("Button clicked");
});
```


## 4. `stopPropagation()` vs `preventDefault()`

This is a common senior frontend interview follow-up.

### `stopPropagation()`

- Stops the event from propagating further through the DOM.
- Does **not** prevent the browser's default action.

```js
event.stopPropagation();
```

### `preventDefault()`

- Prevents the browser's **default action** associated with the event.
- Does **not** stop event propagation.

```js
event.preventDefault();
```

For example, for a link, `preventDefault()` can prevent navigation.

### Easy Comparison

| Method | What does it stop? |
|---|---|
| `stopPropagation()` | Event propagation |
| `preventDefault()` | Browser's default action |

### One-Liner to Remember

> **`preventDefault()` stops the browser action; `stopPropagation()` stops the event journey.**



## Interview Quick Revision

```text
Event Flow
    ↓
Capturing → Target → Bubbling

target
    → Where the event originated

currentTarget
    → Element whose handler is currently executing

Event Delegation
    → Attach listener to parent
    → Use event bubbling
    → Handle child events from the parent

stopPropagation()
    → Stops event propagation

preventDefault()
    → Stops browser's default action
```
