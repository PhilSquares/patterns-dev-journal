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

## Day 2 – Server-Side Rendering (SSR)
**📅 Date:** 2025-09-02  
**📂 Category:** Rendering

### 📖 Pattern Summary 

**💡 Problem It Solves:**  
- Before SSR, most dynamic applications required JavaScript to run in the browser before showing useful content (CSR).  
- SSR solves the **blank page problem** (empty `<div id="root"></div>` until JS loads).  
- It improves **performance for the first render** and ensures better **SEO** since search engine crawlers can index pre-rendered HTML.

**Example (Patterns.dev):**  
```html
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

**🌎 Real-world analogy:**
Think of SSR like ordering food at a restaurant:
- 🍽️ The kitchen (server) prepares a complete dish (HTML) before serving.
- 🧑‍🍳 By the time it reaches you (browser), it’s ready to eat immediately.
- CSR, in contrast, would be like the restaurant delivering raw ingredients and a recipe for you to cook yourself.

**✅ Pros & Cons ❌:**

**✅ Pros:**
Executing the rendering code on the server and reducing JavaScript offers the following advantages: 

1. **Better First Contentful Paint (FCP) & Time to Interactive (TTI):** 
- Less JavaScript required upfront.
- Users see meaningful content faster.

2. **Improved SEO:** 
- Crawlers index pre-rendered HTML without extra workarounds.

3. **Reduced JavaScript Budget Pressure:**
- Since rendering happens server-side, less client-side JS is needed.

**❌ Cons:** 
SSR works great for static content due to the above advantages. However, it does have a few disadvantages because of which it is not perfect for all scenarios.

1. **Slower Time To First Byte (TTFB)**
- Since all processing takes place on the server, the response from the server may be delayed in case of one or more of the following scenarios:
  - Multiple simultaneous users causing excess load on the server.
  - Slow network.
  - Server code not optimized.

2. **Full page reloads required for some interactions:**
- Since all code is not available on the client, frequent round trips to the server are required for all key operations causing full page reloads. 
- This could increase the time between interactions as users are required to wait longer between operations.
- A single-page application is thus not possible with SSR.

To address these drawbacks, **modern frameworks** and **libraries** allow **rendering** on both **server** and **client** for the same application. See below for more details. 

**🧮 Working with Frameworks:**
1. SSR with Next.js
- The **Next.js framework** also supports **SSR**. This pre-renders a page on the server on every request. It can be accomplished by exporting an **async function** called **getServerSideProps()** from a page as follows.

```javascript
export async function getServerSideProps(context) {
  return {
    props: {}, // will be passed to the page component as props
  };
}
```
- The **context object** contains keys for HTTP request and response objects, routing parameters, querystring, locale, etc.

The following implementation shows the use of getServerSideProps() for rendering data on a page formatted using React.

```javascript
// data fetched from an external data source using `getServerSideProps`

const Users = ({ users, error }) => {
 return (
   <section>
     <header>
       <h1>List of users</h1>
     </header>
     {error && <div>There was an error.</div>}
     {!error && users && (
       <table>
         <thead>
           <tr>
             <th>Username</th>
             <th>Email</th>
             <th>Name</th>
           </tr>
         </thead>
         <tbody>
           {users.map((user, key) => (
             <tr key={key}>
               <td>{user.username}</td>
               <td>{user.email}</td>
               <td>{user.name}</td>
             </tr>
           ))}
         </tbody>
       </table>
     )}
   </section>
 );
};

export async function getServerSideProps() {
 // Fetch data from external API
 const res = await fetch("https://jsonplaceholder.typicode.com/users")
 const data = await res.json();

 // Pass data to the page via props
 return { props: { data } }
}

export default Users;
```

2. React for the Server
- React can be rendered **isomorphically**, which means that it can function both on the **browser** as well as other platforms like the **server**. Thus, UI elements may be rendered on the server using React.
- React can also be used with **universal code** which will allow the same code to run in **multiple environments**. This is made possible by using **Node.js** on the server or what is known as a Node server. Thus, universal JavaScript may be used to fetch data on the server and then render it using isomorphic React.

React functions that make this possible include:
```javascript
ReactDOMServer.renderToString(element);
```

This function returns an HTML string corresponding to the React element. The HTML can then be rendered to the client for a faster page load.
- The **renderToString()** function may be used with **ReactDOM.hydrate()**. This will ensure that the rendered HTML is preserved as-is on the client and only the event handlers attached after load.
- To implement this, we use a .js file on both client and server corresponding to every page. The .js file on the server will render the HTML content, and the .js file on the client will hydrate it.

**Example:**
- Assume you have a React element called **App** which contains the HTML to be rendered defined in the universal **app.js** file. Both the server and client-side React can recognize the **App** element. 
- The **ipage.js** file on the server can have the code:

```javascript
app.get("/", (req, res) => {
  const app = ReactDOMServer.renderToString(<App />);
});
```

The constant **App** can now be used to generate the HTML to be rendered. The **ipage.js** on the client side will have the following to ensure that the element **App** is hydrated.

```javascript
ReactDOM.hydrate(<App />, document.getElementById("root"));
```

A complete example of SSR with React can be found [here](https://www.digitalocean.com/community/tutorials/react-server-side-rendering).

**🎯 Final Takeaways (Day 2)**
- SSR ensures **faster initial load** and **SEO-friendliness**.
- Best for **content-heavy sites**(blogs, e-commerce, news portals).
- Less ideal for **highly interactive apps** unless combined with **CSR** or **hydration**.
- Modern frameworks (Next.js, Nuxt, Angular Universal) often **blend SSR** + **CSR** to get the best of both worlds.

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