# JavaScript — Ultimate Final Assessment

---

## Section 1 — Closures & Execution Context

### Q1. The Stale Closure Trap

This counter always logs `0` on click. Identify the stale closure, explain why it happens, and fix it two ways: using a ref pattern (plain object) and using a functional updater.

```js
function setupCounter() {
  let count = 0;

  document.getElementById("btn").addEventListener("click", () => {
    console.log("clicked", count); // always 0 — why?
  });

  setInterval(() => { count++; }, 1000);
}

setupCounter();
```

---

### Q2. Memoize with Cache Invalidation

Write `memoize(fn, ttl)` where `ttl` is in milliseconds. Cached results expire after `ttl` ms — recomputing on the next call. Handle multi-argument functions by serialising args as the cache key.

```js
const slow = (n) => n * n;

const fast = memoize(slow, 3000); // 3-second TTL

fast(5);  // computed  → 25
fast(5);  // cached    → 25 (instant)
// ... 3 seconds later ...
fast(5);  // re-computed → 25 (TTL expired)
```

---

## Section 2 — `this`, call, apply, bind

### Q3. Implement `Function.prototype.myBind`

Polyfill `.bind()` without using it. Must support partial application and must also work correctly when the bound function is used with `new`.

```js
Function.prototype.myBind = function(ctx, ...preset) {
  // your implementation
};

function greet(greeting, punctuation) {
  return `${greeting}, ${this.name}${punctuation}`;
}

const hi = greet.myBind({ name: "Anubhav" }, "Hello");
console.log(hi("!")); // "Hello, Anubhav!"
```

---

### Q4. Predict — `this` in Four Contexts

Predict every output before running. Then explain the rule governing each case.

```js
const obj = {
  x: 10,
  a() { return this.x; },
  b: () => this.x,
  c() { return (() => this.x)(); },
  d() {
    function inner() { return this.x; }
    return inner();
  }
};

console.log(obj.a()); // ?
console.log(obj.b()); // ?
console.log(obj.c()); // ?
console.log(obj.d()); // ?
```

---

## Section 3 — Prototypes & Classes

### Q5. Implement `Object.create` from Scratch

Write `myCreate(proto)` without using `Object.create`, `class`, or `new`. The result's prototype chain must be identical to the native version.

```js
function myCreate(proto) {
  // no Object.create allowed
}

const animal = { speak() { console.log(`${this.name} speaks`); } };
const dog = myCreate(animal);
dog.name = "Rex";

dog.speak(); // "Rex speaks"
console.log(Object.getPrototypeOf(dog) === animal); // true
```

---

### Q6. Class Field Arrow vs Prototype Method — Inheritance Bug

Predict the output of all three cases. Explain why Case C surprises most developers.

```js
// Case A — prototype method
class A1 { speak() { console.log("A1"); } }
class B1 extends A1 { speak() { console.log("B1"); } }
new B1().speak(); // ?

// Case B — class field arrow
class A2 { speak = () => console.log("A2"); }
class B2 extends A2 { speak = () => console.log("B2"); }
new B2().speak(); // ?

// Case C — field in parent, method in child
class A3 { speak = () => console.log("A3"); }
class B3 extends A3 { speak() { console.log("B3"); } }
new B3().speak(); // ?
```

---

### Q7. Mixin Composition + Method Collision

Implement the mixin pattern for `canFly`, `canSwim`, `canDive`. Both `canFly` and `canDive` define a `descend()` method. Compose all three into `Duck` so that `canDive`'s version wins — and explain why.

```js
const canFly  = { fly() {}, descend() { console.log("gliding"); } };
const canSwim = { swim() {} };
const canDive = { dive() {}, descend() { console.log("diving deep"); } };

class Duck { /* compose all three, canDive wins descend */ }

const d = new Duck();
d.descend(); // "diving deep"
```

---

## Section 4 — Async JavaScript & Concurrency

### Q8. Predict — Async Execution Order (Hard)

Write the exact output order before running. Explain each step using the call stack, microtask queue, and macrotask queue.

> **Hint:** `return Promise.resolve()` inside `.then` schedules an extra microtask tick before the next `.then` runs. Most people get E wrong.

