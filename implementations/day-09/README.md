# Day 9 – React Server Components (RSCs)

**React Server Components (RSCs)** let you render parts of your UI on the server without sending their JavaScript to the client. The browser receives a **React tree payload**, not just HTML, which gets merged into the running React app.

---

## ✅ Example: Server vs Client Component

```jsx
// app/page.js (Next.js 13+ with RSC enabled)

// ✅ This runs ONLY on the server, no JS shipped to browser
export default async function ServerComponent() {
  const data = await fetch("https://jsonplaceholder.typicode.com/posts/1").then(res => res.json());

  return (
    <div>
      <h1>{data.title}</h1>
      <ClientButton />
    </div>
  );
}

// ✅ Client component
"use client";

function ClientButton() {
  const [count, setCount] = React.useState(0);

  return (
    <button onClick={() => setCount(c => c + 1)}>
      You clicked {count} times
    </button>
  );
}
```

🚀 Why This Matters:
- Server-only code (DB queries, API fetches, heavy computations) never reaches the client.
- Client-side JS is only shipped for interactive parts (use client components).
- Apps become lighter, faster, and more secure.
