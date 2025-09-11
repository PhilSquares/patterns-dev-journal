# Day 10 – Rendering Patterns Recap

## 📄 Summary
A high-level recap of **all rendering patterns** studied in Days 1–9.  
This entry summarizes trade-offs, use-cases, and how the patterns build on each other.

---

## 💡 Problem It Solves
Instead of focusing on one rendering pattern, today’s exercise reviews all patterns side by side.  
This helps cement understanding of **when to use which pattern**.

---

## 📊 Visual Recap
- **CSR → SSR → SSG → ISR → Streaming SSR → Progressive/Selective Hydration → Islands → RSC**  
- Each step evolves to solve limitations of the previous one.  
- Core theme: **better performance + better developer experience**.

---

## 🛠 My Mini Implementation
```javascript
if (staticContent && rarelyUpdated) {
  useSSG();
} else if (dynamicContent && SEOImportant) {
  useSSR();
} else if (dynamicContent && cachingPossible) {
  useISR();
} else if (reactApp && wantStreaming) {
  useStreamingSSR();
} else if (needInteractivityFaster) {
  useSelectiveHydration();
} else if (multi-framework site) {
  useIslandsArchitecture();
} else if (react18 && serverCapabilities) {
  useReactServerComponents();
} else {
  useCSR();
}
```