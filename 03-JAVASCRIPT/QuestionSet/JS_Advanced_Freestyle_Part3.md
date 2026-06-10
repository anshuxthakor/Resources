# ⚡ JavaScript Advanced Freestyle — Part 3

> **Level:** Advanced / Expert
> **Style:** Freestyle — New territory every time.
> **New Themes:** Metaprogramming, Functional JS, State Machines, Concurrency, Browser APIs, Security, Runtime Type Checking

---

## 🪄 Section 1 — Metaprogramming (Proxy & Reflect)

### Q1. Schema-Enforced Object

Use `Proxy` to create an object that **enforces a type schema** at runtime. Any property set with the wrong type should throw a `TypeError`.

```js
const schema = { name: "string", age: "number", active: "boolean" }

const user = createTypedObject(schema)

user.name = "Anubhav"  // ✅
user.age = 22          // ✅
user.age = "twenty"    // ❌ TypeError: age must be of type number
user.city = "Bhopal"   // ❌ TypeError: city is not defined in schema
```

---

### Q2. Negative Array Indexing

Use `Proxy` to allow Python-style **negative indexing** on arrays.

```js
const arr = proxiedArray([10, 20, 30, 40, 50])

console.log(arr[-1])  // 50
console.log(arr[-2])  // 40
console.log(arr[0])   // 10
```

---

### Q3. Auto-Vivification Object

Use `Proxy` to create an object where accessing any nested path — no matter how deep — **never throws**, and missing paths auto-create themselves.

```js
const data = autoVivify()

data.user.address.city = "Bhopal"
console.log(data.user.address.city)  // "Bhopal"
console.log(data.a.b.c.d.e)          // {} — no error, just empty object
```

> This pattern is common in config management and tree structures.

---

### Q4. Reflect — Implement a Transparent Logging Proxy

Using `Reflect`, write a `createLogger(target)` that wraps any object and **logs every operation** (get, set, delete, has) — while still forwarding all operations to the original object correctly.

```js
const obj = createLogger({ x: 10 })

obj.x          // [GET] x → 10
obj.y = 99     // [SET] y = 99
delete obj.x   // [DELETE] x
"y" in obj     // [HAS] y → true
```

The key requirement: use `Reflect.*` to forward every operation so the proxy behaves identically to the original.

---

## 🧩 Section 2 — Functional JavaScript

### Q5. Functor — Mappable Box

Implement a `Box` type that wraps a value and supports `.map()` chaining — similar to how `Array.map` works but for a single value.

```js
const result = Box(3)
  .map(x => x * 2)
  .map(x => x + 1)
  .map(x => `Result: ${x}`)
  .value()

console.log(result) // "Result: 7"
```

Then implement a `SafeBox` that handles `null`/`undefined` values gracefully — skipping all maps if the value is nullish (like `Optional/Maybe` monad).

```js
SafeBox(null)
  .map(x => x.name)   // skipped — no error
  .map(x => x.toUpperCase())  // skipped
  .value()  // null
```

---

### Q6. Transducer

You have a large array. Using regular `.filter().map()` creates intermediate arrays. Write a **transducer** that composes filter and map into a **single pass** with no intermediate arrays.

```js
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

// Normal way — creates 2 arrays:
const result = numbers.filter(x => x % 2 === 0).map(x => x * x)

// Transducer way — single pass, one array:
const xform = compose(
  filter(x => x % 2 === 0),
  map(x => x * x)
)
const result2 = transduce(xform, numbers)

console.log(result2) // [4, 16, 36, 64, 100]
```

---

### Q7. Partial Application

Write `partial(fn, ...presetArgs)` that partially applies arguments to a function, returning a new function that accepts the remaining ones.

