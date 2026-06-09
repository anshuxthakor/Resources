# Phase 4

## 1. The `this` Keyword

> **GOLDEN RULE:** The value of `this` depends on **how a function is called**, not **where it is written**. (Arrow functions are the only exception.)
> 
> 
> The same function can have different `this` values depending on how it is invoked. That’s why developers often get confused. Always focus on the **call-site** — *how the function was called*.

### 1.1 — `this` in Global Scope (Browser vs Node.js)

**In a browser**, at the top level, `this` refers to the entire `window` object:

```jsx
console.log(this); // window
```

**In Node.js**, inside a `.js` file, every file is treated as a module, so top-level `this` is an empty object:

```jsx
console.log(this); // {} (module.exports)
```

> Note: In the Node.js REPL (`node` in terminal), `this` refers to `global`, but inside a `.js` file it refers to `module.exports`.
> 

### 1.2 — `this` in Regular Functions

```jsx
function show() {
  console.log(this);
}
show();
```

- **Browser (non-strict mode):** `this = window`
- **Node.js (non-strict mode):** `this = global`
- **Strict mode:** `this = undefined`

**Why?** When a function is called without an object (`show()`), JavaScript uses **default binding** and assigns the global object.

### 1.3 — `this` in Methods

When a function is called through an object:

```jsx
const user = {
  name: "Rahul",
  greet() {
    console.log("Hi, " + this.name);
  }
};

user.greet(); // Hi, Rahul
```

> **Trick:** Look at the object on the **left side of the dot**. That object becomes `this`.
> 

`user.greet()` → left side is `user` → `this = user`

**Important Example:**

```jsx
const fn = user.greet;
fn(); // undefined
```

The function is the same, but now it's called without an object. There is no object on the left side of a dot, so `this` falls back to the default binding.

This proves that `this` depends on **how the function is called**, not where it was defined.

### 1.4 — `this` in Arrow Functions (Lexical `this`)

> Arrow functions do **not** have their own `this`. They borrow `this` from the surrounding scope. This is called **lexical this**.
> 

```jsx
const user = {
  name: "Rahul",
  greet: () => {
    console.log(this.name);
  }
};

user.greet(); // undefined
```

The arrow function does not get `this` from `user`.

However, arrow functions are extremely useful inside callbacks:

```jsx
const user = {
  name: "Rahul",
  hobbies: ["coding", "gaming"],

  show() {
    this.hobbies.forEach((hobby) => {
      console.log(this.name + " likes " + hobby);
    });
  }
};

user.show();
```

Output:

```jsx
Rahul likes coding
Rahul likes gaming
```

> Rule of thumb:
> 
> - Use **regular functions** for object methods.
> - Use **arrow functions** for nested callbacks inside those methods.

### 1.5 — `this` in Event Handlers

**Regular Function:**

```jsx
button.addEventListener("click", function () {
  console.log(this);
});
```

`this` refers to the button element that triggered the event.

**Arrow Function:**

```jsx
button.addEventListener("click", () => {
  console.log(this);
});
```

`this` comes from the outer scope, not the button element.

> In event handlers, use a regular function or use `event.target`.
> 

### 1.6 — `this` in Strict Mode

```jsx
"use strict";

function show() {
  console.log(this);
}

show();
```

Output:

```jsx
undefined
```

Without strict mode, JavaScript would use the global object. Strict mode prevents that behavior and helps avoid accidental global modifications.

---

### Quick Summary — `this` Cheatsheet

| Function Call Type | `this` Value |
| --- | --- |
| Global scope (browser) | `window` |
| Global scope (Node module) | `{}` (`module.exports`) |
| Regular function `fn()` | Global object (or `undefined` in strict mode) |
| Method `obj.fn()` | `obj` |
| Arrow function | Parent scope's `this` |
| Event handler (regular function) | Event element |
| Event handler (arrow function) | Lexical `this` |

---

## 2. `call`, `apply`, and `bind` — Explicit Binding

Sometimes we want to decide what `this` should be.

```jsx
function introduce(city, country) {
  console.log(`I am ${this.name} from ${city}, ${country}`);
}

const person = { name: "Rahul" };
```

### `call()` — Call Immediately, Arguments Separately

```jsx
introduce.call(person, "Indore", "India");
```

Output:

```jsx
I am Rahul from Indore, India
```

### `apply()` — Call Immediately, Arguments as an Array

```jsx
introduce.apply(person, ["Indore", "India"]);
```

Output:

```jsx
I am Rahul from Indore, India
```

> Easy memory trick:
> 
> - **Apply → Array**
> - **Call → Comma-separated arguments**

### `bind()` — Create a New Function

```jsx
const boundFn = introduce.bind(person);

boundFn("Indore", "India");
```

Output:

```jsx
I am Rahul from Indore, India
```

