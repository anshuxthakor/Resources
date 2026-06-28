# 📑 JavaScript Advanced Practice Questions (Phase - 3)
> **Prerequisites:** Completion of Phase 1 & Phase 2 (Objects Basics)
> **Focus:** Deep Object Mechanics, Prototypes, Patterns & Real-World Problem Solving

---

## 🔷 Level 1 — Object Internals & Control

### 1. Freeze vs Seal

Given the following object:

```js
const config = {
  apiUrl: "https://api.example.com",
  timeout: 3000,
  retries: 5
}
```

- Freeze the object and try to modify `timeout`. What happens?
- Now do the same with `Object.seal()`. What is the difference?

**Expected Output:**
```
Object.freeze → no changes allowed (even existing keys)
Object.seal  → existing keys can be updated, but no add/delete
```

---

### 2. Property Descriptors

Use `Object.defineProperty()` to create a property `id` on a user object that is:
- **non-writable** (cannot be changed)
- **non-enumerable** (does not appear in loops)
- **non-configurable** (cannot be deleted or redefined)

```js
const user = { name: "Anubhav" }
// add id: 101 with above constraints
```

Verify by trying to change, delete, and loop over `id`.

---

### 3. Getter & Setter

Create an object `circle` with:
- A stored property `_radius`
- A **getter** `radius` that returns `_radius`
- A **setter** `radius` that throws an error if the value is negative
- A **getter** `area` that returns `π * r²` (read-only, no setter)

```js
circle.radius = 5    // ✅ works
circle.radius = -3   // ❌ throws Error: "Radius cannot be negative"
console.log(circle.area) // 78.53...
```

---

### 4. Object.create() — Prototype Chain

Create a base object `animal` with a method `speak()` that logs `"..."`.

Then use `Object.create(animal)` to create a `dog` object that **overrides** `speak()` to log `"Woof!"`.

Verify that `dog` still has access to the prototype chain.

```js
console.log(Object.getPrototypeOf(dog) === animal) // true
```

---

### 5. Symbol as Property Key

Create an object where a private-like ID is stored using a `Symbol` key so it does **not** appear in `Object.keys()`, `for...in`, or `JSON.stringify()`.

```js
const SECRET_ID = Symbol("id")
const user = {
  name: "Rahul",
  [SECRET_ID]: "USR-9021"
}

// accessing it:
console.log(user[SECRET_ID]) // "USR-9021"
console.log(Object.keys(user)) // ["name"] — Symbol hidden
```

---

## 🔷 Level 2 — Cloning, Transformation & Immutability

### 6. Deep Clone Without JSON

Write a function `deepClone(obj)` that **deeply clones** an object, including:
- Nested objects
- Arrays inside objects
- **Without** using `JSON.parse(JSON.stringify())` (that method loses functions)

```js
const original = {
  name: "Aman",
  scores: [90, 80],
  address: { city: "Pune" }
}
const clone = deepClone(original)
clone.address.city = "Mumbai"

console.log(original.address.city) // "Pune" — unchanged
```

---

### 7. Immutable Update Pattern

Write a function `updateUser(user, updates)` that returns a **new object** with updates applied, without mutating the original.

```js
const user = { name: "John", age: 25, role: "user" }
const updated = updateUser(user, { age: 26, role: "admin" })

console.log(user)    // { name: "John", age: 25, role: "user" } — unchanged
console.log(updated) // { name: "John", age: 26, role: "admin" }
```

---

### 8. Flatten a Nested Object

Write a function `flattenObject(obj)` that converts a deeply nested object into a **single-level object** with dot-notation keys.

```js
const nested = {
  user: {
    name: "Anubhav",
    address: {
      city: "Bhopal",
      pincode: 462001
    }
  },
  score: 95
}
```

**Expected Output:**
```js
{
  "user.name": "Anubhav",
  "user.address.city": "Bhopal",
  "user.address.pincode": 462001,
  "score": 95
}
```

---

### 9. Unflatten an Object

Write the reverse of Q8. Convert a flat dot-notation object back into a nested object.

```js
const flat = {
  "user.name": "Anubhav",
  "user.address.city": "Bhopal",
  "score": 95
}
```

**Expected Output:**
```js
{
  user: { name: "Anubhav", address: { city: "Bhopal" } },
  score: 95
}
```

---

### 10. Object Diff

Write a function `diffObjects(obj1, obj2)` that returns an object showing only the **changed keys** with their old and new values.

```js
const before = { name: "Alice", age: 25, city: "Delhi" }
const after  = { name: "Alice", age: 26, city: "Mumbai" }
```

**Expected Output:**
```js
{
  age:  { old: 25, new: 26 },
  city: { old: "Delhi", new: "Mumbai" }
}
```

---

## 🔷 Level 3 — Prototype & Class Patterns

### 11. Prototype Inheritance Chain

Create a prototype chain manually (without `class`):

- `Vehicle` → has `fuelType` and `start()` method
- `Car` → inherits from `Vehicle`, adds `brand` and `honk()` method
- `ElectricCar` → inherits from `Car`, **overrides** `fuelType` to `"electric"`

Verify:
```js
const tesla = Object.create(ElectricCar)
tesla.start()  // inherited from Vehicle
tesla.honk()   // inherited from Car
console.log(tesla.fuelType) // "electric"
```

---

### 12. Mixin Pattern

