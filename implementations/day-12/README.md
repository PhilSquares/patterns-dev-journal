# Day 12 – Tree Shaking

## 📄 Summary
Tree Shaking is the process of eliminating unused code from JavaScript bundles, making applications smaller and faster. It is typically implemented in build tools like Webpack, Rollup, and esbuild.  

## 💡 Problem It Solves
Large bundles often contain unused parts of libraries or modules. Tree Shaking analyzes imports/exports and removes code that isn’t referenced, ensuring only what you use ships to the browser.  

## 🌎 Real-world Mapping
For example, if you import a single utility from Lodash but don’t tree-shake, you might accidentally include the entire library. With tree shaking, only that specific function is bundled.  

## 🛠 My Mini Implementation
```javascript
// utils.js
export function add(a, b) {
  return a + b;
}

export function multiply(a, b) {
  return a * b;
}

// main.js
import { add } from './utils.js';

console.log(add(2, 3)); 
// multiply() is never used, so tree shaking will remove it from the final build.
```