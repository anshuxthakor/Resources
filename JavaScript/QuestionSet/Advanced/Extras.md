# JavaScript — The Extras (Final Leftovers)

---

## Section 1 — WeakMap & WeakSet

### Q1. Private Instance Data with WeakMap

Use a `WeakMap` to store truly private balance data for a `BankAccount` class — inaccessible from outside the class.

```js
const _private = new WeakMap();

class BankAccount {
  constructor(owner, balance) {
    // store balance privately using WeakMap
  }
  deposit(amount) {}
  withdraw(amount) {}
  getBalance() {}
}

const acc = new BankAccount("Anubhav", 5000);
acc.deposit(1000);
console.log(acc.getBalance()); // 6000
console.log(acc.balance);      // undefined — truly private
```

---

### Q2. WeakMap vs Map — Memory Difference

Explain what happens in memory for each case. Which one risks a memory leak, and why? When should you prefer `WeakMap`?

```js
// Case A — Map
let user = { name: "Rahul" };
const map = new Map();
map.set(user, "some data");
user = null;
// Is the object garbage collected?

// Case B — WeakMap
let user2 = { name: "Aman" };
const weakmap = new WeakMap();
weakmap.set(user2, "some data");
user2 = null;
// Is the object garbage collected?
```

---

### Q3. Visit Tracker with WeakSet

Use a `WeakSet` to track which objects (e.g. DOM nodes) have already been processed, without preventing garbage collection.

```js
const visited = new WeakSet();

function process(node) {
  if (visited.has(node)) {
    console.log("Already processed, skipping.");
    return;
  }
  visited.add(node);
  console.log("Processing:", node.id);
}

const node1 = { id: "header" };
process(node1); // "Processing: header"
process(node1); // "Already processed, skipping."
```

Why would a regular `Set` be a problem here in a long-running app?

---

## Section 2 — Error Handling Architecture

### Q4. Custom Error Class Hierarchy

Build:

```
AppError (base)
  ├── NetworkError (adds: statusCode)
  └── ValidationError (adds: field)
```

Each must have a correct `name`, work with `instanceof`, and show a useful stack trace.

```js
throw new NetworkError("API unreachable", 503);
// NetworkError: API unreachable (status: 503)

throw new ValidationError("Invalid input", "email");
// ValidationError: Invalid input (field: email)

err instanceof AppError      // true
err instanceof NetworkError  // true
```

---

### Q5. Result Type Pattern (Rust-style Ok/Err)

Instead of throwing, implement a `Result` pattern to handle errors as values.

```js
function divide(a, b) {
  if (b === 0) return Result.err("Division by zero");
  return Result.ok(a / b);
}

const result = divide(10, 2);
if (result.ok) console.log(result.value); // 5

const fail = divide(10, 0);
if (!fail.ok) console.log(fail.error); // "Division by zero"
```

---

### Q6. SafeBox — A Maybe/Optional Monad

Implement `Box(value).map(fn)` for chaining transformations. Then implement `SafeBox` that gracefully skips all `.map()` calls if the value is nullish, instead of throwing.

```js
const result = Box(3)
  .map(x => x * 2)
  .map(x => x + 1)
  .value();
console.log(result); // 7

SafeBox(null)
  .map(x => x.name)          // skipped — no error
  .map(x => x.toUpperCase()) // skipped
  .value(); // null
```

---

### Q7. Global Error Boundary

Write code that catches all unhandled promise rejections and all uncaught synchronous errors globally in a browser environment, logging each with a timestamp. Name the two events you'd listen to.

```js
// Your global handlers here
```

---

## Section 3 — Regex, Strings & Parsing

### Q8. Tagged Template — Auto-Highlighter

Write a tagged template `highlight` that wraps every interpolated value in `<mark>` tags.

```js
const name = "Anubhav";
const score = 98;

const result = highlight`Student ${name} scored ${score} marks.`;
console.log(result);
// "Student <mark>Anubhav</mark> scored <mark>98</mark> marks."
```

---

### Q9. Tagged Template — XSS-Safe HTML

Write `sanitize(str)` to escape HTML special characters. Then write a tagged template `safeHTML` that auto-sanitizes all interpolated values.