`bind()` does not execute the function immediately. It returns a new function whose `this` is permanently fixed.

> Common use case: callbacks and event handlers where `this` might be lost.
> 

| Method | Executes Immediately? | Arguments |
| --- | --- | --- |
| `call()` | Yes | Comma-separated |
| `apply()` | Yes | Array |
| `bind()` | No | Returns a new function |

---

## 3. Prototypes

### 3.1 — What is a Prototype?

> Every JavaScript object contains a hidden link pointing to another object. That linked object is called its **prototype**.
> 

If JavaScript cannot find a property on an object, it looks in its prototype, then the prototype’s prototype, and so on. This is called the **prototype chain**.

### 3.2 — `__proto__` vs `prototype`

| `__proto__` | `prototype` |
| --- | --- |
| Exists on objects | Exists on functions |
| Points to the object's prototype | Blueprint for future instances |
| Used for lookup | Used when creating objects with `new` |

Example:

```jsx
function Person(name) {
  this.name = name;
}

const p = new Person("Rahul");

console.log(p.__proto__ === Person.prototype);
```

Output:

```jsx
true
```

### 3.3 — Prototype Chain

```jsx
const arr = [1, 2, 3];

arr.push(4);
```

Where does `push()` come from?

JavaScript searches:

```
arr
 ↓
Array.prototype
 ↓
Object.prototype
 ↓
null
```

It finds `push()` inside `Array.prototype`.

### 3.4 — `Object.create()`

```jsx
const animal = {
  eats: true,
  walk() {
    console.log("Animal is walking");
  }
};

const dog = Object.create(animal);

dog.barks = true;

console.log(dog.eats);
dog.walk();
```

Output:

```jsx
true
Animal is walking
```

### 3.5 — Prototype-Based Inheritance

```jsx
const vehicle = {
  start() {
    console.log(this.name + " started");
  }
};

const car = Object.create(vehicle);

car.name = "Swift";

car.start();
```

Output:

```jsx
Swift started
```

---

## 4. ES6 Classes

> When `new MyClass()` is executed, JavaScript:
> 
> 1. Creates a new empty object.
> 2. Sets its prototype.
> 3. Runs the constructor with `this` pointing to the new object.
> 4. Returns the object.

### 4.1 — Class Syntax and Constructor

```jsx
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    console.log("Hi, I am " + this.name);
  }
}

const p = new Person("Rahul", 25);

p.greet();
```

Output:

```jsx
Hi, I am Rahul
```

### 4.2 — Inheritance with `extends` and `super`

```jsx
class Animal {
  constructor(name) {
    this.name = name;
  }

  eat() {
    console.log(this.name + " is eating");
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }

  bark() {
    console.log(this.name + " is barking");
  }
}

const d = new Dog("Tommy", "Labrador");

d.eat();
d.bark();
```

Output:

```jsx
Tommy is eating
Tommy is barking
```

### 4.3 — Static Methods and Properties

```jsx
class MathHelper {
  static PI = 3.14159;

  static add(a, b) {
    return a + b;
  }
}

console.log(MathHelper.add(2, 3));
console.log(MathHelper.PI);
```

Output:

```jsx
5
3.14159
```

Static members belong to the class itself, not to instances.

### 4.4 — Getters and Setters

```jsx
class Person {
  constructor(first, last) {
    this.first = first;
    this.last = last;
  }

  get fullName() {
    return this.first + " " + this.last;
  }

  set fullName(value) {
    const parts = value.split(" ");
    this.first = parts[0];
    this.last = parts[1];
  }
}
```

Usage:

```jsx
const p = new Person("Rahul", "Sharma");

console.log(p.fullName);

p.fullName = "Priya Verma";

console.log(p.first);
```

Output:

```jsx
Rahul Sharma
Priya
```

### 4.5 — Private Fields (`#`)

```jsx
class BankAccount {
  #balance = 0;

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const acc = new BankAccount();

acc.deposit(500);

console.log(acc.getBalance());
```

Output:

```jsx
500
```

Accessing `#balance` outside the class causes a syntax error.

### 4.6 — The Big Truth: Classes Are Just Syntactic Sugar ⭐⭐

Classes are not a new object model in JavaScript. Internally, they are still based on constructor functions and prototypes.

```jsx
class Person {
  constructor(name) {
    this.name = name;
  }

  greet() {
    return "Hi " + this.name;
  }
}

console.log(typeof Person);
```

Output:

```jsx
function
```

Methods are stored on the prototype:

```jsx
console.log(typeof Person.prototype.greet);
```

Output:

```jsx
function
```

The prototype link is still there:

```jsx
const p = new Person("Rahul");

console.log(p.__proto__ === Person.prototype);
```

Output:

```jsx
true
```

Equivalent constructor-function version:

```jsx
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function () {
  return "Hi " + this.name;
};
```