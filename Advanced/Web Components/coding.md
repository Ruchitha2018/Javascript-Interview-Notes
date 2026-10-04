# Coding

### HTML
```html
<!DOCTYPE html>
<html>
<head>
  <title>Web Component Example</title>
</head>
<body>

  <h1>Users</h1>

  <user-card
    name="John Doe"
    role="Frontend Developer"
    user-id="101">

    <span slot="company">Google</span>

  </user-card>

  <user-card
    name="Jane Smith"
    role="Backend Developer"
    user-id="102">

    <span slot="company">Microsoft</span>

  </user-card>


  <!-- Template -->
  <template id="user-card-template">

    <style>
      .card {
        width: 250px;
        padding: 15px;
        margin: 10px;
        border: 1px solid #ccc;
        border-radius: 8px;
      }

      .name {
        font-size: 20px;
        font-weight: bold;
      }

      .role {
        color: gray;
      }

      button {
        padding: 8px;
        cursor: pointer;
      }
    </style>

    <div class="card">

      <div class="name">
        <slot name="name"></slot>
      </div>

      <p class="role"></p>

      <p>
        Company:
        <slot name="company"></slot>
      </p>

      <button id="deleteBtn">
        Delete
      </button>

    </div>

  </template>


  <script src="app.js"></script>

</body>
</html>
```

```js
class UserCard extends HTMLElement {

  // 1. Attributes that we want to observe
  static get observedAttributes() {
    return ["name", "role"];
  }


  // 2. Constructor
  constructor() {
    super();

    // Create Shadow DOM
    this.attachShadow({
      mode: "open"
    });

    // Get template
    const template =
      document.getElementById("user-card-template");

    // Copy template into Shadow DOM
    this.shadowRoot.appendChild(
      template.content.cloneNode(true)
    );
  }


  // 3. connectedCallback
  connectedCallback() {

    console.log("User card connected");

    this.render();

    const deleteButton =
      this.shadowRoot.querySelector("#deleteBtn");

    deleteButton.addEventListener(
      "click",
      () => {

        // 4. Custom Event
        this.dispatchEvent(
          new CustomEvent("user-delete", {
            detail: {
              id: this.getAttribute("user-id"),
              name: this.getAttribute("name")
            },
            bubbles: true
          })
        );

      }
    );
  }


  // 5. Attribute changes
  attributeChangedCallback(
    attributeName,
    oldValue,
    newValue
  ) {

    console.log(
      `${attributeName}: ${oldValue} → ${newValue}`
    );

    if (oldValue !== newValue) {
      this.render();
    }
  }


  // 6. Render
  render() {

    if (!this.shadowRoot) {
      return;
    }

    const role =
      this.getAttribute("role") || "Unknown";

    const roleElement =
      this.shadowRoot.querySelector(".role");

    roleElement.textContent = role;
  }


  // 7. JavaScript Property
  set user(value) {

    this._user = value;

    console.log("User property:", value);
  }


  get user() {

    return this._user;
  }


  // 8. disconnectedCallback
  disconnectedCallback() {

    console.log("User card removed");
  }
}


// 9. Register Custom Element
customElements.define(
  "user-card",
  UserCard
);
```