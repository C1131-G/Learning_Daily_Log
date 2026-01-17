# React Server Components: Architecture & Data Fetching

## 🎯 Core Concepts

### 🏗️ The Multi-World Architecture
React Server Components (RSC) allow you to split your application between the server and the client seamlessly:
- **Server Components**: Handle data fetching, DB access, and rendering heavy libraries (syntax highlighting, math) with **Zero Client JS**.
- **Client Components**: Handle interactivity, state, and browser APIs.

### 🧱 SSG and RSC
RSC isn't just for dynamic apps. It’s an incredibly powerful **Static Site Generator (SSG)** tool, allowing you to use JSX for templating while sending lean HTML to the user.

---

## 📝 Data Fetching Patterns

### 1. Server-Owned Data (Direct Fetch)
Fetch directly inside an `async` Server Component.
- **Optimization**: Use `React.cache` or framework-level deduplication (like in Next.js) to avoid duplicate network calls in one render pass.

### 2. Streaming Promises (The `use` Hook)
Instead of `awaiting` everything on the server, pass a promise to a Client Component.
- **Benefit**: Starts the fetch immediately on the server but doesn't block the initial HTML stream. The client waits for the promise using the `use()` hook.

### ⚠️ Pitfall: Server Actions for Fetching
While you *can* use Server Actions (POST requests) in a `useEffect` to fetch data, it is generally an **anti-pattern**:
- **No HTTP Caching**: POST requests aren't cached by browsers/CDNs.
- **No Memoization**: Unlike `fetch`, actions aren't automatically deduplicated.
- **Recommendation**: Use RSC for initial data and standard `GET` requests (via TanStack Query) for client-side reads.

---

## 💡 Key Takeaways

1. **Move Purity to the Server** - Render non-interactive, library-heavy components on the server to save user bandwidth.
2. **Avoid Waterfalls** - Initiate fetches at the page level rather than deep inside nested components.
3. **Serialization is Required** - All data passed from Server to Client must be JSON-serializable (no methods or class instances).
4. **Use Actions for Mutations Only** - Keep Server Actions strictly for writing data and revalidating paths.