```js
console.log("A");

setTimeout(() => console.log("B"), 0);

Promise.resolve()
  .then(() => {
    console.log("C");
    setTimeout(() => console.log("D"), 0);
    return Promise.resolve();
  })
  .then(() => console.log("E"));

queueMicrotask(() => console.log("F"));

setTimeout(() => console.log("G"), 0);

console.log("H");
```

---

### Q9. Promise Scheduler — Max Concurrency

Write a `Scheduler(limit)` class that runs async tasks with a maximum of `limit` tasks running at the same time. Queued tasks start as soon as a slot opens.

```js
const s = new Scheduler(2); // max 2 concurrent

const task = (id, ms) => () =>
  new Promise(res => setTimeout(() => {
    console.log(`task ${id} done`); res();
  }, ms));

s.add(task(1, 1000));
s.add(task(2, 500));
s.add(task(3, 300)); // waits for a slot
s.add(task(4, 400)); // waits for a slot

// Output order: 2 → 3 → 1 → 4  (by completion time, respecting limit)
```

---

### Q10. Implement `Promise.all` from Scratch

Write `myPromiseAll(promises)` without using `Promise.all`. Must resolve in order, reject immediately if any rejects, and handle non-promise values.

```js
function myPromiseAll(promises) {
  // your implementation
}

myPromiseAll([
  Promise.resolve(1),
  42,                // non-promise — treat as resolved
  new Promise(res => setTimeout(() => res(3), 100))
]).then(console.log); // [1, 42, 3]
```

---

## Section 5 — Metaprogramming · Proxy · Reflect

### Q11. Runtime-Typed Object with Proxy

Use `Proxy` to enforce a type schema at runtime. Setting a wrong type throws a `TypeError`. Setting an undeclared key also throws.

```js
const schema = { name: "string", age: "number", active: "boolean" };

const user = createTyped(schema);

user.name = "Anubhav";  // ✅
user.age  = 22;         // ✅
user.age  = "twenty";   // ❌ TypeError: age must be number
user.city = "Bhopal";   // ❌ TypeError: city not in schema
```

---

### Q12. Mini Reactive Store (Vue-style Signals)

Build `reactive(obj)` + `effect(fn)`. Reading a property inside `effect` tracks it as a dependency. When that property is written, only its subscribed effects re-run.

```js
const state = reactive({ count: 0, name: "Anubhav" });

effect(() => console.log("count =", state.count));
// immediately: "count = 0"

state.count++; // "count = 1"
state.name = "Rahul"; // (no log — count effect not subscribed to name)
state.count++; // "count = 2"
```

---

## Section 6 — Functional Patterns

### Q13. Full Curry with Arity Detection

Write `curry(fn)` that works for any function regardless of arity. Must support mixed-argument calls.

```js
const add = curry((a, b, c) => a + b + c);

add(1)(2)(3);   // 6
add(1, 2)(3);   // 6
add(1)(2, 3);   // 6
add(1, 2, 3);   // 6
```

---

### Q14. Async Pipe

Implement `asyncPipe(...fns)` that composes async functions left-to-right, awaiting each before passing the result to the next. Any rejection must short-circuit the pipeline.

```js
const double   = async n => n * 2;
const addTen   = async n => n + 10;
const validate = async n => {
  if (n > 50) throw new Error("too large");
  return n;
};

const process = asyncPipe(double, addTen, validate);

await process(5);   // 20  (5→10→20, passes)
await process(25);  // Error: too large  (25→50→60, fails)
```

---

## Section 7 — Generators & Iterators

### Q15. Implement `async/await` Using Generators

Write a `run(genFn)` runner that takes a generator where `yield` stands in for `await`. No `async/await` allowed inside `run` itself.

```js
function* fetchData() {
  const user  = yield fetchUser();
  const posts = yield fetchPosts(user.id);
  return posts;
}

run(fetchData).then(console.log);
```

---

### Q16. Lazy Generator Pipeline

Write `naturals()`, `filter(gen, pred)`, `map(gen, fn)`, and `take(gen, n)` as generators. Chain them to get: the first 5 squares of prime numbers greater than 10.