You cannot use multiple inheritance in JS. Implement the **mixin pattern** to compose behaviors.

Create three mixins:
- `canSwim` → `swim()` logs `"Swimming..."`
- `canFly` → `fly()` logs `"Flying..."`
- `canRun` → `run()` logs `"Running..."`

Then create a `Duck` object that uses all three mixins.

```js
duck.swim() // "Swimming..."
duck.fly()  // "Flying..."
duck.run()  // "Running..."
```

---

### 13. Factory Function with Private State

Create a factory function `createCounter(start = 0)` using **closures** to keep count private. Return an object with:
- `increment()` → increases count by 1
- `decrement()` → decreases count by 1
- `reset()` → resets count to initial `start` value
- `getCount()` → returns current count

The `count` variable must **not** be directly accessible from outside.

```js
const c = createCounter(10)
c.increment()
c.increment()
console.log(c.getCount()) // 12
console.log(c.count)      // undefined — private!
```

---

### 14. Method Chaining (Builder Pattern)

Create a `QueryBuilder` object that allows **chaining** like this:

```js
const query = QueryBuilder
  .select("name", "age")
  .from("users")
  .where("age > 18")
  .limit(10)
  .build()

console.log(query)
// "SELECT name, age FROM users WHERE age > 18 LIMIT 10"
```

Each method must return `this` (or a new builder) to enable chaining.

---

### 15. Observable Object with Proxy

Use JavaScript `Proxy` to create an **observable object** that logs every get and set operation.

```js
const user = createObservable({ name: "Anubhav", age: 22 })

user.name        // logs: [GET] name → "Anubhav"
user.age = 23    // logs: [SET] age: 22 → 23
```

**Hint:** Use `new Proxy(target, handler)` with `get` and `set` traps.

---

## 🔷 Level 4 — Real-World Problem Solving

### 16. Deep Merge Two Objects

Write `deepMerge(obj1, obj2)` that recursively merges nested objects (unlike spread `{...obj1, ...obj2}` which only does shallow merge).

```js
const defaults = {
  theme: { color: "blue", font: "sans" },
  timeout: 3000
}
const userPrefs = {
  theme: { color: "red" }
}
```

**Expected Output:**
```js
{
  theme: { color: "red", font: "sans" },
  timeout: 3000
}
```

Spread operator alone would give `theme: { color: "red" }` — losing `font`. Your function must not.

---

### 17. Cache / Memoize with Object

Write a `memoize(fn)` function that caches the results of expensive function calls using an object as a cache store.

```js
function slowSquare(n) {
  // imagine this takes 2 seconds
  return n * n
}

const fastSquare = memoize(slowSquare)

fastSquare(5)  // computed → 25
fastSquare(5)  // from cache → 25 (instant)
fastSquare(9)  // computed → 81
```

---

### 18. Schema Validator

Write a function `validate(data, schema)` that validates an object against a schema definition.

```js
const schema = {
  name:  { type: "string",  required: true  },
  age:   { type: "number",  required: true  },
  email: { type: "string",  required: false }
}

validate({ name: "Anubhav", age: 22 }, schema)
// ✅ { valid: true, errors: [] }

validate({ name: 123, age: 22 }, schema)
// ❌ { valid: false, errors: ["name must be of type string"] }

validate({ age: 22 }, schema)
// ❌ { valid: false, errors: ["name is required"] }
```

---

### 19. EventEmitter (Pub/Sub Pattern)

Implement a simple `EventEmitter` object from scratch with:
- `on(event, listener)` → registers a listener
- `off(event, listener)` → removes a listener
- `emit(event, ...args)` → calls all listeners for that event

```js
const emitter = createEventEmitter()

const greet = (name) => console.log(`Hello, ${name}!`)

emitter.on("greet", greet)
emitter.emit("greet", "Anubhav")  // "Hello, Anubhav!"
emitter.off("greet", greet)
emitter.emit("greet", "Anubhav")  // nothing — listener removed
```

---

### 20. LRU Cache Using Objects & Map

Implement an **LRU (Least Recently Used) Cache** class/factory with:
- `get(key)` → returns the value, or `-1` if not found. Marks as recently used.
- `put(key, value)` → inserts key-value. If capacity is exceeded, evicts the least recently used entry.

```js
const cache = createLRUCache(2) // capacity: 2

cache.put(1, "a")
cache.put(2, "b")
console.log(cache.get(1)) // "a"  — 1 is now most recently used

cache.put(3, "c")         // evicts key 2 (least recently used)
console.log(cache.get(2)) // -1  — evicted
console.log(cache.get(3)) // "c"
```

**Hint:** Use a `Map` (which maintains insertion order) to track usage order efficiently.

---

## 💡 Bonus Challenge

### 21. JSON Path Evaluator

Write a function `getByPath(obj, path)` that extracts a deeply nested value from an object using a **string path**.

```js
const data = {
  company: {
    employees: [
      { name: "Anubhav", role: "dev" },
      { name: "Rahul",   role: "design" }
    ]
  }
}

getByPath(data, "company.employees.0.name")  // "Anubhav"
getByPath(data, "company.employees.1.role")  // "design"
getByPath(data, "company.ceo.name")          // undefined
```

---

> ✅ **Tip:** For Q15–Q21, write test cases of your own before coding the solution. Real-world JS engineering requires thinking about edge cases first.
