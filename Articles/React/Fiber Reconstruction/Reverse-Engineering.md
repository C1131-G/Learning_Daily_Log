# React Fiber Reconstruction: Reverse Engineering

## 🎯 Core Concepts

### 🧩 The "Fiber" Advantage
React Fiber is the stack-reconciler's successor. Because it represents the component tree as a linked list of "fiber" objects, it contains a wealth of metadata about your application's structure, state, and props—even in minified production builds.

### 🧪 The Reconstruction Experiment
The goal was to "steal" or reconstruct a React component from a live production site by traversing the Fiber tree and using Large Language Models (LLMs) to make sense of the extracted data.

---

## 📝 Implementation Workflow

### 🚀 Step 1: Accessing the Fiber Tree
In the browser console, you can often find the root fiber by looking for internal React properties on DOM nodes (e.g., `__reactFiber$...`).

### 🛠️ Step 2: Traversal & Extraction
By recursively following `child`, `sibling`, and `return` pointers, you can map the entire component hierarchy and extract the `memoizedProps` for each component.

### 🤖 Step 3: LLM Reconstruction
Pass the extracted (and often minified) props and component structure to an LLM. With enough context, the LLM can rewrite the component in clean, readable JSX that matches the original functionality.

---

## 💡 Key Takeaways

1. **Fiber is a Map** - If a component is rendered in React, its blueprint exists in the Fiber tree.
2. **Minification isn't Security** - Obfuscated code still leaves a structural trail that can be reverse-engineered.
3. **LLMs as De-obfuscators** - AI is exceptionally good at turning minified property names back into meaningful business logic based on usage patterns.
4. **Defense in Depth** - To prevent this, avoid putting sensitive logic (like auth checks or secret keys) entirely in the client-side React tree.
