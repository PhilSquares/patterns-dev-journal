# Day 7 – Selective Hydration

## 📄 Summary
Selective Hydration is a rendering technique in React 18+ that hydrates parts of a page in a prioritized order. This makes the most important UI interactive first, while less critical components can hydrate later.  

## 💡 Problem It Solves
It solves the delay in interactivity caused by traditional hydration and improves on Progressive Hydration by introducing prioritization. Users can interact with high-priority elements immediately while slower components finish loading in the background.  

## 🌎 Real-world Mapping
- **Social media feeds**: A user can click “like” or navigate before all images and comments finish hydrating.  
- **E-commerce sites**: Navigation and checkout buttons become interactive before full product detail sections load.  

## 🛠 My Mini Implementation
```javascript
import { Suspense, lazy } from "react";
import { hydrateRoot } from "react-dom/client";
import Loader from "./Loader";

const Comments = lazy(() => import("./Comments"));

function App() {
  return (
    <main>
      <Header />
      {/* Comments hydrate later, after Header/Footer */}
      <Suspense fallback={<Loader />}>
        <Comments />
      </Suspense>
      <Footer />
    </main>
  );
}

hydrateRoot(document.getElementById("root"), <App />);
```