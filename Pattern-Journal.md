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

## Day 3 – Static Rendering (SSG)  
**📅 Date:** 2025-09-03  
**📂 Category:** Rendering  

---

### 📖 Pattern Summary  
**Static Rendering** (also called **Static Site Generation – SSG**) pre-builds HTML at **build time**, so users get fully rendered content right away. Unlike CSR or SSR:  

- ⚡️ **CSR** delays first paint while waiting for JavaScript to run.  
- 🖥️ **SSR** makes the server render every request, which increases **TTFB** (Time to First Byte).  
- 📦 **SSG** strikes a balance: pages are generated once at build time and served instantly on request.  

---

### 💡 Problem It Solves  
- ⏳ Avoids **slow TTFB** of **SSR**.  
- 🚫 Avoids **heavy JavaScript bundles** of **CSR**.  
- ✅ Offers **fast FCP/LCP/TTI** by delivering ready-to-use HTML.  

SSG is especially powerful for:  
- Static content like **About** / **Contact** pages.  
- Content-heavy apps like **blogs** and **product catalogs**.  

---

### 🛠 Example (Next.js without data)  
```javascript
// pages/about.js
export default function About() {
  return (
    <div>
      <h1>About Us</h1>
      {/* ... */}
    </div>
  );
}
```

📌 When you run **next build**, this page becomes a static **about.html** served instantly at **/about**.

**📊 SSG with Data**
- For dynamic content (blogs, product pages, etc.):
- Use **getStaticProps()** at build time to fetch and pre-render data.
- Each item can also have its own static page with **getStaticPaths().**

**🌎 Real-world analogy:**
- Think of SSG like printing a book 📖:
  - **🖨️ SSG (pre-printed book):** Content is ready before anyone buys it. Super fast to deliver, but if you need changes, you must reprint the book.
  - **🏭 SSR (print-on-demand):** Each book is printed as the customer orders — flexible but slower.
  - **✍️ CSR (blank notebook):** Customers get blank pages and instructions; they must “build” the book themselves in the browser.

- Code examples:
  - **Example 1: Product Listing Example - All Items**
    - Generation of a listing page is a scenario where the content to be displayed on the page depends on external data.
    - This data will be fetched from the database at build time to construct the page. 
    - In **Next.js** this can be achieved by exporting the function **getStaticProps()** in the page component.
    - The function is called at build time on the build server to fetch the data.
    - The data can then be passed to the page’s **props** to pre-render the page component.

```javascript
// This function runs at build time on the build server
export async function getStaticProps() {
  return {
    props: {
      products: await getProductsFromDatabase(),
    },
  };
}

// The page component receives products prop from getStaticProps at build time
export default function Products({ products }) {
  return (
    <>
      <h1>Products</h1>
      <ul>
        {products.map((product) => (
          <li key={product.id}>{product.name}</li>
        ))}
      </ul>
    </>
  );
}
```

