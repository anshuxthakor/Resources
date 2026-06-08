# ⚡ JavaScript Advanced Freestyle — Part 2

> **Level:** Advanced
> **Style:** Freestyle — Picks up where Part 1 left off.
> **New Themes:** Iterators, WeakMap, Regex, Error Handling, Modules, Memory, DSA in JS

---

## 🔁 Section 1 — Iterators & Generators (Deep Dive)

### Q1. Custom Iterable Object

Make the `Range` object **iterable** so it works with `for...of` and spread.

```js
const range = {
  from: 1,
  to: 5,
}

// After your implementation:
console.log([...range])         // [1, 2, 3, 4, 5]
for (const n of range) console.log(n) // 1 2 3 4 5
```

**Hint:** Implement `[Symbol.iterator]()` on the object.

---

### Q2. Infinite Generator

Write a generator `fibonacci()` that yields Fibonacci numbers **infinitely**.

Then write a helper `take(gen, n)` that takes the first `n` values from any generator.

```js
const fib = fibonacci()

console.log(take(fib, 8))
// [0, 1, 1, 2, 3, 5, 8, 13]
```

---

### Q3. Generator Pipeline

Write three generators:
- `naturals()` — yields 1, 2, 3, 4, ... infinitely
- `evens(gen)` — filters a generator, yields only even numbers
- `takeN(gen, n)` — takes first `n` values and returns them as an array

Chain them together:

```js
console.log(takeN(evens(naturals()), 5))
// [2, 4, 6, 8, 10]
```

---

### Q4. Async Generator — Paginated Fetch

Write an **async generator** `fetchPages(url)` that fetches paginated data one page at a time. Each page has a `nextUrl` field — stop when it's `null`.

```js
async function* fetchPages(url) {
  // your implementation
}

for await (const page of fetchPages("/api/data?page=1")) {
  console.log(page.items)
}
```

Explain why an async generator is more memory-efficient than `Promise.all` on all pages at once.

---

## 🗄️ Section 2 — WeakMap, WeakSet & Memory

### Q5. Private Data with WeakMap

Use a `WeakMap` to store **truly private** instance data for a class — not accessible from outside.

```js
const _private = new WeakMap()

class BankAccount {
  constructor(owner, balance) {
    // store balance privately using WeakMap
  }

  deposit(amount) { /* ... */ }
  withdraw(amount) { /* ... */ }
  getBalance() { /* returns private balance */ }
}

const acc = new BankAccount("Anubhav", 5000)
acc.deposit(1000)
console.log(acc.getBalance())  // 6000
console.log(acc.balance)       // undefined — truly private
```

---

### Q6. WeakMap vs Map — Memory Difference

Explain what happens in memory for each:

```js
// Case A — Map
let user = { name: "Rahul" }
const map = new Map()
map.set(user, "some data")
user = null
// Is the object garbage collected?

// Case B — WeakMap
let user2 = { name: "Aman" }
const weakmap = new WeakMap()
weakmap.set(user2, "some data")
user2 = null
// Is the object garbage collected?
```

- Which one causes a memory leak? Why?
- When should you prefer `WeakMap` over `Map`?

---

### Q7. Object Visit Tracker

Use a `WeakSet` to track which DOM nodes (or plain objects) have already been **visited/processed**, without preventing them from being garbage collected.

```js
const visited = new WeakSet()

function process(node) {
  if (visited.has(node)) {
    console.log("Already processed, skipping.")
    return
  }
  visited.add(node)
  console.log("Processing:", node.id)
}

const node1 = { id: "header" }
process(node1) // "Processing: header"
process(node1) // "Already processed, skipping."
```

Why would using a regular `Set` here be a problem in a long-running app?

---

## 🚨 Section 3 — Error Handling Patterns

### Q8. Custom Error Classes

Create a hierarchy of custom errors:

```
AppError (base)
  ├── NetworkError  (adds: statusCode)
  └── ValidationError (adds: field)
```

Each should:
- Have a proper `name` property matching the class name
- Work correctly with `instanceof`
- Show a useful stack trace

```js
throw new NetworkError("API unreachable", 503)
// NetworkError: API unreachable (status: 503)

throw new ValidationError("Invalid input", "email")
// ValidationError: Invalid input (field: email)

err instanceof AppError      // true
err instanceof NetworkError  // true
```

---

### Q9. Try/Catch with Async/Await

What is wrong with this code? Fix it.

