# 🦧 JavaScript Advanced Concepts — Coding Problems

Problems covering `this`, `call/apply/bind`, `prototypes`, and `ES6 classes`.

---

## 1️⃣ The `this` Keyword

### Problem 1 — Global vs Function `this`

Create a function `showThis()` and print the value of `this` when called normally and in strict mode.

**Concepts:** Default Binding, Strict Mode Behavior

```js
function showThis() {
  console.log(this);
}

"use strict";
function showThisStrict() {
  // What does this print?
  console.log(this);
}
```

---

### Problem 2 — Object Method Context

Add a `greet` method to the `user` object. Then store it in a variable and call it — observe what changes.

**Concepts:** Method Binding, Call Site

```js
const user = { name: "Anubhav" };

user.greet = function () {
  console.log("Hello " + this.name);
};

// Now do:
const fn = user.greet;
fn(); // What prints here?
```

---

### Problem 3 — Arrow vs Regular Method

Create an object with `name: "Rahul"`. Add one regular method and one arrow method, both printing `this.name`. Compare the output.

**Concepts:** Lexical `this`, Arrow Functions

```js
const obj = {
  name: "Rahul",
  regular: function () {
    console.log(this.name);
  },
  arrow: () => {
    console.log(this.name); // Lexical this!
  },
};
```

---

### Problem 4 — Nested Callback Problem

Print `"Rahul likes X"` for each hobby using `forEach`. Preserve `this` inside the callback without using a `self`/`that` variable.

**Concepts:** Arrow Functions, Preserving `this`

```js
const person = {
  name: "Rahul",
  hobbies: ["Coding", "Gaming", "Reading"],
  printHobbies() {
    this.hobbies.forEach((h) => {
      console.log(this.name + " likes " + h);
    });
  },
};
```

**Expected output:**
```
Rahul likes Coding
Rahul likes Gaming
Rahul likes Reading
```

---

### Problem 5 — Event Handler Simulation

Simulate a button object with a regular and an arrow handler. Compare what `this` refers to in each.

**Concepts:** Event Context, Lexical Scope

```js
const button = {
  id: "btn1",
  regular: function () { console.log(this); },
  arrow: () => { console.log(this); },
};

button.regular();
button.arrow();
```

---

## 2️⃣ `call()`, `apply()`, `bind()`

### Problem 6 — Borrow a Method with `call()`

Write `introduce()` separately and use `call()` to run it for each person object.

**Concepts:** `call()`, Method Borrowing

```js
const person1 = { name: "Anubhav" };
const person2 = { name: "Rahul" };

function introduce() {
  console.log("Hi, I am " + this.name);
}

introduce.call(person1);
introduce.call(person2);
```

**Expected output:**
```
Hi, I am Anubhav
Hi, I am Rahul
```

---

### Problem 7 — `apply()` with Array Arguments

Create `introduce(city, country)`. Use `apply()` to pass arguments as an array.

**Concepts:** `apply()`, Spread Args

```js
function introduce(city, country) {
  console.log(`I am ${this.name} from ${city}, ${country}`);
}

const person = { name: "Rahul" };
const args = ["Indore", "India"];

introduce.apply(person, args);
```

**Expected output:**
```
I am Rahul from Indore, India
```

---

### Problem 8 — `bind()` for Delayed Execution

Use `bind()` to permanently attach a person's context to a `greet` function, then call it inside `setTimeout` after 2 seconds.

**Concepts:** Permanent Binding, Callback Context

```js
const user = { name: "Anubhav" };

function greet() {
  console.log("Hello, " + this.name);
}

const boundGreet = greet.bind(user);
setTimeout(boundGreet, 2000);
```

---

### Problem 9 — Custom Calculator

Object with `value: 100`. Write `addTo(n)` that adds `n` to `this.value`. Execute it using `call()`, `apply()`, and `bind()` separately.

**Concepts:** `call()`, `apply()`, `bind()`

```js
const calc = { value: 100 };

function addTo(n) {
  console.log(this.value + n);
}

addTo.call(calc, 50);
addTo.apply(calc, [50]);
const bound = addTo.bind(calc, 50);
bound();
```

---

## 3️⃣ Prototypes

