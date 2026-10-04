# Custom Elements

Custom Elements are a Web Platform API that lets you create your **own HTML tags** such as `<user-card>`, `<app-button>`, or `<product-card>` with their own behavior and lifecycle.

They are part of **Web Components**.
---
### 1. What are Custom Elements?

Normally HTML gives you built-in elements:

```html
<button>Save</button>
<input type="text">
```

With Custom Elements, you can define your own:

```html
<user-card></user-card>
```

Then JavaScript controls what `<user-card>` does:

```js
class UserCard extends HTMLElement {
  constructor() {
    super();

    this.innerHTML = `
      <h2>John</h2>
      <p>Software Engineer</p>
    `;
  }
}

customElements.define("user-card", UserCard);
```

Now:

```html
<user-card></user-card>
```

will render your custom content.

---

### 2. Basic syntax

The main API is:

```js
customElements.define("element-name", ClassName);
```

Example:

```js
class MyElement extends HTMLElement {
}

customElements.define("my-element", MyElement);
```

HTML:

```html
<my-element></my-element>
```

### Important rule

Custom element names **must contain a hyphen**.

Valid:

```html
<user-card>
<app-button>
<product-list>
<my-component>
```

Invalid:

```html
<user>
<appbutton>
<product>
```

This requirement prevents custom elements from conflicting with future standard HTML elements.

---

### 3. Why extend `HTMLElement`?

You generally create a custom element by extending `HTMLElement`:

```js
class UserCard extends HTMLElement {
}
```

`HTMLElement` gives your class the normal DOM element functionality.

For example:

```js
class UserCard extends HTMLElement {
  constructor() {
    super();

    console.log(this);
  }
}
```

`this` represents the actual:

```html
<user-card></user-card>
```

element.

So you can use:

```js
this.innerHTML
this.textContent
this.setAttribute()
this.getAttribute()
this.addEventListener()
this.classList
```

etc.

---

### 4. Complete example

Let's create:

```html
<user-card></user-card>
```

#### HTML

```html
<!DOCTYPE html>
<html>
<head>
  <title>Custom Element</title>
</head>
<body>

  <user-card></user-card>

  <script src="app.js"></script>
</body>
</html>
```

#### JavaScript

```js
class UserCard extends HTMLElement {

  constructor() {
    super();

    this.innerHTML = `
      <div>
        <h2>John Doe</h2>
        <p>Software Engineer</p>
      </div>
    `;
  }
}

customElements.define("user-card", UserCard);
```

Browser sees:

```html
<user-card></user-card>
```

and creates an instance of:

```js
UserCard
```

---

### 5. Custom Element lifecycle

Custom Elements provide lifecycle callbacks.

The most important ones are:

| Callback                     | When it runs                      |
| ---------------------------- | --------------------------------- |
| `constructor()`              | Element is created                |
| `connectedCallback()`        | Element is added to DOM           |
| `disconnectedCallback()`     | Element is removed                |
| `attributeChangedCallback()` | Observed attribute changes        |
| `adoptedCallback()`          | Element moves to another document |

These are extremely important for interviews and real-world Web Components.

---

### 6. `constructor()`

The constructor runs when the element is created.

```js
class UserCard extends HTMLElement {

  constructor() {
    super();

    console.log("UserCard created");
  }
}

customElements.define("user-card", UserCard);
```

HTML:

```html
<user-card></user-card>
```

You can initialize properties here:

```js
class UserCard extends HTMLElement {

  constructor() {
    super();

    this.name = "John";
    this.role = "Developer";
  }
}

customElements.define("user-card", UserCard);
```

### Important

You must call:

```js
super();
```

before using `this`.

---

### 7. `connectedCallback()`

This is called when the custom element is inserted into the DOM.

Example:

```js
class UserCard extends HTMLElement {

  constructor() {
    super();
  }

  connectedCallback() {
    console.log("UserCard added to DOM");
  }
}

customElements.define("user-card", UserCard);
```

HTML:

```html
<user-card></user-card>
```

Output:

```text
UserCard added to DOM
```

