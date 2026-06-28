# ⚡ JavaScript Advanced Freestyle — Practice Question Set

> **Level:** Advanced
> **Style:** Freestyle — No single topic. Anything goes.
> **Goal:** Think like a JS engineer, not just a coder.

---

## 🧠 Section 1 — Closures & Execution Context

### Q1. The Loop Trap

What does this print? Then fix it two different ways.

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1000)
}
```

**Expected after fix:**
```
0
1
2
```

**Fix 1:** Using `let`
**Fix 2:** Using an IIFE

---

### Q2. Counter Factory

Write a function `makeMultiplier(x)` that returns a new function.
The returned function takes a number `n` and returns `n * x`.

```js
const double = makeMultiplier(2)
const triple = makeMultiplier(3)

console.log(double(5))  // 10
console.log(triple(4))  // 12
console.log(double(triple(2))) // 12
```

---

### Q3. Once Function

Write a utility `once(fn)` that ensures `fn` is called **at most one time**,
no matter how many times the returned function is invoked.

```js
const init = once(() => console.log("Initialized!"))

init() // "Initialized!"
init() // (nothing)
init() // (nothing)
```

---

### Q4. Closure Memory Leak Trap

Look at this code:

```js
function outer() {
  const bigData = new Array(1000000).fill("🔥")

  return function inner() {
    console.log("inner called")
  }
}

const fn = outer()
fn()
```

- Does `bigData` get garbage collected after `outer()` runs?
- Why or why not?
- How would you fix this to prevent a memory leak?

---

## ⚙️ Section 2 — `this`, Call, Apply & Bind

### Q5. Lost `this`

What does this print and why?

```js
const user = {
  name: "Anubhav",
  greet() {
    console.log(`Hi, I'm ${this.name}`)
  }
}

const greet = user.greet
greet() // ?
```

Fix it using:
1. `.bind()`
2. Arrow function wrapper

---

### Q6. Explicit Binding

Using only `call` or `apply`, make `introduce` work for any object.

```js
function introduce(greeting, punctuation) {
  console.log(`${greeting}, I am ${this.name}${punctuation}`)
}

const person1 = { name: "Rahul" }
const person2 = { name: "Aman" }

// Expected:
// "Hello, I am Rahul!"
// "Hey, I am Aman?"
```

---

### Q7. Arrow vs Regular `this`

Predict the output of both. Explain the difference.

```js
// Case A
const obj = {
  val: 42,
  getVal: function () {
    return () => this.val
  }
}
console.log(obj.getVal()()) // ?

// Case B
const obj2 = {
  val: 42,
  getVal: () => {
    return () => this.val
  }
}
console.log(obj2.getVal()()) // ?
```

---

### Q8. Custom `bind` Polyfill

Implement your own version of `.bind()` — called `myBind` — without using the native `.bind()`.

```js
function greet(greeting, punctuation) {
  return `${greeting}, ${this.name}${punctuation}`
}

const boundGreet = myBind(greet, { name: "Anubhav" }, "Hello")
console.log(boundGreet("!")) // "Hello, Anubhav!"
```

---

## 🔄 Section 3 — Async JS, Promises & Event Loop

### Q9. Event Loop Prediction

Predict the **exact output order** and explain why.

```js
console.log("1")

setTimeout(() => console.log("2"), 0)

Promise.resolve().then(() => console.log("3"))

