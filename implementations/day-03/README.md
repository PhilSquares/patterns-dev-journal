# Day 3 – Static Rendering (SSG)

## 📄 Summary
Static Rendering (also called **Static Site Generation – SSG**) is a rendering pattern where pages are pre-rendered into HTML files at **build time**. This means the content is ready to be served instantly from a CDN or web server, leading to **fast load times** and excellent **SEO**.

---

## 💡 Problem It Solves
- **SSR (Server Side Rendering):** Higher server processing time → Slower **TTFB** (Time to First Byte).  
- **CSR (Client Side Rendering):** Large JS bundles → Slower **FCP/LCP/TTI** (First/Last Contentful Paint, Time to Interactive).  
- **SSG:** Delivers pre-rendered pages ahead of time → ✅ Fast, SEO-friendly, and lightweight.  

---

## 🌎 Real-world Mapping
Imagine a **product catalog site**:
- **Listing page:** `/products` generated at build time with a list of items.  
- **Details page:** `/products/[id]` generated for each product.  

These pages are built once and served instantly to users.  
🔑 In **Next.js**, this is achieved with:  
- `getStaticProps()` → Fetches data at build time.  
- `getStaticPaths()` → Generates routes for each item.  

---

## 🛠 My Mini Implementation
### 1. Listing Page
```javascript
// pages/products.js
export async function getStaticProps() {
  return {
    props: {
      products: await getProductsFromDatabase(),
    },
  };
}

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