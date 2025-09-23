# Day 23 – Module Pattern

## 📄 Summary  
The Module Pattern is a design pattern that splits code into distinct modules, each with its own private and public API. It helps manage complexity by enforcing boundaries.

## 💡 Problem It Solves  
Large codebases can create name collisions, tight coupling, and difficulty in maintaining internal logic. Modules solve this by isolating internal state and exposing only what’s necessary.

## 🌎 Real-world Mapping  
- Many frontend apps structure logic into modules (e.g. `auth`, `api`, `ui`) that encapsulate related functions.  
- In Node.js backend code, each file is already a module; organizing exports properly is the same pattern in practice.  
- When using bundlers, modules help with **tree shaking**: code that is never imported doesn’t get included in the final bundle.

## 🛠 My Mini Implementation  
```javascript
// counterModule.js
const counterModule = (function () {
  // Private state (not exported)
  let count = 0;

  function increment() {
    count++;
  }

  function decrement() {
    count--;
  }

  function getCount() {
    return count;
  }

  // Public API
  return {
    increment,
    decrement,
    getCount
  };
})();

// usage in another file
console.log(counterModule.getCount()); // 0
counterModule.increment();
console.log(counterModule.getCount()); // 1
counterModule.decrement();

// Example using ES Modules
// myModule.js
let secret = 42;

function revealSecret() {
  return secret;
}
function changeSecret(newVal) {
  secret = newVal;
}

export { revealSecret, changeSecret };

// otherFile.js
import { revealSecret, changeSecret } from './myModule.js';
console.log(revealSecret()); // 42
changeSecret(100);
console.log(revealSecret()); // 100
```