```js
const result = take(
  map(
    filter(filter(naturals(), isPrime), n => n > 10),
    n => n * n
  ),
  5
);

console.log([...result]); // figure out the correct values and verify
```

---

## Section 8 — Design Patterns

### Q17. EventEmitter with Wildcard Support

Build an `EventEmitter` with `on`, `off`, `emit`, `once`. Additionally, `on("*", fn)` must fire for every event emitted, receiving the event name as the first argument.

```js
const ee = createEmitter();

ee.on("*", (event, data) => console.log(`[*] ${event}`, data));
ee.once("login", user => console.log("first login:", user));

ee.emit("login", { name: "Anubhav" }); // wildcard + once fire
ee.emit("login", { name: "Rahul" });   // only wildcard fires
ee.emit("logout", null);               // only wildcard fires
```

---

### Q18. LRU Cache with O(1) Operations

Implement an LRU Cache using a `Map`. Both `get` and `put` must run in O(1). On capacity exceeded, evict the least recently used key.

```js
const cache = new LRUCache(2);

cache.put(1, "a");
cache.put(2, "b");
cache.get(1);       // "a" — 1 becomes MRU
cache.put(3, "c");  // evicts 2 (LRU)
cache.get(2);       // -1 (evicted)
cache.get(3);       // "c"
```

---

## Section 9 — Predict the Output: Final Gauntlet

### Q19. Prototype Pollution

Predict every line. Then fix `merge` to prevent prototype pollution.

```js
function merge(target, src) {
  for (let k in src) target[k] = src[k];
}

const payload = JSON.parse('{"__proto__":{"isAdmin":true}}');
const user = {};
merge(user, payload);

console.log(user.isAdmin);                  // ?
console.log({}.isAdmin);                    // ?
console.log(user.hasOwnProperty("isAdmin")); // ?
```

---

### Q20. Promise Chain + Error Recovery

Predict every line printed, including what value each `.then` receives after the `.catch`.

```js
Promise.resolve(1)
  .then(x => x * 2)
  .then(x => { throw new Error("boom"); })
  .then(x => console.log("then:", x))       // ?
  .catch(e => {
    console.log("catch:", e.message);        // ?
    return 99;
  })
  .then(x => console.log("after:", x))      // ?
  .finally(() => console.log("done"));      // ?
```

---

### Q21. The `new` Operator — Return Rules

Predict each output and state the exact rule: when does `new` ignore the constructor's return value?

```js
function A() { this.x = 1; return { x: 99 }; }
function B() { this.x = 1; return 42; }
function C() { this.x = 1; return null; }

console.log((new A()).x); // ?
console.log((new B()).x); // ?
console.log((new C()).x); // ?
```

---

### Q22. Coercion, `typeof`, and Edge Cases

Predict all 12 outputs. No calculator, no browser — mental model only.

```js
console.log([] + []);             // ?
console.log([] + {});             // ?
console.log({} + []);             // ? (watch the context)
console.log(+[]);                 // ?
console.log(+{});                 // ?
console.log(null + 1);            // ?
console.log(undefined + 1);       // ?
console.log(typeof null);         // ?
console.log(typeof class{});      // ?
console.log(NaN === NaN);         // ?
console.log([] instanceof Object);// ?
console.log(typeof [] === "array");// ?
```

---

## Section 10 — Data Structures in JS

### Q23. Trie — Autocomplete Engine

Implement a `Trie` with `insert`, `search`, `startsWith`, and a bonus `autocomplete(prefix)` that returns all stored words with that prefix.

```js
const t = new Trie();
["apple", "app", "apply", "apt", "bat"].forEach(w => t.insert(w));

t.search("app");        // true
t.search("ap");         // false
t.startsWith("ap");     // true
t.autocomplete("app");  // ["app", "apple", "apply"]
```

---

### Q24. Deep Object Diff with Path Reporting

Write `deepDiff(a, b)` that returns an array of change descriptors — each with the dot-notation path, old value, and new value. Handle nested objects and arrays.

