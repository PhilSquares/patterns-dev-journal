# Day 21 – Singleton Pattern

## 📄 Summary  
Singleton Pattern ensures there is exactly one instance of a resource or manager in your application. It provides global access and ensures shared state or behavior is centralized and consistent.

## 💡 Problem It Solves  
- Prevents duplication of shared resources.  
- Manages global functionality (e.g. logger, configuration, cache) so only one instance is ever used.  
- Helps avoid race conditions or inconsistent state that could arise if multiple instances exist.

## 🌎 Real-world Mapping  
- A **Logger** service in an application that many parts call: every log entry should go through the same instance.  
- A configuration manager or settings module that reads environment settings once and shares across modules.  
- A cache object used across modules: if every module made its own cache, memory and consistency issues could arise.

## 🛠 My Mini Implementation  
```javascript
// singleton.js
let instance = null;

class Logger {
  constructor() {
    if (instance) {
      throw new Error("Only one Logger instance allowed");
    }
    this.logs = [];
    instance = this;
    Object.freeze(this);  // makes this instance immutable
  }

  log(message) {
    this.logs.push(message);
    console.log(`[Logger] ${message}`);
  }

  getLogs() {
    return this.logs;
  }
}

export default Logger;

// usage in other module:
import Logger from './singleton.js';

const logger = new Logger();
logger.log("Hello world!");

// Trying to create another:
try {
  const logger2 = new Logger(); // throws error
} catch (e) {
  console.error(e.message);
}

// simpler module-based singleton (modern JS)
const config = {
  apiKey: "1234",
  environment: "production"
};

Object.freeze(config);

export default config;

// anywhere else:
import config from './config.js';
console.log(config.apiKey);
```