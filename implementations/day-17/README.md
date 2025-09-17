# Day 17 – Service Workers & Caching Strategies

## 📄 Summary
Service Workers provide a programmable network proxy layer in the browser, allowing developers to intercept requests and implement custom caching logic. Caching strategies define how cached vs. networked responses are prioritized. Combined, they improve performance, reduce redundant requests, and enable offline capabilities.  

## 💡 Problem It Solves
Modern web apps often suffer from slow load times and a lack of offline support. Without caching, even assets that rarely change must be re-downloaded. Service Workers and caching strategies solve this by reusing cached content intelligently and falling back to the network when necessary.  

## 🌎 Real-world Mapping
Think of it like a **grocery list and a stocked pantry**:  
- If you already have rice at home (cache), you don’t buy it again.  
- If you run out of milk (cache miss), you go to the store (network).  
- This saves unnecessary trips and ensures essentials are always available.  

## 🛠 My Mini Implementation
```javascript
// sw.js - A simple Service Worker with a cache-first strategy
const CACHE_NAME = 'v1';
const urlsToCache = [
  '/',
  '/index.html',
  '/styles.css',
  '/script.js'
];

// Install Service Worker and pre-cache assets
self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(CACHE_NAME).then(cache => {
      return cache.addAll(urlsToCache);
    })
  );
});

// Fetch handler with cache-first strategy
self.addEventListener('fetch', event => {
  event.respondWith(
    caches.match(event.request).then(response => {
      // Return cached response if found, else fetch from network
      return response || fetch(event.request);
    })
  );
});
```