```js
const userInput = '<script>alert("hacked")</script><b>Hello</b>';
console.log(sanitize(userInput));
// &lt;script&gt;alert(&quot;hacked&quot;)&lt;/script&gt;&lt;b&gt;Hello&lt;/b&gt;

const name = '<img src=x onerror=alert(1)>';
const html = safeHTML`<p>Welcome, ${name}!</p>`;
// "<p>Welcome, &lt;img src=x onerror=alert(1)&gt;!</p>"
```

---

### Q10. Regex Log Extractor

Extract all IP addresses and timestamps from this log string using regex.

```js
const log = `
  [2024-01-15 09:23:11] Request from 192.168.1.101
  [2024-01-15 09:24:05] Request from 10.0.0.55
  [2024-01-15 09:25:44] Request from 172.16.254.1
`;

// timestamps: ["2024-01-15 09:23:11", "2024-01-15 09:24:05", "2024-01-15 09:25:44"]
// ips:        ["192.168.1.101", "10.0.0.55", "172.16.254.1"]
```

---

### Q11. Math Expression Tokenizer

Write `tokenize(expr)` that splits a math expression into tokens (numbers, operators, parentheses).

```js
tokenize("3 + (42 * 7) - 15 / 3");
// ["3", "+", "(", "42", "*", "7", ")", "-", "15", "/", "3"]

tokenize("(10+2)*5");
// ["(", "10", "+", "2", ")", "*", "5"]
```

---

## Section 4 — Browser APIs & Environment

### Q12. AbortController — Cancellable Fetch with Timeout

Write `fetchWithTimeout(url, ms)` that auto-cancels the fetch if it exceeds `ms` milliseconds, returns the data on success, and throws a descriptive `TimeoutError` on timeout. Use `AbortController`.

```js
try {
  const data = await fetchWithTimeout("/api/slow", 2000);
  console.log(data);
} catch (e) {
  console.log(e.message); // "Request timed out after 2000ms"
}
```

---

### Q13. IntersectionObserver — Lazy Load Images

Write `lazyLoadImages()` that finds all `<img data-src="...">` tags and loads the real `src` only when the image enters the viewport, then stops observing it. Explain why this beats loading everything upfront or using scroll listeners.

```js
// <img data-src="photo.jpg" alt="Photo" />
lazyLoadImages(); // call once on page load
```

---

### Q14. Event Delegation

Instead of attaching 1000 listeners to a list, use one listener on the parent to handle all clicks.

```html
<ul id="menu">
  <li data-action="home">Home</li>
  <li data-action="about">About</li>
  <li data-action="contact">Contact</li>
</ul>
```

```js
// Write ONE event listener that handles clicks on any <li>
// and logs: "Navigating to: home" (or about, or contact)
```

Explain why this is more performant and how it automatically handles dynamically added items.

---

### Q15. Custom localStorage Wrapper with TTL

Build a `storage` utility that serializes/deserializes values automatically and supports a TTL in milliseconds — expired entries return `null`.

```js
storage.set("session", { userId: 42 }, 5000); // expires in 5s
storage.get("session"); // { userId: 42 }

// after 5 seconds:
storage.get("session"); // null — expired, auto-removed
```

Methods: `get(key)`, `set(key, value, ttl?)`, `remove(key)`, `clear()`.

---

## Section 5 — State Machines & Patterns

### Q16. Finite State Machine — Traffic Light

Implement a traffic light FSM with valid transitions only (`green → yellow → red → green`). Invalid transitions should throw.

```js
const light = createFSM({
  initial: "green",
  transitions: {
    green:  { next: "yellow" },
    yellow: { next: "red"    },
    red:    { next: "green"  }
  }
});

light.state();    // "green"
light.next();
light.state();    // "yellow"

light.go("purple"); // Error: "purple" is not a valid state
```

---

### Q17. Command Pattern — Undo/Redo Text Editor

Implement a text editor buffer with undo/redo using the Command pattern.

```js
const editor = createEditor();

editor.type("Hello");
editor.type(" World");
editor.getText();   // "Hello World"

editor.undo();
editor.getText();   // "Hello"

editor.undo();
editor.getText();   // ""

editor.redo();
editor.getText();   // "Hello"
```

