# Reflow and Repaint

**Reflow** and **repaint** are browser rendering operations that happen when changes to the DOM or CSS require the browser to update what is displayed.

The general rendering process is:

```text
DOM + CSS
   ↓
Style calculation
   ↓
Layout (Reflow)
   ↓
Paint (Repaint)
   ↓
Composite
   ↓
Screen
```

---

### 1. Reflow / Layout

**Reflow** happens when the browser needs to **recalculate the size or position of elements**.

For example:

```js id="x8f5d9a"
element.style.width = "500px";
```

Changing the width can affect the element's layout and potentially other elements, so the browser recalculates layout.

Other examples:

```js id="c6f7wb"
element.style.height = "200px";
element.style.margin = "20px";
element.style.padding = "10px";
```

Adding/removing DOM elements can also trigger layout:

```js id="i7x3cm"
element.appendChild(newElement);
```

### Reflow is relatively expensive

Because changing one element's dimensions can affect other elements:

```text
Change width
    ↓
Recalculate layout
    ↓
Possibly recalculate other elements
    ↓
Paint affected areas
```

---

### 2. Repaint

**Repaint** happens when the browser needs to redraw pixels but the **layout doesn't change**.

For example:

```js id="2awj6m"
element.style.color = "red";
```

Changing text color doesn't normally change the element's size or position.

Other examples:

```js id="d0yq1c"
element.style.backgroundColor = "blue";
element.style.borderColor = "red";
```

Conceptually:

```text
CSS visual change
      ↓
No layout change
      ↓
Repaint
```

---

### Reflow vs Repaint

| Feature                | Reflow                      | Repaint                     |
| ---------------------- | --------------------------- | --------------------------- |
| Also called            | Layout                      | Paint                       |
| Calculates             | Size and position           | Visual pixels               |
| Usually more expensive | ✅                           | Usually less expensive      |
| Can trigger repaint?   | Usually yes                 | No                          |
| Example                | `width`, `height`, `margin` | `color`, `background-color` |

---

### Example

```js id="z7y1tj"
element.style.width = "500px";
```

Potentially:

```text
Style calculation
      ↓
Reflow / Layout
      ↓
Repaint
      ↓
Composite
```

Whereas:

```js id="v7sp4b"
element.style.color = "red";
```

typically:

```text
Style calculation
      ↓
Repaint
      ↓
Composite
```

---

### How to Reduce Reflow/Repaint ?

#### 1. Avoid repeatedly modifying the DOM

❌

```js id="a8f1c3"
for (let i = 0; i < 1000; i++) {
  element.innerHTML += `<div>${i}</div>`;
}
```

Prefer building the content first and making fewer DOM updates.

---

#### 2. Use CSS classes

Instead of changing many styles individually:

```js id="h7m2ak"
element.style.width = "500px";
element.style.height = "200px";
element.style.color = "red";
```

Use:

```js id="3brt7v"
element.classList.add("active");
```

```css
.active {
  width: 500px;
  height: 200px;
  color: red;
}
```

---

#### 3. Batch DOM changes

Make several changes together rather than repeatedly forcing the browser to recalculate layout.

---

#### 4. Be careful with layout reads after writes

This pattern can cause **forced synchronous layout**:

```js id="j6v8gq"
element.style.width = "500px";

console.log(element.offsetWidth);
```

The browser may need to calculate the updated layout immediately to provide `offsetWidth`.

Repeated read/write patterns can cause **layout thrashing**:

```js id="8r0v3n"
for (const element of elements) {
  element.style.width = "500px";
  console.log(element.offsetWidth);
}
```

---

### `transform` and `opacity`

Animations using:

```css
transform: translateX(100px);
opacity: 0.5;
```

are often more rendering-friendly than repeatedly changing layout properties such as:

```css
left: 100px;
width: 200px;
```

because `transform` and `opacity` can often be handled during the **compositing stage**, avoiding layout work.

---

### Interview Answer

> **Reflow (layout) occurs when the browser recalculates the size or position of elements after a layout-affecting change. Repaint occurs when the browser redraws the visual appearance without recalculating layout. Reflow is generally more expensive because it can affect many elements and is typically followed by repainting.**