```js
function log(level, timestamp, message) {
  console.log(`[${level}] ${timestamp}: ${message}`)
}

const warn  = partial(log, "WARN")
const error = partial(log, "ERROR")

warn("12:00", "Disk space low")
// [WARN] 12:00: Disk space low

const errorAt12 = partial(log, "ERROR", "12:00")
errorAt12("Server crashed")
// [ERROR] 12:00: Server crashed
```

Explain the difference between **partial application** and **currying**.

---

### Q8. Lazy Evaluation with Thunks

A **thunk** is a function that wraps a value to defer its computation.

Implement a `lazy(fn)` wrapper that:
- Does **not** call `fn` immediately
- Computes the value **only on first access**
- **Caches** the result so `fn` never runs twice

```js
let count = 0

const expensive = lazy(() => {
  count++
  return 42
})

console.log(count)          // 0 — not computed yet
console.log(expensive())    // 42 — computed now
console.log(count)          // 1
console.log(expensive())    // 42 — from cache
console.log(count)          // still 1 — fn never ran again
```

---

## 🔀 Section 3 — State Machines & Patterns

### Q9. Finite State Machine

Implement a **traffic light** as a finite state machine with states and valid transitions.

```
green → yellow → red → green → ...
```

```js
const light = createFSM({
  initial: "green",
  transitions: {
    green:  { next: "yellow" },
    yellow: { next: "red"    },
    red:    { next: "green"  }
  }
})

light.state()    // "green"
light.next()
light.state()    // "yellow"
light.next()
light.next()
light.state()    // "green"

// Invalid transition attempt:
light.go("purple") // Error: "purple" is not a valid state
```

---

### Q10. Strategy Pattern

Implement a `Sorter` that accepts different **sorting strategies** at runtime — without changing the core class.

```js
const bubbleSort  = arr => { /* bubble sort logic */ }
const mergeSort   = arr => { /* merge sort logic */ }
const nativeSort  = arr => [...arr].sort((a, b) => a - b)

const sorter = new Sorter(nativeSort)
sorter.sort([5, 3, 1, 4])  // [1, 3, 4, 5]

sorter.setStrategy(bubbleSort)
sorter.sort([5, 3, 1, 4])  // same result, different algorithm
```

Write at least two strategy implementations (bubble sort and one other).

---

### Q11. Command Pattern — Undo/Redo

Implement a text editor `buffer` with an **undo/redo** system using the Command pattern.

```js
const editor = createEditor()

editor.type("Hello")
editor.type(" World")
editor.getText()   // "Hello World"

editor.undo()
editor.getText()   // "Hello"

editor.undo()
editor.getText()   // ""

editor.redo()
editor.getText()   // "Hello"
```

---

### Q12. Observable / Reactive Store

Build a mini `createStore(initialState)` (like Redux, without reducers) that:
- Holds state
- Allows `getState()`
- Allows `setState(partial)` — merges partial updates
- Allows `subscribe(fn)` — calls `fn` whenever state changes
- Returns an `unsubscribe` function

```js
const store = createStore({ count: 0, user: null })

const unsub = store.subscribe((newState, oldState) => {
  console.log("Changed:", oldState, "→", newState)
})

store.setState({ count: 1 })
// Changed: { count: 0, user: null } → { count: 1, user: null }

unsub()
store.setState({ count: 2 }) // no log — unsubscribed
```

---

## ⚡ Section 4 — Concurrency & Timing

### Q13. Race Condition Simulation

This code has a **race condition**. Identify it and fix it.

```js
let balance = 1000

async function withdraw(amount) {
  const current = balance          // read
  await delay(100)                 // simulate async DB call
  balance = current - amount       // write
}

await Promise.all([
  withdraw(200),
  withdraw(300),
])

console.log(balance)
// Expected: 500
// Actual: ???  — why?
```

Fix it using a **mutex lock** pattern.

---

### Q14. Promise Queue (Serial Execution)

Write a `PromiseQueue` that accepts async tasks and executes them **one at a time**, in order — even if they're added rapidly.