```js
deepDiff(
  { user: { name: "Alice", scores: [90, 80] }, role: "user" },
  { user: { name: "Alice", scores: [90, 95] }, role: "admin" }
);

// [
//   { path: "user.scores.1", old: 80,     new: 95      },
//   { path: "role",          old: "user",  new: "admin" }
// ]
```

---

### Q25. Serialize Circular Objects Safely

`JSON.stringify` throws on circular references. Write `safeStringify(obj)` that replaces them with `"[Circular → path]"` showing where the cycle points.

```js
const obj = { a: { b: {} } };
obj.a.b.back = obj.a; // circular

console.log(safeStringify(obj));
// '{"a":{"b":{"back":"[Circular → $.a]"}}}'
```

---

## ⚔ Boss Round

> Each question fuses multiple domains. The whole thing must work.

---

### B01. Build a Mini Test Framework

Implement `describe`, `it`, and `expect` from scratch. Support `.toBe()`, `.toEqual()` (deep), `.toBeTruthy()`, `.toThrow()`. Print ✅ / ❌ per test and a summary. Must support async `it` callbacks.

```js
describe("Math", () => {
  it("adds correctly", () => expect(1 + 1).toBe(2));
  it("deep equals",   () => expect({a:1}).toEqual({a:1}));
  it("async test", async () => {
    const v = await Promise.resolve(42);
    expect(v).toBe(42);
  });
  it("catches throws", () => {
    expect(() => { throw new Error("!"); }).toThrow();
  });
});

// ✅ adds correctly
// ✅ deep equals
// ✅ async test
// ✅ catches throws
// Passed: 4 / 4
```

---

### B02. Plugin Middleware System (Koa-style)

Build `createApp()` whose `use(fn)` registers async middleware and `run(ctx)` executes them in order — each receiving `(ctx, next)` and able to run code before and after calling `next()`.

```js
const app = createApp();

app.use(async (ctx, next) => {
  console.log("1 →"); await next(); console.log("1 ←");
});
app.use(async (ctx, next) => {
  console.log("2 →"); await next(); console.log("2 ←");
});
app.use(async (ctx) => { ctx.body = "OK"; });

await app.run({});
// 1 →  2 →  2 ←  1 ←
```

---

### B03. Schema Validator with Nested Rules

Write `validate(data, schema)` supporting nested schemas, `required`/optional, `min`/`max` on numbers, custom `validate` functions, and structured errors with full dot-notation paths.

```js
const schema = {
  name:    { type: "string", required: true },
  age:     { type: "number", min: 0, max: 120 },
  address: {
    city:    { type: "string", required: true },
    pincode: { type: "number", validate: n => n.toString().length === 6 }
  }
};

validate({ name: 123, address: { pincode: 123 } }, schema);
// { valid: false, errors: [
//   "name must be string",
//   "age is required",
//   "address.city is required",
//   "address.pincode failed custom validation"
// ]}
```

---

### B04. Implement `Promise` from Scratch

Build `MyPromise` supporting `resolve`, `reject`, `.then` (chainable), `.catch`, and correct async microtask timing using `queueMicrotask`. `.then` must return a new `MyPromise`.

```js
new MyPromise((res) => setTimeout(() => res(42), 100))
  .then(x => x * 2)
  .then(x => console.log(x))  // 84
  .catch(e => console.error(e));
```

---

### B05. Reactive Form Engine *(Capstone)*

Using `Proxy`, closures, and your EventEmitter from Q17, build `createForm(schema)` that:
- Tracks field values reactively
- Runs validators on every change
- Tracks `dirty`/`touched` state per field
- Emits `"change"` and `"submit"` events with the current form state
- Must be under 100 lines

```js
const form = createForm({
  name:  { required: true, type: "string" },
  email: { required: true, validate: v => v.includes("@") }
});

form.on("change", state => console.log(state));

form.fields.name = "Anubhav";
// { name: "Anubhav", email: "", valid: false, errors: { email: "required" } }

form.fields.email = "anubhav@dev.com";
// { name: "Anubhav", email: "anubhav@dev.com", valid: true, errors: {} }

form.submit(); // emits "submit" with final state
```

---