---

## Section 6 — Functional Internals

### Q18. Transducer — Single-Pass Filter + Map

`.filter().map()` creates an intermediate array. Write a transducer that composes filter and map into one pass, no intermediate arrays.

```js
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Normal way — creates 2 arrays:
const result = numbers.filter(x => x % 2 === 0).map(x => x * x);

// Transducer way — single pass:
const xform = compose(
  filter(x => x % 2 === 0),
  map(x => x * x)
);
const result2 = transduce(xform, numbers);

console.log(result2); // [4, 16, 36, 64, 100]
```

---

## Section 7 — Object Internals

### Q19. Freeze vs Seal

```js
const config = {
  apiUrl: "https://api.example.com",
  timeout: 3000,
  retries: 5
};
```

Freeze the object and try modifying `timeout`. Then do the same with `Object.seal()`. What's the difference?

```
Object.freeze → no changes allowed (even existing keys)
Object.seal   → existing keys can be updated, but no add/delete
```

---

### Q20. Property Descriptors

Use `Object.defineProperty()` to create an `id` property on a user object that is non-writable, non-enumerable, and non-configurable. Verify by trying to change, delete, and loop over it.

```js
const user = { name: "Anubhav" };
// add id: 101 with above constraints
```

---

### Q21. Flatten & Unflatten an Object

Write `flattenObject(obj)` converting nested objects into single-level dot-notation keys, and `unflattenObject(flat)` reversing it.

```js
const nested = {
  user: { name: "Anubhav", address: { city: "Bhopal", pincode: 462001 } },
  score: 95
};

flattenObject(nested);
// {
//   "user.name": "Anubhav",
//   "user.address.city": "Bhopal",
//   "user.address.pincode": 462001,
//   "score": 95
// }

unflattenObject(flattenObject(nested)); // back to original shape
```

---

### Q22. JSON Path Evaluator

Write `getByPath(obj, path)` to extract a deeply nested value using a string path, including array indices.

```js
const data = {
  company: {
    employees: [
      { name: "Anubhav", role: "dev" },
      { name: "Rahul",   role: "design" }
    ]
  }
};

getByPath(data, "company.employees.0.name"); // "Anubhav"
getByPath(data, "company.employees.1.role"); // "design"
getByPath(data, "company.ceo.name");         // undefined
```

---

## Section 8 — Proxy Deep Cuts

### Q23. Negative Array Indexing via Proxy

Use `Proxy` to allow Python-style negative indexing on arrays.

```js
const arr = proxiedArray([10, 20, 30, 40, 50]);

console.log(arr[-1]); // 50
console.log(arr[-2]); // 40
console.log(arr[0]);  // 10
```

---

### Q24. Auto-Vivifying Object

Use `Proxy` so that accessing any nested path — however deep — never throws, and missing paths auto-create themselves.

```js
const data = autoVivify();

data.user.address.city = "Bhopal";
console.log(data.user.address.city); // "Bhopal"
console.log(data.a.b.c.d.e);         // {} — no error
```

---

## Section 9 — Concurrency Bugs

### Q25. Race Condition — Find & Fix

This code has a race condition. Identify it and fix it using a mutex lock pattern.

```js
let balance = 1000;

async function withdraw(amount) {
  const current = balance;     // read
  await delay(100);            // simulate async DB call
  balance = current - amount;  // write
}

await Promise.all([
  withdraw(200),
  withdraw(300),
]);

console.log(balance);
// Expected: 500
// Actual: ???  — why?
```

---

## Boss — UI & Rendering Internals

### B1. Virtual DOM Differ

Write `diff(oldTree, newTree)` comparing two virtual DOM trees (plain objects) and returning a list of changes.

```js
const oldTree = { tag: "div", props: { id: "app" }, children: ["Hello"] };
const newTree = { tag: "div", props: { id: "app" }, children: ["Hello", "World"] };

diff(oldTree, newTree);
// [{ type: "CHILDREN_CHANGED", added: ["World"] }]
```

Extend it to also detect `PROPS_CHANGED` and `TAG_CHANGED`.

---