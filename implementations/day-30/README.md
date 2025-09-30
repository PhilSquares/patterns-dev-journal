# Day 30 – MVC & MVVM Pattern

## 📄 Summary
**MVC (Model-View-Controller)** and **MVVM (Model-View-ViewModel)** are architectural design patterns that organize code into distinct layers. They separate data (Model), UI (View), and logic/interaction (Controller or ViewModel) to make applications more modular, testable, and scalable.

## 💡 Problem It Solves
Without a structured pattern, UI, data, and business logic get tangled together. MVC and MVVM solve this by creating a **separation of concerns** that makes apps easier to extend, debug, and maintain.

## 🌎 Real-world Mapping
- **MVC:** Classic in web frameworks (e.g., Ruby on Rails, ASP.NET MVC).  
- **MVVM:** Popular in frontend frameworks (e.g., Angular, Vue, Knockout.js, and even React with hooks/state management).  

These patterns underpin the architecture of almost every large-scale application today.

## 🛠 My Mini Implementation

### Example – MVC (JavaScript Simulation)
```javascript
// Model
class TaskModel {
  constructor() {
    this.tasks = [];
  }
  addTask(task) {
    this.tasks.push(task);
  }
  getTasks() {
    return this.tasks;
  }
}

// View
class TaskView {
  displayTasks(tasks) {
    console.log("Tasks:");
    tasks.forEach((task, i) => console.log(`${i + 1}. ${task}`));
  }
}

// Controller
class TaskController {
  constructor(model, view) {
    this.model = model;
    this.view = view;
  }
  addTask(task) {
    this.model.addTask(task);
    this.view.displayTasks(this.model.getTasks());
  }
}

// Usage
const model = new TaskModel();
const view = new TaskView();
const controller = new TaskController(model, view);

controller.addTask("Learn MVC");
controller.addTask("Build something cool");
```