```js
const queue = new PromiseQueue()

queue.add(() => delay(300).then(() => console.log("Task 1")))
queue.add(() => delay(100).then(() => console.log("Task 2")))
queue.add(() => delay(200).then(() => console.log("Task 3")))

// Output (always in order, not by speed):
// Task 1
// Task 2
// Task 3
```

---

### Q15. `AbortController` — Cancellable Fetch

Write a `fetchWithTimeout(url, ms)` function that:
- Automatically **cancels** the fetch if it takes longer than `ms` milliseconds
- Returns the data if it succeeds in time
- Throws a descriptive `TimeoutError` if it times out

```js
try {
  const data = await fetchWithTimeout("/api/slow", 2000)
  console.log(data)
} catch (e) {
  console.log(e.message) // "Request timed out after 2000ms"
}
```

Use `AbortController` and `AbortSignal`.

---

## 🌐 Section 5 — Browser APIs & Environment

### Q16. Event Delegation

Instead of attaching 1000 listeners to a list, use **event delegation** to handle all clicks with a single listener on the parent.

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

Explain why this is more performant and how it handles **dynamically added items**.

---

### Q17. IntersectionObserver — Lazy Load Images

Write a function `lazyLoadImages()` that:
- Finds all `<img>` tags with `data-src` attribute
- Uses `IntersectionObserver` to load the real `src` **only when the image enters the viewport**
- Stops observing the image once it has loaded

```js
// HTML:
// <img data-src="photo.jpg" alt="Photo" />

lazyLoadImages() // call once on page load
```

Explain why this is better than loading all images upfront or using a scroll event listener.

---

### Q18. Custom `localStorage` Wrapper

Build a `storage` utility that:
- **Serializes/deserializes** values automatically (so you can store objects, arrays)
- Supports a **TTL (time-to-live)** in milliseconds — expired entries return `null`
- Has `get(key)`, `set(key, value, ttl?)`, `remove(key)`, `clear()` methods

```js
storage.set("session", { userId: 42 }, 5000) // expires in 5s
storage.get("session")   // { userId: 42 }

// after 5 seconds:
storage.get("session")   // null — expired, auto-removed
```

---

## 🔐 Section 6 — Security & Defensive Coding

### Q19. XSS Prevention — Safe HTML Renderer

Write a `sanitize(str)` function that escapes all HTML special characters so user input cannot inject scripts.

```js
const userInput = '<script>alert("hacked")</script><b>Hello</b>'

console.log(sanitize(userInput))
// &lt;script&gt;alert(&quot;hacked&quot;)&lt;/script&gt;&lt;b&gt;Hello&lt;/b&gt;
```

Then write a tagged template `safeHTML` that sanitizes all interpolated values automatically.

```js
const name = '<img src=x onerror=alert(1)>'
const html = safeHTML`<p>Welcome, ${name}!</p>`
// "<p>Welcome, &lt;img src=x onerror=alert(1)&gt;!</p>"
```

---

### Q20. Deep Object Freeze (Recursive)

`Object.freeze()` only freezes the **top level**. Write `deepFreeze(obj)` that freezes an object and all its nested objects recursively.

```js
const config = deepFreeze({
  server: {
    host: "localhost",
    port: 3000,
    nested: { timeout: 5000 }
  }
})

config.server.port = 9999              // silently fails (or throws in strict mode)
config.server.nested.timeout = 99999  // also fails
console.log(config.server.port)       // 3000 — unchanged
```

---

## 🧪 Section 7 — Predict the Output (Expert Mode)

### Q21. The `arguments` Object Trap

```js
function test(a, b) {
  arguments[0] = 99
  console.log(a)   // ?

  b = 100
  console.log(arguments[1])  // ?
}

test(1, 2)
```

Now rewrite `test` as an arrow function. What changes? What breaks?

---

### Q22. Prototype Property Shadowing