```js
async function loadData() {
  const res = await fetch("/api/data")
  const json = await res.json()
  return json
}

loadData().then(data => console.log(data))
```

Now handle:
1. Network failure (fetch throws)
2. Non-OK HTTP response (`res.ok === false`)
3. JSON parse failure

Write a robust version with proper error handling for all three.

---

### Q10. Global Error Boundary

In a browser environment, write code that:
- Catches all **unhandled promise rejections** globally
- Catches all **uncaught synchronous errors** globally
- Logs them with a timestamp and prevents the app from silently failing

```js
// Your global handlers here
```

Name the two browser events you'd listen to.

---

### Q11. Result Type Pattern

Instead of throwing, implement a `Result` pattern (like Rust's `Ok/Err`) to handle errors as values.

```js
function divide(a, b) {
  if (b === 0) return Result.err("Division by zero")
  return Result.ok(a / b)
}

const result = divide(10, 2)
if (result.ok) console.log(result.value) // 5

const fail = divide(10, 0)
if (!fail.ok) console.log(fail.error)   // "Division by zero"
```

---

## 🔡 Section 4 — Strings, Regex & Parsing

### Q12. Template Tag Function

Write a custom **tagged template** `highlight` that wraps all interpolated values in `<mark>` tags.

```js
const name = "Anubhav"
const score = 98

const result = highlight`Student ${name} scored ${score} marks.`
console.log(result)
// "Student <mark>Anubhav</mark> scored <mark>98</mark> marks."
```

---

### Q13. Regex — Extract All Data

Given this messy log string, extract all **IP addresses** and **timestamps** using regex.

```js
const log = `
  [2024-01-15 09:23:11] Request from 192.168.1.101
  [2024-01-15 09:24:05] Request from 10.0.0.55
  [2024-01-15 09:25:44] Request from 172.16.254.1
`

// Expected:
// timestamps: ["2024-01-15 09:23:11", "2024-01-15 09:24:05", "2024-01-15 09:25:44"]
// ips: ["192.168.1.101", "10.0.0.55", "172.16.254.1"]
```

---

### Q14. String Tokenizer

Write a `tokenize(expr)` function that splits a math expression string into an array of tokens (numbers, operators, parentheses).

```js
tokenize("3 + (42 * 7) - 15 / 3")
// ["3", "+", "(", "42", "*", "7", ")", "-", "15", "/", "3"]

tokenize("(10+2)*5")
// ["(", "10", "+", "2", ")", "*", "5"]
```

---

## 📦 Section 5 — Modules & Code Architecture

### Q15. Circular Dependency Problem

You have two modules:

```js
// a.js
import { b } from "./b.js"
export const a = `a uses ${b}`

// b.js
import { a } from "./a.js"
export const b = `b uses ${a}`
```

- What happens when you import either file?
- What will the values of `a` and `b` be?
- How do you restructure this to fix the circular dependency?

---

### Q16. Plugin System

Design a simple **plugin system** where a core `App` object can have plugins registered and executed in order.

```js
const app = createApp()

app.use((ctx, next) => {
  console.log("Plugin 1: before")
  next()
  console.log("Plugin 1: after")
})

app.use((ctx, next) => {
  console.log("Plugin 2")
  next()
})

app.run({})
// Plugin 1: before
// Plugin 2
// Plugin 1: after
```

This is exactly how **Express middleware** and **Koa** work. Implement it.

---

## 🧮 Section 6 — DSA Patterns in JavaScript

### Q17. Flatten Nested Array (Recursive + Iterative)

Write `flattenDeep(arr)` two ways — recursively and iteratively — without using `Array.flat()`.

```js
flattenDeep([1, [2, [3, [4]], 5]])
// [1, 2, 3, 4, 5]

flattenDeep([[[1]], [2, [3, [4, [5]]]]])
// [1, 2, 3, 4, 5]
```

---

### Q18. Group By (Native Polyfill)

Implement `groupBy(arr, keyFn)` — a polyfill for the native `Object.groupBy`.

```js
const people = [
  { name: "Alice", dept: "Engineering" },
  { name: "Bob",   dept: "Design" },
  { name: "Carol", dept: "Engineering" },
  { name: "Dave",  dept: "Design" },
]

groupBy(people, p => p.dept)
// {
//   Engineering: [{ name: "Alice", ... }, { name: "Carol", ... }],
//   Design:      [{ name: "Bob", ... }, { name: "Dave", ... }]
// }
```

---

### Q19. Linked List in JS

Implement a singly linked list in JavaScript with:
- `push(val)` — append to end
- `pop()` — remove from end
- `toArray()` — convert to array

```js
const list = new LinkedList()
list.push(1)
list.push(2)
list.push(3)

console.log(list.toArray()) // [1, 2, 3]
list.pop()
console.log(list.toArray()) // [1, 2]
```

---

### Q20. Stack & Queue Using Only Closures

Implement a `Stack` and a `Queue` using **only closures** — no classes, no arrays exposed externally.

```js
const stack = createStack()
stack.push(1); stack.push(2); stack.push(3)
stack.pop()     // 3
stack.peek()    // 2
stack.size()    // 2

const queue = createQueue()
queue.enqueue("a"); queue.enqueue("b")
queue.dequeue()  // "a"
queue.size()     // 1
```

---

## 🧪 Section 7 — Predict the Output (Hard Mode)

### Q21. Async Execution Order — The Hard One

Predict the **exact output order**. No running.

```js
console.log("A")

setTimeout(() => console.log("B"), 0)

Promise.resolve()
  .then(() => {
    console.log("C")
    setTimeout(() => console.log("D"), 0)
  })
  .then(() => console.log("E"))

setTimeout(() => console.log("F"), 0)

console.log("G")
```

---

### Q22. Reference vs Value Trap

What is the output? Why?

```js
const a = { x: 1 }
const b = a
b.x = 99

console.log(a.x) // ?

let m = 5
let n = m
n = 100

console.log(m) // ?
```

Now explain: what is the difference between how primitives and objects are stored in memory?

---

### Q23. Chained Promises — Predict Output

```js
Promise.resolve(1)
  .then(x => x + 1)
  .then(x => { throw new Error("Oops") })
  .then(x => console.log("then:", x))
  .catch(e => {
    console.log("catch:", e.message)
    return 10
  })
  .then(x => console.log("after catch:", x))
```

Predict every line printed.

---

### Q24. Class Field Trap

```js
class Animal {
  sound = "..."
  speak = () => console.log(this.sound)
}

class Dog extends Animal {
  sound = "Woof"
}

const d = new Dog()
d.speak() // ?
```

Predict the output and explain **why** it might surprise you.
What is the difference between a **class field arrow function** and a **prototype method**?

---

### Q25. The `delete` Operator

Predict what each line does:

```js
const obj = { a: 1, b: 2 }
delete obj.a
console.log(obj)         // ?
console.log("a" in obj)  // ?

const arr = [10, 20, 30]
delete arr[1]
console.log(arr)         // ?
console.log(arr.length)  // ?
```

Why is `delete` on arrays considered a bad practice?

---

## 🏆 Bonus — Deep End Challenges

### B1. Implement `async/await` Using Generators

`async/await` is syntactic sugar over **generators + promises**. Prove it.

Write a function `run(generatorFn)` that takes a generator function where `yield` is used in place of `await`, and makes it work like an async function.

```js
function* fetchUser() {
  const user  = yield fetch("/api/user").then(r => r.json())
  const posts = yield fetch(`/api/posts/${user.id}`).then(r => r.json())
  return posts
}

run(fetchUser).then(console.log)
```

---

### B2. Trie Data Structure

Implement a `Trie` (prefix tree) in JavaScript with:
- `insert(word)` — add a word
- `search(word)` — return `true` if the full word exists
- `startsWith(prefix)` — return `true` if any word starts with this prefix

```js
const trie = new Trie()
trie.insert("apple")
trie.insert("app")

trie.search("app")      // true
trie.search("apple")    // true
trie.search("ap")       // false
trie.startsWith("ap")   // true
trie.startsWith("ban")  // false
```

---

### B3. Scheduler — Concurrent Task Limiter

Write a `Scheduler` that runs async tasks with a **max concurrency limit**.

```js
const scheduler = new Scheduler(2) // max 2 tasks at a time

const task = (id, delay) => () =>
  new Promise(res => setTimeout(() => {
    console.log(`Task ${id} done`)
    res()
  }, delay))

scheduler.add(task(1, 1000))
scheduler.add(task(2, 500))
scheduler.add(task(3, 300))  // waits until task 1 or 2 finishes
scheduler.add(task(4, 400))  // waits until a slot opens
```

This pattern is used in real apps to avoid hammering APIs with too many concurrent requests.

---

> 🔥 **Part 2 Rule:** Sections 1–4 are building blocks. Sections 5–7 are where you separate good devs from great ones. The Bonus section is where you understand how JavaScript itself is built.
