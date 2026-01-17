# Structured Logging Best Practices

> Source: [loggingsucks.com](https://loggingsucks.com/)

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

## 📝 Examples

### ❌ Bad Example (Standard console.log)

**Problem:** Hard to read and impossible to filter automatically.

```javascript
// BAD - Don't do this
try {
  await processPayment(user);
} catch (err) {
  // ❌ Vague string, missing context, separate lines make it hard to group
  console.log("Payment started"); 
  console.log("Error happened: " + err); 
  console.log("User was " + user.email);
}
```

**Issues:**
- Multiple log statements for one operation
- String concatenation makes filtering impossible
- Missing critical context (error codes, performance metrics)
- No structured data

---

### ✅ Good Example (Structured Object)

**Solution:** Single "Wide Event" that contains the full story.

```javascript
// GOOD - Do this instead
try {
  const start = Date.now();
  await processPayment(user);
} catch (err) {
  // ✅ Single JSON object with all context
  logger.error({
    event: "payment_failed",              // What happened
    user_id: user.id,                     // Who it happened to
    plan_tier: user.subscription,         // Business context
    error_code: err.code || "UNKNOWN",    // Specific error for filtering
    error_message: err.message,           // Human readable details
    duration_ms: Date.now() - start,      // Performance metric
    path: "/api/checkout"                 // Location
  });
}
```

**Benefits:**
- ✅ Single log entry with complete context
- ✅ Filterable by any field (`user_id`, `error_code`, etc.)
- ✅ Includes performance metrics
- ✅ Standardized key names
- ✅ Machine-readable and human-readable

---

## 🔧 Recommended Tools

- **[OpenTelemetry JS SDK](https://opentelemetry.io/docs/languages/js/)** - Industry standard for observability
- **[Pino](https://getpino.io/)** - Fast, low-overhead JSON logger for Node.js

---

## 💡 Key Takeaways

1. **Log objects, not strings** - Enable filtering and searching
2. **One operation = One log entry** - Centralize all context
3. **Standardize field names** - Consistency across your codebase
4. **Include context** - User, error, performance, location
5. **Use proper tools** - OpenTelemetry and Pino for production