```js
function Person(name) {
  this.name = name
}
Person.prototype.name = "Default"

const p1 = new Person("Alice")
const p2 = new Person()

console.log(p1.name)           // ?
console.log(p2.name)           // ?

delete p1.name
console.log(p1.name)           // ?

delete p2.name
console.log(p2.name)           // ?
```

Trace through exactly what happens at each step using prototype chain rules.

---

### Q23. `new` Operator — What It Really Does

What does this return and why?

```js
function createUser(name) {
  this.name = name
  return { name: "Impostor" }
}

const u1 = new createUser("Anubhav")
console.log(u1.name)  // ?

function createRole(name) {
  this.role = name
  return 42  // primitive returned
}

const u2 = new createRole("admin")
console.log(u2.role)  // ?
```

Explain the rules of what `new` does when the constructor returns an object vs a primitive.

---

### Q24. `typeof` and `instanceof` Edge Cases

Predict each output:

```js
console.log(typeof null)               // ?
console.log(typeof undefined)          // ?
console.log(typeof function(){})       // ?
console.log(typeof class{})            // ?
console.log(typeof NaN)                // ?

console.log(NaN === NaN)               // ?
console.log(null == undefined)         // ?
console.log(null === undefined)        // ?

console.log([] instanceof Array)       // ?
console.log([] instanceof Object)      // ?
console.log(typeof [] === "array")     // ?
```

---

### Q25. Short-Circuit Evaluation — Deep Cut

Predict every output:

```js
console.log(0 || "default")         // ?
console.log("" || false || "hello") // ?
console.log(null ?? "fallback")     // ?
console.log(0 ?? "fallback")        // ?
console.log(false && "unreached")   // ?
console.log(1 && 2 && 3)           // ?

const obj = null
console.log(obj?.user?.name)        // ?
console.log(obj?.user?.name ?? "Guest") // ?
```

Explain the difference between `||`, `&&`, `??` and `?.` — and when each is appropriate.

---

## 🏆 Bonus — Language Internals

### B1. Implement `Object.create` from Scratch

Implement `myCreate(proto, propertiesObject)` — a polyfill for `Object.create()` — without using `Object.create` itself.

```js
const animal = {
  speak() { console.log(`${this.name} makes a sound.`) }
}

const dog = myCreate(animal, {
  name: { value: "Rex", writable: true, enumerable: true }
})

dog.speak()   // "Rex makes a sound."
console.log(Object.getPrototypeOf(dog) === animal) // true
```

---

### B2. Serialize & Deserialize a Circular Object

`JSON.stringify` throws on circular references. Write `safeStringify(obj)` and `safeParse(str)` that handle circular references by replacing them with a `"[Circular]"` marker.

```js
const obj = { name: "Anubhav" }
obj.self = obj  // circular reference

const str = safeStringify(obj)
console.log(str)
// '{"name":"Anubhav","self":"[Circular]"}'
```

---

### B3. Build a Mini Test Runner

Implement a minimal `describe` / `it` / `expect` test framework from scratch.

```js
describe("Array utilities", () => {
  it("flattens nested arrays", () => {
    expect(flattenDeep([1, [2, [3]]])).toEqual([1, 2, 3])
  })

  it("filters correctly", () => {
    expect([1, 2, 3].filter(x => x > 1)).toEqual([2, 3])
  })

  it("fails on wrong value", () => {
    expect(1 + 1).toEqual(3)  // this should fail
  })
})

// Console Output:
// Array utilities
//   ✅ flattens nested arrays
//   ✅ filters correctly
//   ❌ fails on wrong value → Expected 3 but got 2
```

Support: `.toEqual()`, `.toBe()`, `.toBeTruthy()`, `.toThrow()`.

---

> 🧠 **Part 3 Rule:** Every predict-the-output question in Section 7 has at least one answer that contradicts what most developers assume. Treat each one as a bug hunt — if your prediction matches reality, you understand JavaScript. If it doesn't, you've found a gap worth closing.