- Note: The function will not be included in the client-side JS bundle and hence can even be used to fetch the data directly from a database.

  - **Example 2: Individual Product Details Page - Per Item**
    - In the previous example above, we could have an individual detailed page for each of the products listed on the listing page.
    - These pages could be accessed by clicking on the corresponding items on the listing page or directly through some other route.
      - For instance, assume we have products with **product ids** **101**, **102**, **103**, and so on. 
      - We need their information to be available at routes **/products/101,** **/products/102,** **/products/103** etc. 
      - To achieve this at build time in Next.js we can use the function **getStaticPaths()** in combination with [dynamic routes](https://nextjs.org/docs/routing/dynamic-routes).

- For starters, we need to create a **common page component** **products/[id].js** for this and export the function **getStaticPaths()** in it.
  - The function will return all possible product ids which can be used to pre-render individual product pages at build time.
  - The following Next.js skeleton available [here](https://vercel.com/blog/nextjs-server-side-rendering-vs-static-generation#individual-product-page-static-generation-with-data) shows how to structure the code for this.

```javascript
// pages/products/[id].js

// In getStaticPaths(), you need to return the list of
// ids of product pages (/products/[id]) that you'd
// like to pre-render at build time. To do so,
// you can fetch all products from a database.
export async function getStaticPaths() {
  const products = await getProductsFromDatabase();

  const paths = products.map((product) => ({
    params: { id: product.id },
  }));

  // fallback: false means pages that don't have the correct id will 404.
  return { paths, fallback: false };
}

// params will contain the id for each generated page.
export async function getStaticProps({ params }) {
  return {
    props: {
      product: await getProductFromDatabase(params.id),
    },
  };
}

export default function Product({ product }) {
  // Render product
}
```

**✅ Pros & Cons ❌:**

**✅ Pros:**
1. 🚀 Super-fast performance (pre-rendered HTML = instant response).
2. 🔍 SEO-friendly (content visible without JS execution).
3. 🌐 Works great with CDNs (edge caching = global speed).

**❌ Cons:** 
1. 📂 Potentially huge number of HTML files for large datasets.
2. 🔄 Requires rebuild + redeploy whenever content changes.
3. ⚠️ Not suitable for highly dynamic, real-time apps.

🎯 Final Takeaways
- **SSG** is the sweet spot for static-heavy content sites (blogs, portfolios, docs).
- Blazing fast, SEO-friendly, and great for read-heavy apps.
- But beware of stale content if updates happen often.

---

## Day 4 – Incremental Static Generation
**📅 Date:** 2025-09-04  
**📂 Category:** Rendering

### 📖 Pattern Summary 

**💡 Problem It Solves:**  
- **Incremental Static Generation (iSSG)** was introduced as an **upgrade to Static Site Generation (SSG)** to overcome its main limitation: **dynamic, frequently updated data.**  
- With iSSG, static sites can be updated **incrementally** without requiring a full rebuild by regenerating pages in the background as needed.

---

### ⚙️ How It Works  

**1️⃣ Adding New Pages:**  
- Uses lazy-loading for non-existent pages.  
- On first request, the page is generated and served.  
- While building, users may see a **fallback UI** (`Loading...`) instead of a 404.  

```javascript
// Next.js code required for lazy-loading the non-existent page with iSSG.
// pages/products/[id].js

// In getStaticPaths(), you need to return the list of
// ids of product pages (/products/[id]) that you'd
// like to pre-render at build time. To do so,
// you can fetch all products from a database.
export async function getStaticPaths() {
  const products = await getProductsFromDatabase();

  const paths = products.map((product) => ({
     params: { id: product.id }
  }));


  // fallback: true means that the missing pages
  // will not 404, and instead can render a fallback.
  return { paths, fallback: true };
}

// params will contain the id for each generated page.
export async function getStaticProps({ params }) {
  return {
    props: {
      product: await getProductFromDatabase(params.id)
    }
  }
}


export default function Product({ product }) {
  const router = useRouter();

  if (router.isFallback) {
    return <div>Loading...</div>;
  }

  // Render product
}
```

**2️⃣ Updating Existing Pages:**
- Pages are **revalidated** in the background after a set time.
- iSSG uses the [stale-while-revalidate](https://web.dev/stale-while-revalidate/) strategy where the user receives the cached or stale version while the revalidation takes place.
- To make the previous example serve a relatively dynamic list of products, we will include the code to set the timeout for rebuilding the page (see below).

```javascript
// This function runs at build time on the build server
export async function getStaticProps() {
  return {
    props: {
      products: await getProductsFromDatabase(),
      revalidate: 60, // This will force the page to revalidate after 60 seconds
    }
  }
}

// The page component receives products prop from getStaticProps at build time
export default function Products({ products }) {
  return (
    <>
      <h1>Products</h1>
      <ul>
        {products.map((product) => (
          <li key={product.id}>{product.name}</li>
        ))}
      </ul>
    </>
  )
}
```

**🌎 Real-world analogy:**
- Think of a digital menu board at a restaurant 🍔:
  - The board always shows something, even if the menu is being updated behind the scenes.
  - If new dishes are added, they appear on-demand when someone first asks for them.
  - Customers don’t experience downtime and the menu quietly stays up-to-date.

**✅ Pros & Cons ❌:**

**✅ Pros:**
1. **Dynamic Data:**
- Handles dynamic data without full rebuilds.

2. **Speed:**
- Fast, since regeneration happens in the background.

3. **Availability:**
- A fairly recent version of any page will always be available online for users to access. 
- Even if the regeneration fails in the background, the old version remains unaltered.

4. **Consistent:**
- As the regeneration takes place on the server one page at a time, the load on the database and the backend is low and performance is consistent. 
- As a result, there are no spikes in latency.

5. **Ease of Distribution:**
- Just like SSG sites, iSSG sites can also be distributed through a network of CDN’s used to serve pre-rendered web pages.

**❌ Cons:** 
- Initial requests for a new page may show a fallback until it’s generated.
- Content can still be slightly stale, depending on revalidation timing.
- More complex to set up compared to basic SSG.

---

## Day 5 – Streaming Server-Side Rendering
**📅 Date:** 2025-09-05  
**📂 Category:** Rendering  

### 📖 Pattern Summary  

**💡 Problem It Solves:**  
- **Traditional SSR** waits for the entire HTML to be generated before sending it to the client. This can delay the **Time To First Byte (TTFB)** and slow down rendering.  
- **Streaming SSR (with Node streams)** allows servers to send chunks of HTML to the client **as soon as they are ready**.  
- The client can begin parsing and rendering while the server continues to send more chunks.  
- In React, this is enabled through **renderToNodeStream** and **renderToStaticNodeStream**.  

---

### ⚡️ Example (from Patterns.dev)  

**Scenario:** An app showing thousands of cat facts. Instead of waiting for all facts to render, we stream them to the client as they’re generated.
```javascript
// server.js

import React from "react";
import path from "path";
import express from "express";
import { renderToNodeStream } from "react-dom/server";

import App from "./src/App";

const app = express();

// app.get("/favicon.ico", (req, res) => res.end());
app.use("/client.js", (req, res) => res.redirect("/build/client.js"));

const DELAY = 500;
app.use((req, res, next) => {
  setTimeout(() => {
    next();
  }, DELAY);
});

const BEFORE = `
<!DOCTYPE html>
  <html>
    <head>
      <title>Cat Facts</title>
      <link rel="stylesheet" href="/style.css">
      <script type="module" defer src="/build/client.js"></script>
    </head>
    <body>
      <h1>Stream Rendered Cat Facts!</h1>
      <div id="approot">
`.replace(/
s*/g, "");

app.get("/", async (request, response) => {
  try {
    const stream = renderToNodeStream(<App />);
    const start = Date.now();

    stream.on("data", function handleData() {
      console.log("Render Start: ", Date.now() - start);
      stream.off("data", handleData);
      response.useChunkedEncodingByDefault = true;
      response.writeHead(200, {
        "content-type": "text/html",
        "content-transfer-encoding": "chunked",
        "x-content-type-options": "nosniff"
      });
      response.write(BEFORE);
      response.flushHeaders();
    });
    await new Promise((resolve, reject) => {
      stream.on("error", err => {
        stream.unpipe(response);
        reject(err);
      });
      stream.on("end", () => {
        console.log("Render End: ", Date.now() - start);
        response.write("</div></body></html>");
        response.end();
        resolve();
      });
      stream.pipe(
        response,
        { end: false }
      );
    });
  } catch (err) {
    response.writeHead(500, {
      "content-type": "text/pain"
    });
    response.end(String((err && err.stack) || err));
    return;
  }
});

app.use(express.static(path.resolve(__dirname, "src")));
app.use("/build", express.static(path.resolve(__dirname, "build")));

const listener = app.listen(process.env.PORT || 2048, () => {
  console.log("Your app is listening on port " + listener.address().port);
});
```

- Notes:
  - This data contains useful information that our app has to use in order to render the contents correctly, such as the title of the document and a stylesheet.
  - If we were to server render the **App** component using the **renderToString** method, we would have had to wait until the application has received all data before it can start loading and processing this metadata.
  - To speed this up, **renderToNodeStream** makes it possible for the app to start loading and processing this information as it’s still receiving the chunks of data from the App component.

**⚛️ React Streaming APIs:**

1. **renderToNodeStream(element)**
  - Same output as renderToString, but in stream format.
  - Allows hydration on the client (ReactDOM.hydrate()).

2. **renderToStaticNodeStream(element)**
  - Same output as renderToStaticMarkup, but streamed.
  - Used for static, non-interactive content.

- Both can pipe directly into the HTTP response, progressively sending chunks to the client.

**🌎 Real-world analogy:**
- Imagine a restaurant kitchen:
  - With **SSR**, the server waits until the entire meal is prepared before bringing it out.
  - With **Streaming SSR**, the server brings out dishes one by one as soon as they’re ready. The customer can start eating immediately while the rest is still being cooked.

**✅ Pros & Cons ❌:**

**✅ Pros:**
1. **Faster TTFB** – The client gets the first byte sooner and can start parsing immediately.
2. **Handles backpressure** – Streams pause when the network is congested and resume when clear.
3. **Lower memory usage** – The server doesn’t need to buffer the entire HTML before sending.
4. **Supports SEO** – Since HTML is progressively streamed, crawlers can index it.


**❌ Cons:** 
1. Migration is **not always trivial** – code relying on **renderToString** can break.
2. Frameworks that need to inject CSS/JS into **<head>** before rendering may require workarounds.
3. If **renderToStaticMarkup** is combined with dynamic content via **renderToString**, it cannot be easily replaced with streams.

**🔗 Relation to Other Patterns**
- **SSR vs Streaming SSR:** SSR waits until everything is ready, Streaming SSR sends chunks progressively.
- **Streaming + Progressive Hydration:** Together, they allow the browser to display partial HTML quickly and hydrate it in steps, combining speed with interactivity.

---

## Day 6 – Progressive Hydration 
**📅 Date:** 2025-09-06  
**📂 Category:** Rendering  

### 📖 Pattern Summary  
- **Progressive Hydration** is a rendering pattern that improves interactivity on server-rendered pages.  
- Instead of hydrating the entire page at once, parts of the page are progressively hydrated as the browser becomes ready.  
- This reduces the **Time to Interactive (TTI)** and helps large applications feel faster to users.  

**💡 Problem It Solves:**  
- Traditional SSR + Hydration loads all the HTML upfront but delays interactivity because the JavaScript bundle must be fully parsed and executed.  
- For large pages or apps, this creates a “long pause” before users can interact.  
- Progressive Hydration solves this by splitting hydration into chunks: critical UI is hydrated first, while less critical parts hydrate later.  

**Example (Patterns.dev):**  
- Imagine an **e-commerce product page**:  
  - Header, product images, and “Add to Cart” button should hydrate immediately.  
  - Below-the-fold sections like “Reviews” or “Related Products” can hydrate later without blocking interaction with the core content.  

**🌎 Real-world analogy:**  
- Think of setting up a new apartment:  
  - You start by unpacking **essentials** (bed, fridge, table).  
  - Then, as time allows, you unpack **non-essentials** (books, decorations).  
  - The apartment is usable right away, even if not everything is set up.  

**✅ Pros & Cons ❌:**  

**✅ Pros:**  
1. **Faster Interactivity:** Critical UI becomes usable quickly, improving perceived performance.  
2. **Prioritized Hydration:** Lets developers decide which parts of the app matter most to users.  
3. **Better User Experience:** Users can interact with important features immediately, even if the rest of the page hydrates later.  
4. **Resource Efficiency:** Avoids blocking the main thread with a massive hydration task.  

**❌ Cons:**  
1. **Increased Complexity:** Requires careful planning of hydration priorities.  
2. **Framework Support Required:** Not all frameworks support progressive hydration natively (e.g., React doesn’t out-of-the-box; others like Astro/Marko/Qwik provide alternatives).  
3. **Potential UX Inconsistency:** If secondary content takes too long to hydrate, users may try to interact with parts of the page that aren’t interactive yet.  

---

## Day 7 – Selective Hydration
**📅 Date:** 2025-09-07  
**📂 Category:** Rendering  

### 📖 Pattern Summary 
Selective Hydration is a rendering technique introduced in **React 18** that allows parts of a page to be hydrated in a prioritized order instead of all at once.  
- Instead of blocking on slower parts of the UI (e.g., a comments section fetching data), React can hydrate interactive parts first (like navigation or buttons).  
- This makes the page feel responsive faster, even if not all parts are fully interactive yet.  

**💡 Problem It Solves:**  
- Traditional hydration waits for the full app bundle before attaching interactivity, which can delay time-to-interactive.  
- Progressive Hydration streams and hydrates gradually, but doesn’t prioritize *which* components hydrate first.  
- **Selective Hydration** solves this by letting React prioritize important UI first while deprioritizing non-critical sections.  

**Example (Patterns.dev):**  
```javascript
// server.js
import { pipeToNodeWritable } from "react-dom/server";

export function render(res) {
  const data = createServerData();
  const { startWriting, abort } = pipeToNodeWritable(
    <DataProvider data={data}>
      <App assets={assets} />
    </DataProvider>,
    res,
    {
      onReadyToStream() {
        res.setHeader('Content-type', 'text/html');
        res.write('<!DOCTYPE html>');
        startWriting();
      }
    }
  );
}

// App.js
import { Suspense, lazy } from "react";
import Loader from "./Loader";

const Comments = lazy(() => import("./Comments"));

function App() {
  return (
    <main>
      <Header />
      <Suspense fallback={<Loader />}>
        <Comments />
      </Suspense>
      <Footer />
    </main>
  )
}

// index.js
import { hydrateRoot } from "react-dom";
import App from './App';

hydrateRoot(document, <App />);
```

📌 How they work together (big picture flow)  
1. **Server (server.js)** streams down the initial HTML shell quickly.  
2. **App (App.js)** marks some components as critical (hydrate now) and others as deferred (hydrate later using `Suspense` + `lazy`).  
3. **Client (index.js)** progressively hydrates sections of the DOM.  
   - This avoids the “long blank screen” problem from full SSR hydration.  

<details>
<summary>🔧 Under the hood (React 18 APIs)</summary>

- **`pipeToNodeWritable`**  
  Streams HTML from the server to the client without waiting for the full render.  
- **`Suspense` + `lazy`**  
  Define hydration boundaries and defer less-critical components until their code/data is ready.  
- **`hydrateRoot`**  
  Hydrates the streamed HTML on the client, progressively attaching interactivity.  

Together, these APIs allow React to **prioritize hydration**:  
- Critical UI (e.g., navigation) hydrates first.  
- Non-critical UI (e.g., comments, ads) hydrates later, without blocking the main thread.  

</details>

**🌎 Real-world analogy:**
Think of a busy restaurant:
- Customers should be seated, served drinks, and start eating appetizers right away.
- The full meal (like a slow-roasted dish) can come later.
- Selective Hydration works the same way — the essential parts of the UI are ready quickly while heavier components load in the background.

**✅ Pros & Cons ❌:**

**✅ Pros:**
- Faster perceived interactivity since critical UI hydrates first.
- Works well with React’s Suspense for data-fetching boundaries.
- Great for apps with both high-priority (navigation, buttons) and low-priority (comments, ads, long lists) content.
- Reduces main-thread blocking by breaking hydration into smaller, schedulable tasks.

**❌ Cons:** 
- Requires React 18+ and server-side setup for streaming.
- More complex debugging since hydration can complete out of order.
- Some third-party libraries may not yet be compatible with deferred hydration.
- Developers must think carefully about which components are wrapped in Suspense or marked as lower-priority.

🧪 When to reach for it:
- Large pages, async data, or heavy components (feeds, comments, charts).
- You want nav/search/buttons interactive ASAP without waiting for the whole app.

⚠️ Gotchas:
- Ensure code that manipulates document/head (styles/meta) runs before streaming starts.
- Wrap network-bound or heavy components in Suspense and code-split via lazy.

---

## Day 8 – Islands Architecture
**📅 Date:** 2025-09-08  
**📂 Category:** Rendering Architecture  

### 📖 Pattern Summary  
Islands Architecture structures pages as mostly static HTML, with interactive “islands” (small hydrated widgets) embedded where needed.

**💡 Problem It Solves:**  
- Reduces JavaScript payload by shipping only what’s necessary.  
- Avoids full application hydration—only hydrates isolated parts that need interactivity.

**Example (Patterns.dev):**  
- Static page layout (e.g., blog post) with JS-powered widgets like image carousels, filters, or comment sections—each independent island.
```html
// SamplePost.astro

// Component Imports
import { SocialButtons } from '../../components/SocialButtons.js';
---

<html lang="en">
 <head>
   <link rel="stylesheet" href="/blog.css" />
 </head>

 <body>
   <div class="layout">
     <article class="content">
       <section class="intro">
         <h1 class="title">Post title (static)</h1>
         <br/>
         <p>Post sub-title (static)</p>
       </section>
       <section class="intro">
           <p>This is the  post content with images that is rendered by the server.</p>
           <Image src="https://source.unsplash.com/user/c_v_r/200x200" />
           <p>The next section contains the interactive social buttons component which includes its script.</p>
       </section>
       <section class="social">
           <div>
           <SocialButtons client:visible></SocialButtons>
           </div>
       </section>
     </article>
   </div>
 </body>
</html>
```

```javascript
// SocialButtons.js

import { useState } from "preact/hooks";

export function SocialButtons() {
  const [count, setCount] = useState(0);
  const add = () => setCount((i) => i + 1);
  const subtract = () => setCount((i) => i - 1);
  return (
    <>
      <div>{count} people liked this post</div>
      <div align="right">
        <Image src="/like.png" width="32" height="32" onclick={add}></img>
        <Image src="/unlike.png" width="32" height="32" onclick={subtract}></img>
      </div>
    </>
  );
}
```

**🌎 Real-world Analogy:**  
Think of a printed newspaper (static text/images) with detachable widgets inserted (e.g., video embeds or mini-games) that activate only when someone scans them with a device.

**✅ Pros & Cons ❌**  

**✅ Pros:**  
1. Smaller JS payload = faster load & TTI.  
2. SEO-friendly—most content is static HTML.  
3. Performance-friendly—interactive areas hydrate independently.  
4. Accessible—static links and markup remain intact. 

**❌ Cons:**  
- Requires frameworks that support island embedding (e.g., Astro, Marko) or manual setup. 
- Communication between islands can be complex. 
- Limited tooling helps compared to SSR/CSR pipelines.

---

## Day 9 – React Server Components
**Date:** 2025-09-09  
**Category:** Rendering 

### 🌟 What I Learned
React Server Components (RSCs) allow parts of a React app to render on the server, sending **serialized UI trees** (instead of HTML) to the client. This means the browser hydrates less JavaScript, reducing bundle size and improving performance.

The big shift:  
- Traditional SSR/CSR patterns send **HTML + JavaScript**.  
- RSCs send **serialized React tree data**, which React on the client merges with existing components.  

This creates a hybrid rendering model where some components never need to ship to the client at all.

---

### 📌 Key Takeaways
- **Zero-bundle components**: Pure server logic never reaches the browser.  
- **Seamless integration**: RSCs work alongside client components (using the `"use client"` directive).  
- **Improved performance**: Less JavaScript shipped → faster TTI (Time to Interactive).  
- **Still experimental**: Not fully stable outside Next.js 13+ and some frameworks.

---

### 🔧 Under the hood (React 18 APIs) — click to expand
<details>
<summary>React APIs that make RSCs work</summary>

- **`ReactDOM.createServerContext`** – experimental API to support server-driven context.  
- **Flight protocol** – a React-specific serialization format that ships component trees.  
- **Streaming integration** – RSC payloads can be streamed, combining with Suspense.  

</details>

---

### 🧠 Personal Reflection
This feels like the natural progression after selective hydration. I like how React Server Components blur the line between SSR and CSR while avoiding shipping unnecessary client code. The tradeoff is complexity—debugging and framework support is still a moving target.

I see RSCs as most beneficial in **data-heavy apps** (dashboards, SaaS, CMS) where you want to keep logic close to the server and minimize client-side weight.

---

## Day 10 – Rendering Patterns Recap
**📅 Date:** 2025-09-10  
**📂 Category:** Rendering  

### 📖 Summary  
The first 10 days of the challenge focused on **Rendering Patterns** — how applications deliver and hydrate UI.  
Each pattern balances trade-offs between performance, interactivity, developer experience, and infrastructure setup.  

This recap ties together the core lessons learned from **Days 1–9**.  

---

### 📊 Comparison of Rendering Patterns  

| Pattern | Description | Strengths | Weaknesses | Example Use-Cases |
|---------|-------------|-----------|------------|-------------------|
| **CSR** | Rendered fully on client via JS. | Simple, no server infra needed. | Slow first paint, poor SEO. | SPAs, dashboards. |
| **SSR** | Server renders HTML on request. | SEO-friendly, fast first paint. | Higher server load. | Blogs, e-commerce. |
| **SSG** | Pre-build HTML at build-time. | Ultra-fast, cheap hosting. | Inflexible with dynamic data. | Marketing, docs. |
| **ISR** | Static pages revalidated on-demand. | Combines SSG speed + dynamic content. | Stale data between rebuilds. | News, product catalogs. |
| **Streaming SSR** | Server streams HTML progressively. | Faster TTFB, progressive rendering. | More complex infra. | Apps with large UI trees. |
| **Progressive Hydration** | Gradual client hydration. | Faster interactivity vs full hydration. | Priority not controlled. | Medium-sized apps. |
| **Selective Hydration** | React 18 prioritizes hydration. | Critical UI is interactive first. | Debugging is harder. | Apps w/ mixed priority UI. |
| **Islands Architecture** | Independent UI “islands” hydrate separately. | Flexible, great for multi-frameworks. | Extra build complexity. | Jamstack, Astro sites. |
| **React Server Components (RSC)** | Server + client split of React components. | Less JS shipped, powerful streaming. | React-specific, infra setup. | Large React apps, data-heavy UI. |

---

### 🌎 Reflection
- Rendering patterns evolve from **speed vs flexibility trade-offs**.  
- Simpler patterns (**CSR**/**SSR**/**SSG**) are easy to reason about but come with limits.  
- Modern approaches (**ISR**, **RSC**, **Selective Hydration**) aim to balance **performance + developer ergonomics**.  
- The right choice depends on the app’s **scale, content type, and interactivity needs**.  

---

### ✅ Key Takeaways  
- There’s no “best” pattern—just the right fit for a use case.  
- Streaming + hydration strategies are shaping the future of rendering.  
- React Server Components may redefine the balance between server and client work.  
- For my projects, I’d likely use **ISR for content-heavy sites** and **RSC/Selective Hydration for interactive apps**.  

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