This is commonly where you render UI and attach event listeners.

```js
class UserCard extends HTMLElement {

  connectedCallback() {

    this.innerHTML = `
      <button>Click Me</button>
    `;

    this.querySelector("button")
      .addEventListener("click", () => {
        console.log("Clicked");
      });
  }
}

customElements.define("user-card", UserCard);
```

---

### 8. `disconnectedCallback()`

Called when the element is removed from the DOM.

```js
class UserCard extends HTMLElement {

  connectedCallback() {
    console.log("Connected");
  }

  disconnectedCallback() {
    console.log("Disconnected");
  }
}

customElements.define("user-card", UserCard);
```

If:

```js
const card = document.querySelector("user-card");

card.remove();
```

you'll get:

```text
Disconnected
```

This is useful for cleanup:

```js
disconnectedCallback() {
  window.removeEventListener("resize", this.handleResize);
}
```

You can clean up:

* event listeners
* timers
* subscriptions
* observers
* WebSocket connections

---

### 9. Observing attributes

Suppose you want:

```html
<user-card name="John" role="Developer"></user-card>
```

You can read attributes:

```js
class UserCard extends HTMLElement {

  connectedCallback() {

    const name = this.getAttribute("name");
    const role = this.getAttribute("role");

    this.innerHTML = `
      <h2>${name}</h2>
      <p>${role}</p>
    `;
  }
}

customElements.define("user-card", UserCard);
```

---

### 10. `observedAttributes`

If you want to detect attribute changes, define:

```js
static get observedAttributes() {
  return ["name", "role"];
}
```

Then:

```js
attributeChangedCallback(name, oldValue, newValue) {
  console.log(name);
  console.log(oldValue);
  console.log(newValue);
}
```

Complete example:

```js
class UserCard extends HTMLElement {

  static get observedAttributes() {
    return ["name", "role"];
  }

  attributeChangedCallback(name, oldValue, newValue) {
    console.log(
      `${name}: ${oldValue} → ${newValue}`
    );
  }

  connectedCallback() {
    this.render();
  }

  render() {
    const name = this.getAttribute("name");
    const role = this.getAttribute("role");

    this.innerHTML = `
      <h2>${name}</h2>
      <p>${role}</p>
    `;
  }
}

customElements.define("user-card", UserCard);
```

HTML:

```html
<user-card
  name="John"
  role="Developer">
</user-card>
```

Now:

```js
const card = document.querySelector("user-card");

card.setAttribute("name", "Mike");
```

causes:

```text
name: John → Mike
```

---

### 11. Render when an attribute changes

A more useful implementation:

```js
class UserCard extends HTMLElement {

  static get observedAttributes() {
    return ["name", "role"];
  }

  connectedCallback() {
    this.render();
  }

  attributeChangedCallback() {
    this.render();
  }

  render() {

    const name = this.getAttribute("name") || "Unknown";
    const role = this.getAttribute("role") || "Unknown";

    this.innerHTML = `
      <div class="card">
        <h2>${name}</h2>
        <p>${role}</p>
      </div>
    `;
  }
}

customElements.define("user-card", UserCard);
```

HTML:

```html
<user-card
  name="John"
  role="Frontend Developer">
</user-card>
```

Change it:

```js
const user = document.querySelector("user-card");

user.setAttribute("name", "David");
```

The component automatically re-renders.

---

### 12. Styling a Custom Element

You can style the custom element from normal CSS:

```html
<style>
  user-card {
    display: block;
    border: 1px solid #ccc;
    padding: 20px;
    border-radius: 8px;
  }
</style>
```

And:

```html
<user-card></user-card>
```

But there is an important concept here:

> **Custom Elements and Shadow DOM are separate technologies.**

Custom Elements define the **element/behavior**.

Shadow DOM provides **encapsulation**.

---

### 13. Custom Elements + Shadow DOM

For reusable components, Shadow DOM is very common.

Example:

```js
class UserCard extends HTMLElement {

  constructor() {
    super();

    const shadow = this.attachShadow({
      mode: "open"
    });

    shadow.innerHTML = `
      <style>
        .card {
          padding: 20px;
          border: 1px solid #ccc;
          border-radius: 8px;
        }

        h2 {
          margin: 0;
        }
      </style>

      <div class="card">
        <h2>John Doe</h2>
        <p>Software Engineer</p>
      </div>
    `;
  }
}

customElements.define("user-card", UserCard);
```

HTML:

```html
<user-card></user-card>
```

Now the component has its own DOM tree:

```text
user-card
   │
   └── #shadow-root
         │
         └── .card
              ├── h2
              └── p
```

---

### 14. Why Shadow DOM?

Suppose your application has:

```css
h2 {
  color: red;
}
```

Without Shadow DOM, that CSS could affect your component's `<h2>`.

With Shadow DOM:

```js
this.attachShadow({ mode: "open" });
```

the component's internal DOM is encapsulated.

For example:

```html
<user-card>
  #shadow-root
    <style>
      h2 {
        color: blue;
      }
    </style>

    <h2>John</h2>
</user-card>
```

The application's normal:

```css
h2 {
  color: red;
}
```

doesn't simply override the component's shadow-tree styling.

This is one of the major reasons Web Components are useful for reusable UI components.

---

### 15. `mode: "open"` vs `"closed"`

You can create Shadow DOM using:

```js
this.attachShadow({
  mode: "open"
});
```

or:

```js
this.attachShadow({
  mode: "closed"
});
```

#### Open

```js
const shadow = element.shadowRoot;
```

works.

#### Closed

```js
element.shadowRoot
```

returns:

```js
null
```

because the shadow root isn't exposed through that property.

In most application-level examples, you'll commonly see:

```js
mode: "open"
```

---

### 16. Passing data to a Custom Element

You can use HTML attributes:

```html
<user-card
  name="John"
  role="Software Engineer"
  avatar="/images/john.jpg">
</user-card>
```

JavaScript:

```js
class UserCard extends HTMLElement {

  connectedCallback() {

    const name = this.getAttribute("name");
    const role = this.getAttribute("role");
    const avatar = this.getAttribute("avatar");

    this.innerHTML = `
      <img src="${avatar}" alt="${name}">
      <h2>${name}</h2>
      <p>${role}</p>
    `;
  }
}

customElements.define("user-card", UserCard);
```

---

### 17. Using JavaScript properties

Attributes are strings.

Sometimes you want to pass objects or arrays.

For example:

```js
const card = document.querySelector("user-card");

card.user = {
  name: "John",
  role: "Developer",
  skills: ["JavaScript", "React", "Node.js"]
};
```

Custom element:

```js
class UserCard extends HTMLElement {

  set user(value) {
    this._user = value;
    this.render();
  }

  get user() {
    return this._user;
  }

  render() {

    if (!this._user) return;

    this.innerHTML = `
      <h2>${this._user.name}</h2>
      <p>${this._user.role}</p>
    `;
  }
}

customElements.define("user-card", UserCard);
```

This is useful when your component needs complex JavaScript data.

---

### 18. Custom events

Custom elements can communicate with the parent application using `CustomEvent`.

Example:

```js
class UserCard extends HTMLElement {

  connectedCallback() {

    this.innerHTML = `
      <button>Delete</button>
    `;

    this.querySelector("button")
      .addEventListener("click", () => {

        this.dispatchEvent(
          new CustomEvent("user-delete", {
            detail: {
              id: 101
            },
            bubbles: true
          })
        );

      });
  }
}

customElements.define("user-card", UserCard);
```

Parent application:

```js
const card = document.querySelector("user-card");

card.addEventListener("user-delete", (event) => {

  console.log(event.detail);

});
```

Output:

```js
{
  id: 101
}
```

This is a common pattern:

```text
Parent
   ↓
Custom Element
   ↓
User interaction
   ↓
CustomEvent
   ↓
Parent
```

---

### 19. A complete reusable component

Here's a more realistic example:

```js
class UserCard extends HTMLElement {

  static get observedAttributes() {
    return ["name", "role"];
  }

  constructor() {
    super();

    this.attachShadow({
      mode: "open"
    });
  }

  connectedCallback() {
    this.render();
  }

  disconnectedCallback() {
    console.log("UserCard removed");
  }

  attributeChangedCallback(name, oldValue, newValue) {

    if (oldValue !== newValue) {
      this.render();
    }
  }

  render() {

    const name =
      this.getAttribute("name") || "Unknown";

    const role =
      this.getAttribute("role") || "Unknown";

    this.shadowRoot.innerHTML = `
      <style>
        .card {
          padding: 16px;
          border: 1px solid #ddd;
          border-radius: 8px;
        }

        .name {
          font-size: 20px;
          font-weight: bold;
        }

        .role {
          color: gray;
        }
      </style>

      <div class="card">
        <div class="name">
          ${name}
        </div>

        <div class="role">
          ${role}
        </div>

        <button id="delete">
          Delete
        </button>
      </div>
    `;

    this.shadowRoot
      .querySelector("#delete")
      .addEventListener("click", () => {

        this.dispatchEvent(
          new CustomEvent("user-delete", {
            detail: {
              name
            },
            bubbles: true
          })
        );

      });
  }
}

customElements.define("user-card", UserCard);
```

HTML:

```html
<user-card
  name="John Doe"
  role="Frontend Developer">
</user-card>
```

Parent JavaScript:

```js
document.addEventListener("user-delete", (event) => {
  console.log("Delete:", event.detail);
});
```

---

### 20. Autonomous vs customized built-in elements

There are two major types of custom elements.

#### Autonomous custom element

You create a completely new element:

```html
<user-card></user-card>
```

JavaScript:

```js
class UserCard extends HTMLElement {}

customElements.define("user-card", UserCard);
```

This is the most common approach.

#### Customized built-in element

You extend an existing HTML element.

For example:

```js
class MyButton extends HTMLButtonElement {

  connectedCallback() {
    this.textContent = "Custom Button";
  }
}

customElements.define(
  "my-button",
  MyButton,
  {
    extends: "button"
  }
);
```

HTML:

```html
<button is="my-button"></button>
```

Here you're extending:

```text
HTMLButtonElement
       ↓
MyButton
```

rather than creating a completely new HTML tag.

---

### 21. `customElements.get()`

You can check whether an element has already been registered:

```js
if (!customElements.get("user-card")) {

  customElements.define(
    "user-card",
    UserCard
  );

}
```

This can prevent:

```text
DOMException:
This name has already been registered
```

because a custom element name can only be registered once in a document.

---

### 22. `customElements.whenDefined()`

You can wait until a custom element has been registered:

```js
customElements.whenDefined("user-card")
  .then(() => {
    console.log("user-card is ready");
  });
```

This can be useful when custom elements are loaded asynchronously.

---

### 23. `customElements.upgrade()`

The browser can encounter custom-element markup before its definition is registered.

For example:

```html
<user-card></user-card>
```

before:

```js
customElements.define("user-card", UserCard);
```

The element can initially exist as an unresolved custom element.

When the definition is registered:

```js
customElements.define("user-card", UserCard);
```

the browser upgrades matching elements.

The API also provides:

```js
customElements.upgrade(element);
```

to explicitly upgrade an element/subtree where applicable.

---

### 24. Lifecycle flow

A useful mental model is:

```text
HTML parser / document.createElement()
              │
              ▼
        constructor()
              │
              ▼
      connectedCallback()
              │
              ▼
      attribute changes
              │
              ▼
 attributeChangedCallback()
              │
              ▼
       element.remove()
              │
              ▼
    disconnectedCallback()
```

---

### 25. Custom Element vs Shadow DOM vs Template

These concepts are often confused.

| Technology      | Purpose                                                  |
| --------------- | -------------------------------------------------------- |
| Custom Elements | Create custom HTML elements                              |
| Shadow DOM      | Encapsulate DOM and CSS                                  |
| `<template>`    | Store reusable HTML that isn't rendered immediately      |
| `<slot>`        | Allow content to be passed into a component              |
| Web Components  | Overall platform technology combining these capabilities |

