# 🚀 JavaScript Advanced Practice Task

---

### 📌 Instructions

- Create a single file named `practice_advanced.js`.
- Solve all questions in the same file.
- Add comments before every question.
- Run your code and verify the output.

---

# Part 1: Functions & Scope (Advanced)

### Q1. Create a function that returns the factorial of a number using recursion.

```js
factorial(5);

// Output: 120
```

---

### Q2. Create a closure that keeps track of how many times a function has been called.

```js
const counter = createCounter();
counter(); // 1
counter(); // 2
counter(); // 3
```

---

### Q3. Create a function using default parameters that greets a user.

```js
greet("Ayaan");      // Output: Hello, Ayaan! Welcome.
greet();             // Output: Hello, Guest! Welcome.
```

---

### Q4. Create an arrow function that takes a number and returns its cube.

```js
cube(3);   // Output: 27
cube(4);   // Output: 64
```

---

### Q5. Write a function that accepts any number of arguments and returns their sum (use rest parameters).

```js
sumAll(1, 2, 3);         // Output: 6
sumAll(10, 20, 30, 40);  // Output: 100
```

---

# Part 2: Higher-Order Array Methods

### Q6. Use `.map()` to return a new array with each number doubled.

```js
// Input:  [1, 2, 3, 4, 5]
// Output: [2, 4, 6, 8, 10]
```

---

### Q7. Use `.filter()` to return only numbers greater than 10.

```js
// Input:  [5, 12, 3, 18, 7, 22]
// Output: [12, 18, 22]
```

---

### Q8. Use `.reduce()` to find the product of all elements in an array.

```js
// Input:  [1, 2, 3, 4, 5]
// Output: 120
```

---

### Q9. Use `.find()` to return the first number divisible by 7.

```js
// Input:  [3, 5, 14, 21, 9]
// Output: 14
```

---

### Q10. Chain `.filter()` and `.map()` to get squares of even numbers only.

```js
// Input:  [1, 2, 3, 4, 5, 6]
// Output: [4, 16, 36]
```

---

# Part 3: Objects & Destructuring

### Q11. Create an object for a student and print their details using destructuring.

```js
const student = { name: "Riya", age: 20, grade: "A" };

// Output:
// Name: Riya
// Age: 20
// Grade: A
```

---

### Q12. Write a function that merges two objects using the spread operator.

```js
mergeObjects({ a: 1, b: 2 }, { c: 3, d: 4 });

// Output: { a: 1, b: 2, c: 3, d: 4 }
```

---

### Q13. Write a function that takes an object and returns an array of its keys and values.

```js
getKeysValues({ x: 10, y: 20, z: 30 });

// Output:
// Keys:   ["x", "y", "z"]
// Values: [10, 20, 30]
```

---

### Q14. Create an array of student objects and sort them by marks in descending order.

```js
const students = [
  { name: "Ayaan", marks: 78 },
  { name: "Riya",  marks: 92 },
  { name: "Dev",   marks: 85 },
];

// Output:
// Riya  – 92
// Dev   – 85
// Ayaan – 78
```

---

### Q15. Write a function that counts how many times each character appears in a string (return as object).

```js
charFrequency("banana");

// Output: { b: 1, a: 3, n: 2 }
```

---

# Part 4: Advanced Strings & Numbers

### Q16. Write a function to check if a number is prime.

```js
isPrime(7);   // Output: true
isPrime(10);  // Output: false
```

---

### Q17. Write a function to generate the Fibonacci sequence up to `n` terms.

```js
fibonacci(7);

// Output: [0, 1, 1, 2, 3, 5, 8]
```

---

### Q18. Write a function that flattens a nested array (one level deep without using `.flat()`).

```js
flattenArray([1, [2, 3], [4, 5], 6]);

// Output: [1, 2, 3, 4, 5, 6]
```

---

### Q19. Write a function that finds all pairs in an array that sum to a target.

```js
findPairs([1, 2, 3, 4, 5, 6], 7);

// Output:
// [1, 6]
// [2, 5]
// [3, 4]
```

---

### Q20. Write a function that rotates an array to the left by `k` positions.

