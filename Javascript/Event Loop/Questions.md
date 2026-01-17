# JavaScript Event Loop: Practice Questions

## 🎯 Core Concepts

### 🧩 Execution Priority
JavaScript is single-threaded and executes code synchronously. However, asynchronous operations like Promises are handled via the **Event Loop**, which uses specific queues to manage execution order.

### ⚡ Microtasks vs. Macrotasks
- **Microtask Queue**: High-priority queue (Promises, `queueMicrotask`). These run immediately after the current synchronous script finishes.
- **Macrotask Queue**: Low-priority queue (`setTimeout`, `setInterval`, I/O). These run only after the microtask queue is completely empty.

---

## 📝 Practical Example

### 🤔 The Question
**Explain what will be printed to the console, in what order, and why this order happens.**

```javascript
console.log("A");

Promise.resolve().then(() => {
  console.log("B");
});

console.log("C");
```

### ✅ The Answer
**Correct output order:** `A → C → B`

### 📖 Step-by-Step Breakdown
1. **Sync Execution**: `console.log("A")` runs immediately.
2. **Scheduling**: `Promise.resolve().then(...)` schedules its callback in the **microtask queue**.
3. **Sync Execution**: `console.log("C")` runs immediately.
4. **Queue Drain**: The call stack is now empty. The event loop checks the **microtask queue** and executes the callback: `console.log("B")`.

---

## 💡 Key Takeaways

1. **Synchronous First** - Everything on the main stack completes before React or the Event Loop looks at async queues.
2. **Microtasks > Macrotasks** - If you schedule a `Promise` and a `setTimeout(0)`, the Promise will **always** run first.
3. **Queue Draining** - The event loop will clear the *entire* microtask queue before moving to the next macrotask, which can lead to "blocking" if you create an infinite loop of microtasks.
4. **Predictability** - Promises use microtasks to ensure their chains run as fast as possible without waiting for the next browser paint or timer.
