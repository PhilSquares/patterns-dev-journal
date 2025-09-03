# Day 2 – Server-Side Rendering (SSR)

## 📄 Summary
In **Server-Side Rendering (SSR)**, the server generates the **complete HTML** for a page on each request. This allows the browser to display meaningful content immediately, without waiting for client-side JavaScript execution.

## 💡 Problem It Solves
- Eliminates the **blank page problem** of CSR (waiting for JS to render).  
- Improves **first load performance** and **SEO** by delivering pre-rendered HTML.  
- Ensures consistent rendering across devices with different performance levels.  

## 🌎 Real-world Mapping
Think of SSR like a **restaurant kitchen**: the server (kitchen) delivers a **fully cooked meal (HTML)** to the customer (browser). The user can enjoy it instantly, unlike CSR where they’d get raw ingredients and instructions.

## 🛠 My Mini Implementation
- **Classic SSR Implementation:**
```html
<!DOCTYPE html>
<html>
   <head>
       <title>Time</title>
   </head>
   <body>
       <div>
       <h1>Hello, world!</h1>
       <b>It is <div id=currentTime></div></b>
       </div>
   </body>
</html>
```
```javascript
function tick() {
    var d = new Date();
    var n = d.toLocaleTimeString();
    document.getElementById("currentTime").innerHTML = n;
}
setInterval(tick, 1000);
```

📌 Note:
- Here, the HTML is pre-rendered on the server.
- The time displayed comes from client-side JavaScript (tick()).
- If you want server time, it must be embedded into the HTML before sending it — requiring a full page reload to update.

- **Modern SSR Example (React Hydration):**
```javascript
// Server
import ReactDOMServer from "react-dom/server";
import App from "./App";

const html = ReactDOMServer.renderToString(<App />);
res.send(html);

// Client
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.hydrate(<App />, document.getElementById("root"));

```

**✅ This approach gives:**
- Fast first paint (HTML from server).
- Interactive client-side UI after hydration.

**🎯 Key Takeaways:**
- SSR is great for SEO and fast first load.
- Works best for content-heavy sites.
- Comes with trade-offs: slower TTFB and less SPA-like interactivity.
- Modern frameworks combine SSR + CSR to balance speed and interactivity.