So:

```text
Web Components
│
├── Custom Elements
├── Shadow DOM
├── HTML Templates
└── Slots
```

They can be used independently, but they work particularly well together.

---

### 26. Using `<template>`

Instead of putting a large HTML string inside JavaScript:

```js
this.shadowRoot.innerHTML = `
  ...
`;
```

you can use:

```html
<template id="user-card-template">

  <style>
    .card {
      padding: 20px;
      border: 1px solid #ccc;
    }
  </style>

  <div class="card">
    <h2 class="name"></h2>
    <p class="role"></p>
  </div>

</template>
```

Then:

```js
class UserCard extends HTMLElement {

  constructor() {
    super();

    const shadow = this.attachShadow({
      mode: "open"
    });

    const template =
      document.querySelector("#user-card-template");

    shadow.appendChild(
      template.content.cloneNode(true)
    );
  }

}
```

Register:

```js
customElements.define("user-card", UserCard);
```

---

### 27. Slots

Slots let users provide content to your component.

Component:

```html
<slot></slot>
```

Usage:

```html
<user-card>
  <strong>John Doe</strong>
</user-card>
```

The supplied content appears where:

```html
<slot></slot>
```

is defined.

You can also have named slots:

```html
<slot name="title"></slot>
<slot name="description"></slot>
```

Usage:

```html
<user-card>

  <h2 slot="title">
    John Doe
  </h2>

  <p slot="description">
    Software Engineer
  </p>

</user-card>
```

This makes Web Components much more flexible.

---

### 28. Recommended project structure

For a larger application, you could organize custom elements like:

```text
src/
│
├── components/
│   │
│   ├── user-card/
│   │   ├── user-card.js
│   │   ├── user-card.css
│   │   └── user-card.test.js
│   │
│   ├── app-button/
│   │   ├── app-button.js
│   │   └── app-button.css
│   │
│   └── product-card/
│       ├── product-card.js
│       └── product-card.css
│
├── services/
├── utils/
└── main.js
```

Then:

```js
// main.js

import "./components/user-card/user-card.js";
import "./components/app-button/app-button.js";
import "./components/product-card/product-card.js";
```

---

### 29. Important interview points

Remember these distinctions:

```text
customElements.define()
        ↓
Registers a custom element

HTMLElement
        ↓
Base class for autonomous custom elements

constructor()
        ↓
Initialization

connectedCallback()
        ↓
Element enters DOM

disconnectedCallback()
        ↓
Element leaves DOM

attributeChangedCallback()
        ↓
Observed attribute changes

observedAttributes
        ↓
Attributes you want to observe

attachShadow()
        ↓
Creates Shadow DOM

CustomEvent
        ↓
Component → application communication

slot
        ↓
Application → component content
```

### The big picture

```text
                Web Components
                      │
        ┌─────────────┼─────────────┐
        │             │             │
 Custom Elements   Shadow DOM    Templates
        │             │             │
   <user-card>    Encapsulation   Reusable
        │             │            markup
        └─────────────┼─────────────┘
                      │
                    Slots
                      │
              Content projection
```

The simplest custom element is therefore:

```js
class MyElement extends HTMLElement {

  connectedCallback() {
    this.textContent = "Hello!";
  }

}

customElements.define("my-element", MyElement);
```

used as:

```html
<my-element></my-element>
```

And a production-style Web Component typically combines **Custom Elements + Shadow DOM + lifecycle callbacks + attributes/properties + custom events + slots**.

### Uses

| Benefit                  | Meaning                                                                |
| ------------------------ | ---------------------------------------------------------------------- |
| **Reusable**             | Create once, use many times                                            |
| **Cleaner HTML**         | `<user-card>` is easier to understand                                  |
| **Encapsulation**        | Component can keep its own behavior/styles                             |
| **Reusable across apps** | Web Components work with plain JS and can also be used with frameworks |
| **Maintainable**         | Component logic stays together                                         |

