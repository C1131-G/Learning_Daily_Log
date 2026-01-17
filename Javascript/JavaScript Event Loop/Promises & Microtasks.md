# JavaScript Event Loop: Promises & Microtasks

## 🤔 Question

**Explain what will be printed to the console, in what order, and why this order happens.**

```javascript
console.log("A");

Promise.resolve().then(() => {
  console.log("B");
});

console.log("C");
```

---

## ✅ Answer

**Correct output order:** `A → C → B`

---

## 📖 Explanation

1. **JavaScript runs synchronous code first**, so `A` is logged.

2. **`Promise.resolve().then(...)`** schedules its callback in the **microtask queue**, but it does **not** run immediately.

3. **The next synchronous line runs**, so `C` is logged.

4. **After the call stack is empty**, the event loop processes the **microtask queue**, so `B` is logged last.

---

## 🔍 Follow-up Question

**Why do Promises use the microtask queue instead of the macrotask (callback) queue?**

### Answer

Promises use the **microtask queue** so that their callbacks run **immediately after** the current synchronous code finishes, **before** any macrotasks like `setTimeout`.

This guarantees:
- ✅ **Predictable execution order**
- ✅ **Promise chains run without delay** from rendering or timers
- ✅ **Higher priority** than setTimeout/setInterval callbacks

---

## 💡 Key Takeaways

| Queue Type | Examples | Priority | When It Runs |
|------------|----------|----------|--------------|
| **Microtask** | Promises, `queueMicrotask()` | High | Immediately after current script |
| **Macrotask** | `setTimeout`, `setInterval`, I/O | Low | After microtasks are cleared |

**Remember:** Microtasks always run before the next macrotask!