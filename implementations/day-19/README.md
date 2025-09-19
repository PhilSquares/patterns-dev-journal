# Day 19 – Web Vitals Monitoring

## 📄 Summary
**Core Web Vitals (LCP, FID, CLS, INP, TTFB)** measure key aspects of web performance that affect user experience. Monitoring them helps developers identify bottlenecks and optimize site speed, responsiveness, and visual stability.  

## 💡 Problem It Solves
Without standardized performance metrics, developers relied on inconsistent tools or subjective judgments. Web Vitals create a shared language for measuring and improving user-centric performance, ensuring that sites are both fast and usable.  

## 🌎 Real-world Mapping
Google Search uses Core Web Vitals as part of its ranking signals. For example, a site with a slow **Largest Contentful Paint (LCP)** may rank lower in search results compared to competitors with faster load times.  

## 🛠 My Mini Implementation
```javascript
// Example: Logging Web Vitals using the `web-vitals` package
// Install with: npm install web-vitals

import { onCLS, onFID, onLCP, onINP, onTTFB } from 'web-vitals';

function reportMetric(metric) {
  console.log(`${metric.name}: ${metric.value}`);
  // You could also send this to Google Analytics or a custom API
}

onCLS(reportMetric);
onFID(reportMetric);
onLCP(reportMetric);
onINP(reportMetric);
onTTFB(reportMetric);
```