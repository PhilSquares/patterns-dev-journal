# 📚 Pattern Journal

## Day 1 – Client-Side Rendering (CSR)  
**📅 Date:** 2025-09-01  
**📂 Category:** Rendering  

---

### 📖 Pattern Summary  

**💡 Problem It Solves:**  
Traditional SSR re-renders full HTML on every request. CSR moves rendering into the browser:  
- 🖥 Server workload decreases.  
- ⚡ Faster in-app navigation.  
- 📊 Scales better for interactive dashboards/feeds.  

**Example (Patterns.dev):**  
```jsx
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById("root"));
}
setInterval(tick, 1000);
```
**🌎 Real-world analogy:**

Think of CSR like building furniture at home vs. buying it pre-assembled:
- SSR (pre-assembled): The store (server) gives you a fully built piece of furniture every time you want one. Convenient, but slower and more resource-intensive for the store.
- CSR (DIY): The store sends you a flat-pack kit (HTML + JS) and instructions (framework/runtime). Your home (browser) assembles the furniture on demand. It may take a moment longer on the first try (initial load), but after that, you can rearrange, add, or update furniture (UI) quickly without needing to go back to the store for a whole new setup.

**✅ Pros & Cons ❌:**

**✅ Pros:**
With React, most of the application logic is executed on the client and it interacts with the server through API calls to fetch or save data. Almost all of the UI is thus generated on the client. The entire web application is loaded on the first request. As the user navigates by clicking on links, no new request is generated to the server for rendering the pages. The code runs on the client to change the view/data.

CSR allows us to have a Single-Page Application that supports navigation without page refresh and provides a great user experience. As the data processed to change the view is limited, routing between pages is generally faster making the CSR application seem more responsive. CSR also allows developers to achieve a clear separation between client and server code.

**❌ Cons:** 
Despite the great interactive experience that it provides, there are a few pitfalls to this CSR.

1. **SEO considerations:** 
Most web crawlers can interpret server rendered websites in a straight-forward manner. Things get slightly complicated in the case of client-side rendering as large payloads and a waterfall of network requests (e.g for API responses) may result in meaningful content not being rendered fast enough for a crawler to index it. Crawlers may understand JavaScript but there are limitations. As such, some workarounds are required to make a client-rendered website SEO friendly.

2. **Performance:** 
With client-side rendering, the response time during interactions is greatly improved as there is no round trip to the server. However, for browsers to render content on client-side the first time, they have to wait for the JavaScript to load first and start processing. Thus users will experience some lag before the initial page loads. This may affect the user experience as the size of JS bundles get bigger and/or the client does not have sufficient processing power.

3. **Code Maintainability:**
Some elements of code may get repeated across client and server (APIs) in different languages. In other cases, clean separation of business logic may not be possible. Examples of this could include validations and formatting logic for currency and date fields.

4. **Data Fetching:** 
With client-side rendering, data fetching is usually event-driven. The page could initially be loaded without any data. Data may be subsequently fetched on the occurrence of events like page-load or button-clicks using API calls. Depending on the size of data this could add to the load/interaction time of the application.

**Note:**
The importance of these considerations may be different across applications. Developers are often interested in finding SEO friendly solutions that can serve pages faster without compromising on the interaction time. Priorities assigned to the different performance criteria may be different based on application requirements. Sometimes it may be enough to use client- side rendering with some tweaks instead of going for a completely different pattern. 

**⚡️ Improving CSR Performance:**

Since performance for CSR is inversely proportional to the size of the JavaScript bundle, the best thing we can do is structure our JavaScript code for optimal performance. Following is a list of pointers that could help.

- **📦 Bundle budgeting:** → keep initial JS < 170KB gzipped. Code can then be loaded on-demand as features are needed.

- **⏩ Preloading:** This technique can be used to preload critical resources that would be required by the page, earlier in the page lifecycle. Critical resources may include JavaScript which can be preloaded by including the following directive in the <head> section of the HTML.

```html
<link rel="preload" as="script" href="critical.js" />
```
This informs the browser to start loading the critical.js file before the page rendering mechanism starts. The script will thus be available earlier and will not block the page rendering mechanism thereby improving the performance.

- **💤 Lazy loading:** With lazy loading, you can identify resources that are non-critical and load these only when needed. Initial page load times can be improved using this approach as the size of resources loaded initially is reduced. For example, a chat widget component would generally not be needed immediately on page load and can be lazy loaded.

- **✂️ Code splitting:** To avoid a large bundle of JavaScript code, you could start splitting your bundles. Code-Splitting is supported by bundlers like [Webpack](https://webpack.js.org/guides/code-splitting/) where it can be used to create multiple bundles that can be dynamically loaded at runtime. Code splitting also enables you to lazy load JavaScript resources.

- **📱 Application Shell Caching via Service Workers:** This technique involves caching the application shell which is the minimal HTML, CSS, and JavaScript powering a user interface. Service workers can be used to cache the application shell offline. This can be useful in providing a native single-page app experience where the remaining content is loaded progressively as needed.

With these techniques, **CSR** can help to provide a faster **Single-Page Application** experience with a decent **First Contentful Paint(FCP)** and **Time to Interactive(TTI)**.

---

## Day 2 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 3 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 4 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 5 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 6 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 7 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 8 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 9 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 10 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 11 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 12 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 13 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 14 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 15 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 16 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 17 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 18 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 19 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 20 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 21 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 22 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 23 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 24 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 25 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 26 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 27 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 28 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 29 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

## Day 30 – Pattern Name
**Date:** YYYY-MM-DD  
**Category:** Rendering / Performance / Design  

### Pattern Summary  
- Problem it solves:
- Example from Patterns.dev:
- Example from a real-world project:
- Pros & cons:

---

<!-- Continue for all 30 days -->