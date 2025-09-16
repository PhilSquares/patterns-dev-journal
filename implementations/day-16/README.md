# Day 16 – Route-based Splitting  

## 📄 Summary  
Route-based splitting is a code-splitting strategy where each application route (page) has its own JavaScript chunk. This keeps the initial bundle small and loads additional code only when the user navigates to a new route.  

## 💡 Problem It Solves  
Large single-bundle applications slow down initial load times. By splitting bundles per route, users only download the code necessary for the page they’re currently viewing.  

## 🌎 Real-world Mapping  
In **React**, libraries like `React.lazy` and `React Router` support route-level splitting. Similarly, **Next.js** automatically performs route-based code splitting. For example, navigating between `/dashboard` and `/settings` only loads code relevant to each page, not the entire app upfront.  

## 🛠 My Mini Implementation  
```javascript
import React, { Suspense, lazy } from "react";
import { BrowserRouter as Router, Route, Routes } from "react-router-dom";

// Lazy load components by route
const Home = lazy(() => import("./Home"));
const Dashboard = lazy(() => import("./Dashboard"));
const Settings = lazy(() => import("./Settings"));

function App() {
  return (
    <Router>
      <Suspense fallback={<div>Loading...</div>}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/dashboard" element={<Dashboard />} />
          <Route path="/settings" element={<Settings />} />
        </Routes>
      </Suspense>
    </Router>
  );
}

export default App;
```