### Problem 10 — Prototype Lookup

Create a `person` object. Check if `hasOwnProperty` is on the object itself or the prototype chain.

**Concepts:** Prototype Chain, `hasOwnProperty`

```js
const person = { name: "Rahul" };

console.log(person.hasOwnProperty("name"));          // true
console.log(person.hasOwnProperty("hasOwnProperty")); // false — it's on the prototype!
```

---

### Problem 11 — Custom `Array.prototype` Method

Add a `sum()` method to `Array.prototype`. `[1,2,3,4].sum()` should return `10`.

**Concepts:** Prototype Extension, `Array.prototype`

```js
Array.prototype.sum = function () {
  return this.reduce((acc, n) => acc + n, 0);
};

console.log([1, 2, 3, 4].sum()); // 10
```

---

### Problem 12 — `Object.create()` Inheritance

Create an `animal` object with `eat()` and `sleep()`. Create a `dog` using `Object.create(animal)` and call its inherited methods.

**Concepts:** `Object.create()`, Prototypal Inheritance

```js
const animal = {
  eat()   { console.log(this.name + " is eating"); },
  sleep() { console.log(this.name + " is sleeping"); },
};

const dog = Object.create(animal);
dog.name = "Bruno";
dog.eat();
dog.sleep();
```

---

### Problem 13 — Prototype Inheritance: Vehicles

Create a `vehicle` prototype with `start()` and `stop()`. Create `car`, `bike`, `truck` using `Object.create()` and verify inheritance.

**Concepts:** Prototype Chain, Delegation

```js
const vehicle = {
  start() { console.log(this.type + " started"); },
  stop()  { console.log(this.type + " stopped"); },
};

const car   = Object.create(vehicle); car.type   = "Car";
const bike  = Object.create(vehicle); bike.type  = "Bike";
const truck = Object.create(vehicle); truck.type = "Truck";
```

---

### Problem 14 — Constructor Function + Prototype

Create a `Person(name, age)` constructor. Attach `greet()` to `Person.prototype`. Output: `Hi, I am Rahul`.

**Concepts:** Constructor Functions, Prototype Methods

```js
function Person(name, age) {
  this.name = name;
  this.age  = age;
}

Person.prototype.greet = function () {
  console.log("Hi, I am " + this.name);
};

const p = new Person("Rahul", 25);
p.greet();
```

---

### Problem 15 — Prototype Chain Investigation

Create `const arr = []`. Log three levels of the prototype chain and explain each.

**Concepts:** Prototype Chain, Null Termination

```js
const arr = [];
console.log(arr.__proto__);                       // Array.prototype
console.log(arr.__proto__.__proto__);             // Object.prototype
console.log(arr.__proto__.__proto__.__proto__);   // null — end of chain
```

---

## 4️⃣ ES6 Classes

### Problem 16 — Basic Class: Student

Create a `Student` class with `name` and `course`. Add `introduce()` that prints `"I am X and I study Y"`.

**Concepts:** Class Syntax, Constructor, Methods

```js
class Student {
  constructor(name, course) {
    this.name   = name;
    this.course = course;
  }
  introduce() {
    console.log(`I am ${this.name} and I study ${this.course}`);
  }
}

const s = new Student("Anubhav", "MERN Stack");
s.introduce();
```

**Expected output:**
```
I am Anubhav and I study MERN Stack
```

---

### Problem 17 — Employee Management

Build an `Employee` class with `name` and `salary`. Add `increaseSalary(amount)` and `showSalary()`.

**Concepts:** Classes, Instance Methods

```js
class Employee {
  constructor(name, salary) {
    this.name   = name;
    this.salary = salary;
  }
  increaseSalary(amount) { this.salary += amount; }
  showSalary() { console.log(`${this.name}: ₹${this.salary}`); }
}
```

---

### Problem 18 — Bank Account System

Create `BankAccount` with `deposit()`, `withdraw()`, and `checkBalance()`. Reject withdrawals exceeding balance.

**Concepts:** Classes, Guard Clauses