console.log("4")
```

---

### Q10. Promise Chaining

Rewrite this callback hell using **Promise chaining**.

```js
getUser(id, function(user) {
  getPosts(user.id, function(posts) {
    getComments(posts[0].id, function(comments) {
      console.log(comments)
    })
  })
})
```

Then rewrite again using **async/await**.

---

### Q11. Promise.all vs Promise.allSettled

You have three API calls. One of them **always fails**.

```js
const p1 = Promise.resolve("Data from API 1")
const p2 = Promise.reject("API 2 failed")
const p3 = Promise.resolve("Data from API 3")
```

- What does `Promise.all([p1, p2, p3])` return?
- What does `Promise.allSettled([p1, p2, p3])` return?
- When would you prefer one over the other?

---

### Q12. Async Retry Mechanism

Write an `asyncRetry(fn, retries)` function that calls an async function `fn` and retries it up to `retries` times if it throws an error. If all attempts fail, throw the last error.

```js
let attempts = 0
const unstable = async () => {
  attempts++
  if (attempts < 3) throw new Error("Not ready yet")
  return "Success!"
}

asyncRetry(unstable, 5).then(console.log) // "Success!" (on 3rd try)
```

---

### Q13. Sequential vs Parallel Async

What is the difference in execution time between these two?

```js
// Version A
async function runA() {
  const a = await fetch("/api/user")
  const b = await fetch("/api/posts")
  return [a, b]
}

// Version B
async function runB() {
  const [a, b] = await Promise.all([fetch("/api/user"), fetch("/api/posts")])
  return [a, b]
}
```

Assume each fetch takes 1 second. What is the total time for A? For B?
Rewrite Version A to be as fast as Version B without using `Promise.all`.

---

## 🧬 Section 4 — Prototypes, Classes & Inheritance

### Q14. Class vs Constructor Function

Implement the same `Person` type using:
1. A **constructor function** with prototype methods
2. An ES6 **class**

Both must support:
```js
const p = new Person("Anubhav", 22)
p.greet()   // "Hi, I'm Anubhav and I'm 22"
p.birthday() // increases age by 1
```

---

### Q15. Inheritance + super

Create a class hierarchy:

```
Shape
  ├── Circle
  └── Rectangle
```

- `Shape` has a `color` property and a `describe()` method
- `Circle` extends `Shape`, adds `radius`, overrides `area()`
- `Rectangle` extends `Shape`, adds `width` and `height`, overrides `area()`

```js
const c = new Circle("red", 5)
c.describe()  // "I am a red shape"
c.area()      // 78.53...

const r = new Rectangle("blue", 4, 6)
r.area()      // 24
```

---

### Q16. instanceof Surprise

Predict the output:

```js
class Animal {}
class Dog extends Animal {}

const d = new Dog()

console.log(d instanceof Dog)     // ?
console.log(d instanceof Animal)  // ?
console.log(d instanceof Object)  // ?

console.log(Object.getPrototypeOf(Dog) === Animal) // ?
```

Explain the full prototype chain of `d`.

---

## 🏗️ Section 5 — Patterns & Advanced Concepts

### Q17. Currying

Write a `curry(fn)` function that converts any function into a **curried** version.

```js
function add(a, b, c) {
  return a + b + c
}

const curriedAdd = curry(add)

curriedAdd(1)(2)(3)  // 6
curriedAdd(1, 2)(3)  // 6
curriedAdd(1)(2, 3)  // 6
curriedAdd(1, 2, 3)  // 6
```

---

### Q18. Pipe & Compose

Implement `pipe(...fns)` and `compose(...fns)`.

- `pipe` applies functions **left to right**
- `compose` applies functions **right to left**

```js
const double  = x => x * 2
const addTen  = x => x + 10
const square  = x => x * x

const transform = pipe(double, addTen, square)
console.log(transform(3)) // ((3*2)+10)^2 = 256

const transform2 = compose(square, addTen, double)
console.log(transform2(3)) // same result: 256
```

---

### Q19. Debounce

Implement a `debounce(fn, delay)` function that delays invoking `fn` until after `delay` ms have passed **since the last call**.

```js
const log = debounce(() => console.log("Searched!"), 300)

log() // called
log() // reset timer
log() // reset timer
// 300ms later → "Searched!" (only once)
```

**Use case context:** search bar input — you don't want to fire an API call on every keystroke.

---

### Q20. Throttle

Implement a `throttle(fn, limit)` function that ensures `fn` is called **at most once** every `limit` milliseconds, no matter how many times it's triggered.

```js
const save = throttle(() => console.log("Saved!"), 1000)

