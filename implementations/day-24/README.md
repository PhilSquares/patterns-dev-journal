# Day 24 – Observer Pattern

## 📄 Summary  
The Observer Pattern implements a pub-sub style mechanism where observers register interest in changes on a subject. When the subject changes state or emits an event, all observers are notified with relevant data.

## 💡 Problem It Solves  
Without Observer, you'd need to manually call each handler or tightly couple objects that react to changes. Observer automates this decoupled notification process, improving modularity and event flow.

## 🌎 Real-world Mapping  
- UI frameworks where components update when data changes (e.g. a model notifying a view).  
- Messaging systems: subscribers to events or channels.  
- In browser APIs: `EventTarget` / DOM events are a built-in Observer-like abstraction (elements “listen” to events).  
- Custom analytics logging: when particular user actions happen, notifying multiple logging or tracking modules.

## 🛠 My Mini Implementation  
```javascript
// observable.js
class Observable {
  constructor() {
    this.observers = [];
  }

  subscribe(fn) {
    this.observers.push(fn);
  }

  unsubscribe(fn) {
    this.observers = this.observers.filter(observer => observer !== fn);
  }

  notify(data) {
    this.observers.forEach(observer => observer(data));
  }
}

// usage example
const obs = new Observable();

function logger(data) {
  console.log("Logger received:", data);
}

function notifier(data) {
  alert(`Notifier: ${data}`);
}

// Subscribe observers
obs.subscribe(logger);
obs.subscribe(notifier);

// Trigger a change
obs.notify("Event happened!");

// Unsubscribe one
obs.unsubscribe(notifier);

// Trigger again
obs.notify("Another event");

// In React or modern frameworks, observers could be components re-rendering or subscribing to state changes.
```