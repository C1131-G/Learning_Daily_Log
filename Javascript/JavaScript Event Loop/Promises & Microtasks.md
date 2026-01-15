<!-- Question -->

## Explain what will be printed to the console, in what order, and why this order happens.

console.log("A");

Promise.resolve().then(() => {
  console.log("B");
});

console.log("C");

<!-- Answer -->

Correct output order: A → C → B

JavaScript runs synchronous code first, so A is logged.
Promise.resolve().then(...) schedules its callback in the microtask queue, but it does not run immediately.
The next synchronous line runs, so C is logged.
After the call stack is empty, the event loop processes the microtask queue, so B is logged last.

<!-- Follow up Question -->
## Why do Promises use the microtask queue instead of the macrotask (callback) queue?

<!-- Answer -->
Promises use the microtask queue so that their callbacks run immediately after the current synchronous code finishes, before any macrotasks like setTimeout.
This guarantees predictable execution order and allows promise chains to run without being delayed by rendering or timers.