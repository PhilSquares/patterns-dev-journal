# Day 22 – Factory Pattern

## 📄 Summary  
Factory Pattern is a design pattern that uses factory functions to create objects. Instead of using classes or `new` everywhere, a factory centralizes object creation and configuration, making code easier to change, configure, and maintain.

## 💡 Problem It Solves  
When multiple objects share structure and behavior, writing each one manually leads to duplication. If a field or method needs to change, you’d have to update many places. Factory functions help by centralizing creation logic, reducing duplication and making maintenance easier.

## 🌎 Real-world Mapping  
- A **user registration system** might accept user input from different sources but enforce shared defaults or computed properties (e.g. `fullName`, roles, settings).  
- UI component systems where you create many similar UI widgets (cards, buttons) with configuration options (color, size, behavior).  
- Backend services generating configuration objects or data transfer objects (DTOs) based on environment or permissions.

## 🛠 My Mini Implementation  
```javascript
// createUserFactory.js
function createUser({ firstName, lastName, email, isAdmin = false }) {
  return Object.freeze({  // freeze to avoid unintended mutation
    firstName,
    lastName,
    email,
    isAdmin,
    fullName() {
      return `${this.firstName} ${this.lastName}`;
    },
  });
}

// usage
const user1 = createUser({ firstName: "Alice", lastName: "Smith", email: "alice@example.com" });
const user2 = createUser({ firstName: "Bob", lastName: "Jones", email: "bob@example.com", isAdmin: true });

console.log(user1.fullName());  
console.log(user2);  

// Advanced factory with configuration
function widgetFactory(type, config = {}) {
  const base = { type, createdAt: new Date() };

  if (type === "chart") {
    return Object.freeze({ 
      ...base,
      data: config.data || [],
      render() {
        console.log("Rendering chart with", this.data.length, "points");
      }
    });
  }

  if (type === "button") {
    return Object.freeze({
      ...base,
      label: config.label || "Click me",
      onClick: config.onClick || (() => {}),
    });
  }

  throw new Error(`Unknown widget type: ${type}`);
}

// usage
const chartWidget = widgetFactory("chart", { data: [1, 2, 3] });
const btnWidget = widgetFactory("button", { label: "Submit", onClick: () => alert("Clicked!") });
```