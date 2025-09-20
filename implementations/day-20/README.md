# Day 20 – Debouncing & Throttling (Extended Performance Pattern)

## 📄 Summary
Debouncing and throttling are performance optimization techniques for controlling how often functions run in response to frequent events.  

## 💡 Problem It Solves
Without these techniques, events like `scroll`, `resize`, or `input` can fire dozens of times per second, causing lag, jank, or unnecessary network requests.  

## 🌎 Real-world Mapping
- **Debounce:** Search bar auto-suggest waiting until the user stops typing.  
- **Throttle:** Limiting scroll event handlers to run every 200ms for smooth animations.  

## 🛠 My Mini Implementation
```javascript
// Debounce: Execute after user stops triggering the event
function debounce(fn, delay) {
  let timeout;
  return (...args) => {
    clearTimeout(timeout);
    timeout = setTimeout(() => fn(...args), delay);
  };
}

// Throttle: Execute at most once in a given interval
function throttle(fn, limit) {
  let inThrottle;
  return (...args) => {
    if (!inThrottle) {
      fn(...args);
      inThrottle = true;
      setTimeout(() => (inThrottle = false), limit);
    }
  };
}

// Example usage
window.addEventListener("resize", debounce(() => {
  console.log("Resized after user stopped resizing");
}, 300));

window.addEventListener("scroll", throttle(() => {
  console.log("Throttled scroll event");
}, 200));
```