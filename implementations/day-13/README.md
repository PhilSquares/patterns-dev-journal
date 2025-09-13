# Day 13 – JavaScript Compression & Minification

## 📄 Summary
This pattern reduces the size of JavaScript files to improve performance by applying two techniques: **minification** (removing unnecessary characters and code) and **compression** (using algorithms like Gzip or Brotli to shrink the file further before sending it over the network).

## 💡 Problem It Solves
Large JavaScript bundles increase load times and hurt performance, especially for users on slow connections or mobile devices. By compressing and minifying code, developers can ensure faster delivery and better overall user experience.

## 🌎 Real-world Mapping
Most production-grade web apps use tools like **Terser**, **UglifyJS**, or **esbuild** for minification, and rely on **CDNs or servers** that apply Gzip or Brotli compression automatically.  
For example, React and Vue apps built with Webpack or Vite include minification and compression in their production builds.

## 🛠 My Mini Implementation
```javascript
// Example: Webpack setup with Terser for minification
const TerserPlugin = require("terser-webpack-plugin");

module.exports = {
  mode: "production",
  optimization: {
    minimize: true,
    minimizer: [new TerserPlugin()],
  },
};
```

```javascript
// Example: enabling Brotli compression in an Express server
import express from "express";
import compression from "compression";

const app = express();

// Apply Brotli/Gzip compression
app.use(compression());

app.use(express.static("dist"));

app.listen(3000, () => {
  console.log("Server running on http://localhost:3000");
});
```