```js
rotateLeft([1, 2, 3, 4, 5], 2);

// Output: [3, 4, 5, 1, 2]
```

---

# Part 5: ES6+ Concepts

### Q21. Use a `Map` to store and retrieve student names with their roll numbers.

```js
// Store: Roll 101 → "Ayaan", Roll 102 → "Riya"
// Get roll 101 → Output: Ayaan
```

---

### Q22. Use a `Set` to find unique words from a sentence.

```js
uniqueWords("the cat sat on the mat and the cat");

// Output: Set { 'the', 'cat', 'sat', 'on', 'mat', 'and' }
```

---

### Q23. Use a `Symbol` to create a unique key on an object and log it.

```js
// Create a symbol ID and attach it to an object
// Output: [Symbol(id)]: 42
```

---

### Q24. Use optional chaining (`?.`) to safely access a deeply nested property.

```js
const user = { profile: { address: { city: "Vadodara" } } };

// Output: Vadodara

const emptyUser = {};
// Output: undefined  (no error)
```

---

### Q25. Use nullish coalescing (`??`) to set a default value.

```js
const score = null;
const display = score ?? "No score yet";

// Output: No score yet
```

---

# Part 6: Async JavaScript

### Q26. Create a Promise that resolves after 2 seconds and logs a message.

```js
waitAndLog();

// (after 2 seconds)
// Output: Done waiting!
```

---

### Q27. Create a Promise that rejects and handle the error using `.catch()`.

```js
riskyTask();

// Output: Error caught: Something went wrong!
```

---

### Q28. Use `async/await` to fetch and log a joke from a public API.

```js
// API: https://official-joke-api.appspot.com/random_joke
// Output: Setup: ... Punchline: ...
```

---

### Q29. Use `Promise.all()` to run two async tasks in parallel and log both results.

```js
// Task 1 resolves "Hello" after 1s
// Task 2 resolves "World" after 2s
// Output: ["Hello", "World"]
```

---

### Q30. Write an `async` function that retries a failing task up to 3 times before giving up.

```js
// Output:
// Attempt 1 failed...
// Attempt 2 failed...
// Attempt 3 failed...
// All retries exhausted.
```

---

# 🎯 Bonus Projects (For Fast Learners)

---

### Bonus 1 – Mini ATM Machine

```js
// Features:
// - Initial balance: ₹5000
// - deposit(amount)   → adds to balance
// - withdraw(amount)  → deducts if sufficient funds
// - getBalance()      → returns current balance

deposit(2000);     // Balance: ₹7000
withdraw(3000);    // Balance: ₹4000
withdraw(9999);    // Output: Insufficient funds
getBalance();      // ₹4000
```

---

### Bonus 2 – Word Frequency Analyzer

```js
// Input: "the quick brown fox jumps over the lazy dog the fox"
// Output (sorted by frequency):
// the  → 3
// fox  → 2
// quick → 1
// ...
```

---

### Bonus 3 – Student Grade Calculator (OOP Style)

```js
class Student {
  // Properties: name, marks (array)
  // Methods:
  //   getTotal()    → sum of marks
  //   getAverage()  → average
  //   getGrade()    → A / B / C / D / F
  //   getReport()   → prints full report
}

const s = new Student("Ayaan", [88, 76, 92, 65, 80]);
s.getReport();

// Output:
// Student: Ayaan
// Total:   401
// Average: 80.2
// Grade:   A
```

---

### Bonus 4 – Debounce Function

```js
// Implement a debounce function that delays execution
// until after `delay` ms have passed since the last call.

const debouncedLog = debounce((msg) => console.log(msg), 300);

debouncedLog("typing...");
debouncedLog("typing...");
debouncedLog("done");

// Output (after 300ms of silence):
// done
```

---

### Bonus 5 – Deep Clone an Object (without JSON tricks)

```js
deepClone({ a: 1, b: { c: 2, d: [3, 4] } });

// Output: { a: 1, b: { c: 2, d: [ 3, 4 ] } }
// Must be a fully independent copy — changing clone should NOT affect original.
```
