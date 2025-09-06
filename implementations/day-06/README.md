# Day 6 – Progressive Hydration

## 📄 Summary
Progressive Hydration is a rendering pattern where server-rendered HTML is hydrated in steps rather than all at once.  
Critical UI hydrates immediately, while non-critical sections become interactive later.  

## 💡 Problem It Solves
It reduces the long delay before interaction in large server-rendered apps by **prioritizing hydration of important components first**.  

## 🌎 Real-world Mapping
An example is an **e-commerce product page**:  
- The “Add to Cart” button and product images hydrate first.  
- Secondary content like “Reviews” or “Related Products” hydrates afterward.  

## 🛠 My Mini Implementation
```javascript
// Example using React.lazy and Suspense to hydrate progressively

import React, { Suspense, lazy } from "react";
import ReactDOM from "react-dom/client";

// Critical components
import Header from "./Header";
import ProductDetails from "./ProductDetails";

// Lazy-loaded, less-critical components
const Reviews = lazy(() => import("./Reviews"));
const RelatedProducts = lazy(() => import("./RelatedProducts"));

function App() {
  return (
    <>
      <Header />
      <ProductDetails />

      {/* Hydrate non-critical parts later */}
      <Suspense fallback={<div>Loading reviews...</div>}>
        <Reviews />
      </Suspense>

      <Suspense fallback={<div>Loading related products...</div>}>
        <RelatedProducts />
      </Suspense>
    </>
  );
}

const root = ReactDOM.hydrateRoot(document.getElementById("root"), <App />);
```