save() // "Saved!"
save() // ignored
save() // ignored
// after 1000ms
save() // "Saved!"
```

**Explain the difference between debounce and throttle.** When would you use each?

---

## 🧪 Section 6 — Tricky & Predict the Output

### Q21. Type Coercion Chaos

Predict the output of each. No running allowed — think first.

```js
console.log([] + [])         // ?
console.log([] + {})         // ?
console.log({} + [])         // ?
console.log(+"")             // ?
console.log(+[])             // ?
console.log(+{})             // ?
console.log(null + 1)        // ?
console.log(undefined + 1)   // ?
console.log("5" - 3)         // ?
console.log("5" + 3)         // ?
```

---

### Q22. Hoisting Riddle

What is the output? Explain each line.

```js
console.log(a)  // ?
console.log(b)  // ?
console.log(c)  // ?

var a = 1
let b = 2
const c = 3
```

---

### Q23. Scope Chain

What does this print?

```js
let x = "global"

function outer() {
  let x = "outer"

  function inner() {
    console.log(x)
  }

  inner()
}

outer()
console.log(x)
```

Now change `inner` to:
```js
function inner() {
  let x = "inner"
  console.log(x)
}
```
What changes?

---

### Q24. Prototype Pollution Awareness

Look at this function:

```js
function merge(target, source) {
  for (let key in source) {
    target[key] = source[key]
  }
}

const payload = JSON.parse('{"__proto__": {"isAdmin": true}}')
const user = {}
merge(user, payload)

console.log(user.isAdmin)   // ?
console.log({}.isAdmin)     // ?
```

- What is **prototype pollution**?
- What is the output?
- How do you fix `merge` to prevent this?

---

### Q25. Generator — Custom Range

Write a **generator function** `range(start, end, step = 1)` that yields numbers from `start` to `end` (exclusive) with a given step.

```js
for (const n of range(0, 10, 2)) {
  console.log(n) // 0, 2, 4, 6, 8
}

console.log([...range(1, 6)])    // [1, 2, 3, 4, 5]
console.log([...range(0, 1, 0.25)]) // [0, 0.25, 0.5, 0.75]
```

---

## 🏆 Bonus — Real World Challenges

### B1. Implement `Promise` from Scratch

Build a simplified `MyPromise` class that supports:
- `resolve` and `reject`
- `.then(onFulfilled, onRejected)`
- `.catch(onRejected)`

It does **not** need to handle async microtask queuing perfectly — but the chain must work.

---

### B2. Virtual DOM Differ

Write a function `diff(oldTree, newTree)` that compares two virtual DOM trees (plain objects) and returns a list of changes.

```js
const oldTree = { tag: "div", props: { id: "app" }, children: ["Hello"] }
const newTree = { tag: "div", props: { id: "app" }, children: ["Hello", "World"] }

diff(oldTree, newTree)
// [{ type: "CHILDREN_CHANGED", added: ["World"] }]
```

---

### B3. Reactive State (Mini Vue/Signal)

Build a `reactive(obj)` function using `Proxy` that:
- Tracks which properties are **read** (dependency tracking)
- Automatically calls a registered **effect** function whenever a tracked property changes

```js
const state = reactive({ count: 0 })

effect(() => {
  console.log("Count is:", state.count)
})
// "Count is: 0" — runs immediately

state.count = 1  // "Count is: 1" — auto re-runs
state.count = 2  // "Count is: 2" — auto re-runs
```

> This is the core idea behind Vue 3's Composition API and JavaScript Signals.

---

> 🧩 **Challenge Rule:** For every question in Section 6, write your prediction **before** running the code. Only then verify. The goal is to build an accurate mental model of how JavaScript actually executes.
