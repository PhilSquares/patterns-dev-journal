# Day 4 – Incremental Static Generation

## 📄 Summary
- **Incremental Static Generation (iSSG)** enhances **Static Site Generation (SSG)** by enabling pages to be **added or updated after the site has been built** without requiring a full rebuild.

## 💡 Problem It Solves
- SSG works great for static data, but fails when content changes frequently. 
- iSSG solves this by **incrementally updating or creating pages in the background**, keeping the site fresh and scalable.

## 🌎 Real-world Mapping
- A **news site 📰** with hundreds of daily articles can’t rebuild the entire site every few minutes.  
- Instead, iSSG regenerates **only the changed or requested pages**, keeping the experience fast and up-to-date.

## 🛠 My Mini Implementation
```javascript
// Example: iSSG with revalidation in Next.js
export async function getStaticProps() {
  return {
    props: {
      products: await getProductsFromDatabase(),
    },
    revalidate: 60, // ♻️ rebuild page every 60s in background
  };
}

export default function Products({ products }) {
  return (
    <>
      <h1>Products</h1>
      <ul>
        {products.map((p) => (
          <li key={p.id}>{p.name}</li>
        ))}
      </ul>
    </>
  );
}
```
