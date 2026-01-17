# ([logging sucks](https://loggingsucks.com/))

## Learning

**Stop Using Strings**: 
Never log simple sentences like "Payment failed." Computers cannot easily search or filter sentences.

**Start Using JSON**: 
Always log objects (key-value pairs). This allows tools to filter by specific fields like user_id or error_code.

**Centralize Context**: 
Don't log five times in one function. Log once at the end of the operation with all the details (input, output, duration, and error).

**Standardize Keys**: 
Decide on naming conventions early (e.g., always use user_id, never mix it with userId or uid) to ensure searches work across the whole system.

## Examples

**The Bad Example (Standard console.log)**

**This is hard to read and impossible to filter automatically.**

`// BAD

try {

  await processPayment(user);

} catch (err) {

  // ❌ Vague string, missing context, separate lines make it hard to group

  console.log("Payment started"); 

  console.log("Error happened: " + err); 

  console.log("User was " + user.email);

}`

**The Good Example (Structured Object)**

**This creates a single "Wide Event" that contains the full story.**

`// GOOD

try {

  const start = Date.now();
  
  await processPayment(user);

} catch (err) {

  // ✅ Single JSON object with all context (Business, User, Error, Performance)

  logger.error({

    event: 
    
    "payment_failed",            /
    / What happened
    
    user_id: user.id,                   // Who it happened to
    
    plan_tier: user.subscription,       // 
    
    Business context
    error_code: err.code || "UNKNOWN",  // Specific error for filtering
    
    error_message: err.message,         // Human readable details
    
    duration_ms: Date.now() - start,    // Performance metric
    
    path: "/api/checkout"               //Location
  });

}
`
**Check OpenTelemetry JS SDK and Pino.**
