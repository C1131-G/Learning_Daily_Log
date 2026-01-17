# Structured Logging Best Practices

## 🎯 Core Principles

### ❌ Stop Using Strings
Never log simple sentences like `"Payment failed."` — Computers cannot easily search or filter sentences.

### ✅ Start Using JSON
Always log objects (key-value pairs). This allows tools to filter by specific fields like `user_id` or `error_code`.

### 📦 Centralize Context
Don't log five times in one function. Log **once** at the end of the operation with all the details (input, output, duration, and error).

### 🏷️ Standardize Keys
Decide on naming conventions early (e.g., always use `user_id`, never mix it with `userId` or `uid`) to ensure searches work across the whole system.

---

## 📝 Case Study: Structured vs. Unstructured

### ❌ The Bad Way (Strings)
```javascript
try {
  await processPayment(user);
} catch (err) {
  console.log("Error happened: " + err); 
}
```

### ✅ The Sharp Way (JSON)
```javascript
try {
  const start = Date.now();
  await processPayment(user);
} catch (err) {
  logger.error({
    event: "payment_failed",
    user_id: user.id,
    error_code: err.code || "UNKNOWN",
    duration_ms: Date.now() - start
  });
}
```

---

## 💡 Key Takeaways

1. **Log objects, not strings** - Enable filtering and searching.
2. **One operation = One log entry** - Centralize all context.
3. **Standardize field names** - Consistency across your codebase.
4. **Include context** - User, error, performance, location.
