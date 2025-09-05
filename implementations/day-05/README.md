# Day 5 – Streaming Server-Side Rendering

## 📄 Summary  
Streaming SSR allows the server to send HTML to the client in chunks as soon as they are ready instead of waiting for the full page to render. This reduces **time to first byte (TTFB)** and speeds up rendering.  

## 💡 Problem It Solves  
- Traditional SSR can delay rendering since the client must wait for the full HTML.  
- Streaming SSR lets the client start rendering immediately while the rest of the page is still being processed.  

## 🌎 Real-world Mapping  
Like a restaurant serving appetizers before the main course, Streaming SSR sends partial content to the browser so users can start interacting sooner.  

## 🛠 My Mini Implementation  
```javascript
import { renderToNodeStream } from "react-dom/server";
import App from "./src/App";

app.get("/", (req, res) => {
  res.write("<html><body><div id='root'>");

  const stream = renderToNodeStream(<App />);
  stream.pipe(res, { end: false });

  stream.on("end", () => {
    res.end("</div></body></html>");
  });
});
```