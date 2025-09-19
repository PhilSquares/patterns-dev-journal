# Day 18 – Image Optimization

## 📄 Summary
Image Optimization is the practice of reducing image size and choosing efficient formats to deliver faster-loading web pages.

## 💡 Problem It Solves
Large, unoptimized images can significantly slow down websites, negatively affecting user experience and search rankings. Optimizing images helps reduce load time and data usage.

## 🌎 Real-world Mapping
Platforms like **Medium** and **E-commerce sites** aggressively optimize images using formats like WebP/AVIF and CDNs that deliver responsive image sizes. For example, an online store might serve a 200px thumbnail instead of a full 2000px image for product previews.

## 🛠 My Mini Implementation
```javascript
// Example: Serving modern formats with fallbacks
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="Optimized example" width="600" height="400" loading="lazy">
</picture>
```
