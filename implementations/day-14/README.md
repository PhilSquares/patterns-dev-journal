# Day 14 – Preloading & Prefetching Assets

## 📄 Summary  
**Preloading** and **prefetching** are performance optimization techniques. **Preload** is for resources that must load early (critical path), while **prefetch** is for assets likely needed soon, but not immediately.

## 💡 Problem It Solves  
Delays in loading key resources (fonts, scripts, hero images) can cause poor performance, slow render, or delays in interactivity. Using **preload** and **prefetch** reduces those delays by proactively fetching resources.

## 🌎 Real-world Mapping  
- A website might preload its main font and hero image so the first view displays cleanly without FOIT/FOUT.  
- A single-page app might prefetch code bundles for routes the user is likely to visit next so navigation feels instant.  
- Example: eCommerce site preloading checkout-related scripts after the user adds items to cart, so checkout loads faster.

## 🛠 My Mini Implementation  
```html
<!-- HTML example for preload -->
<head>
  <link rel="preload" href="/fonts/hero-font.woff2" as="font" type="font/woff2" crossorigin>
  <link rel="preload" href="/js/critical-script.js" as="script">
</head>

<body>
  <!-- main content -->
  <script src="/js/noncritical.js" defer></script>
</body>
```

```javascript
// JS example for prefetch with Webpack magic comment
const NextPageComponent = import(/* webpackPrefetch: true */ "./NextPageComponent.js");

// also example using JS to dynamically add prefetch link
const link = document.createElement("link");
link.rel = "prefetch";
link.href = "/js/next-page-bundle.js";
link.as = "script";
document.head.appendChild(link);
```