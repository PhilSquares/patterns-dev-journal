# Day 11 – Code Splitting (Bundle Splitting, Dynamic Import, Route-Based Splitting)

## 📄 Summary
Code Splitting is a performance pattern where the JavaScript bundle is split into smaller chunks.  
It ensures that only the code needed for the current page or feature is loaded, reducing initial load time.

## 💡 Problem It Solves
- Large JavaScript bundles can slow down page load and delay interactivity.  
- Many features or routes may not be used immediately by the user.  
- Code splitting ensures unnecessary code doesn’t block critical rendering.  

## 🌎 Real-world Mapping
Applications like **Twitter, YouTube, and Slack** all use code splitting:  
- The main feed loads first.  
- Heavy features (video upload, analytics dashboards, admin tools) are loaded only when accessed.  

## 🛠 My Mini Implementation
```javascript
// index.js
import { lazy, Suspense } from "react";
import ReactDOM from "react-dom";

const Profile = lazy(() => import("./Profile"));

function App() {
  return (
    <div>
      <h1>Welcome!</h1>
      <Suspense fallback={<p>Loading profile...</p>}>
        <Profile />
      </Suspense>
    </div>
  );
}

ReactDOM.render(<App />, document.getElementById("root"));
```