```js
class BankAccount {
  constructor(owner) {
    this.owner   = owner;
    this.balance = 0;
  }
  deposit(n)  { this.balance += n; }
  withdraw(n) {
    if (n > this.balance) return console.log("Insufficient funds");
    this.balance -= n;
  }
  checkBalance() { console.log("Balance:", this.balance); }
}
```

---

### Problem 19 — Inheritance: Animal → Dog

Create `Animal` with `eat()`. Extend it in `Dog`, adding `bark()`. Use `super()` in the constructor.

**Concepts:** `extends`, `super()`, Method Override

```js
class Animal {
  constructor(name) { this.name = name; }
  eat() { console.log(this.name + " is eating"); }
}

class Dog extends Animal {
  bark() { console.log(this.name + " says Woof!"); }
}

const d = new Dog("Rex");
d.eat();
d.bark();
```

---

### Problem 20 — Multi-Level Inheritance

Build `Person → Employee → Manager` with unique properties at each level. `Manager` adds a `team` array and `addMember()`.

**Concepts:** Multi-level `extends`, `super`, Chain

```js
class Person {
  constructor(name) { this.name = name; }
}

class Employee extends Person {
  constructor(name, dept) {
    super(name);
    this.dept = dept;
  }
}

class Manager extends Employee {
  constructor(name, dept) {
    super(name, dept);
    this.team = [];
  }
  addMember(m) { this.team.push(m); }
}
```

---

## 5️⃣ Static Methods

### Problem 21 — MathHelper Utility Class

Create `MathHelper` with static `add()`, `subtract()`, `multiply()`, `divide()`. Call without instantiating.

**Concepts:** Static Methods, Utility Class

```js
class MathHelper {
  static add(a, b)      { return a + b; }
  static subtract(a, b) { return a - b; }
  static multiply(a, b) { return a * b; }
  static divide(a, b)   { return a / b; }
}

console.log(MathHelper.add(5, 3)); // 8
```

---

### Problem 22 — User Counter

Use a static property to count how many `User` instances have been created.

**Concepts:** Static Property, Instance Counting

```js
class User {
  static count = 0;
  constructor(name) {
    this.name = name;
    User.count++;
  }
  static showCount() {
    console.log("Total Users:", User.count);
  }
}
```

**Expected output:**
```
Total Users: 5
```

---

## 6️⃣ Getters & Setters

### Problem 23 — Full Name Getter

Create `Person` with `firstName` and `lastName`. Add a `fullName` getter that combines them.

**Concepts:** Getter, Computed Property

```js
class Person {
  constructor(first, last) {
    this.firstName = first;
    this.lastName  = last;
  }
  get fullName() {
    return this.firstName + " " + this.lastName;
  }
}

const p = new Person("Anubhav", "Sharma");
console.log(p.fullName);
```

---

### Problem 24 — Email Validation Setter

Add an `email` setter that rejects values without `@`. Print `"Invalid email"` on rejection.

**Concepts:** Setter, Validation

```js
class Person {
  set email(val) {
    if (!val.includes("@")) {
      console.log("Invalid email");
      return;
    }
    this._email = val;
  }
  get email() { return this._email; }
}
```

---

## 7️⃣ Private Fields

### Problem 25 — Secure Bank Account

Use `#balance` as a private field. Expose `deposit()`, `withdraw()`, `getBalance()`. Direct access to `#balance` should throw.

**Concepts:** Private Fields, Encapsulation

```js
class BankAccount {
  #balance = 0;

  deposit(n)  { this.#balance += n; }
  withdraw(n) {
    if (n > this.#balance) return console.log("Insufficient");
    this.#balance -= n;
  }
  getBalance() { return this.#balance; }
}

const acc = new BankAccount();
acc.deposit(500);
console.log(acc.getBalance()); // 500
// acc.#balance → SyntaxError
```

---

### Problem 26 — Student Grades System

Store marks in `#marks` (private). Add `setMarks()` and `getMarks()` as the only access points.

**Concepts:** Private Fields, Data Hiding

```js
class Student {
  #marks = [];

  setMarks(arr) { this.#marks = arr; }
  getMarks()    { return this.#marks; }
  average() {
    const sum = this.#marks.reduce((a, b) => a + b, 0);
    return sum / this.#marks.length;
  }
}
```

---
