# 📑Phase 5

# Part 1: Synchronous vs Asynchronous JavaScript

## What is Synchronous?

JavaScript is **single-threaded** and **synchronous** by default.

### Single-Threaded

Single-threaded means JavaScript can perform only **one task at a time**. It has a single execution thread that handles all operations.

### Synchronous

Synchronous execution means code runs **line by line**, from top to bottom. The next line will not execute until the current line has finished.

```jsx
console.log("1. Make Tea");
console.log("2. Drink Tea");
console.log("3. Wash Cup");

// Output:
// 1. Make Tea
// 2. Drink Tea
// 3. Wash Cup
```

Everything executes in sequence and in the correct order.

---

## The Problem with Synchronous Code

Suppose a task takes a long time to complete, such as:

- Fetching data from a server
- Reading a file
- Waiting for a timer

In a purely synchronous environment, the entire program becomes blocked until that task finishes.

```jsx
console.log("Start");

let now = Date.now();

while (Date.now() - now < 5000) {
  // Wait for 5 seconds
}

console.log("End");
```

### Output

```jsx
Start
// Waits 5 seconds
End
```

During those 5 seconds:

- Browser freezes
- Buttons stop responding
- Scrolling stops working
- UI becomes unresponsive

This behavior is called **Blocking Execution**.

## What is Asynchronous?

Asynchronous programming allows long-running tasks to execute in the background while the rest of the program continues running.

Instead of waiting for a task to finish, JavaScript schedules it and moves on.

### Restaurant Waiter Analogy

Imagine a waiter in a restaurant:

### Synchronous Waiter ❌

1. Takes an order.
2. Stands in the kitchen until food is ready.
3. Delivers food.
4. Goes to the next customer.

Result: Everyone waits.

### Asynchronous Waiter ✅

1. Takes an order.
2. Gives it to the kitchen.
3. Immediately serves other customers.
4. Returns when the food is ready.

Result: Much more efficient.

JavaScript follows the second approach for asynchronous operations.

---

## Example: setTimeout

```jsx
console.log("Start");

setTimeout(() => {
  console.log("Executed after 3 seconds");
}, 3000);

console.log("End");
```

### Output

```jsx
Start
End
Executed after 3 seconds
```

### What Happened?

1. `"Start"` is printed.
2. `setTimeout` schedules the task for later.
3. JavaScript continues execution.
4. `"End"` is printed immediately.
5. After 3 seconds, the callback executes.

This behavior is called **Non-Blocking Execution**.

---

## Why Asynchronous Programming is Important

Asynchronous programming is commonly used for:

### 1. API Calls

```jsx
fetch("https://api.example.com/users");
```

Fetching data from a server may take several seconds.

---

### 2. Timers

```jsx
setTimeout(() => {
  console.log("Hello");
}, 1000);
```

### 3. File Operations

Reading or writing files should not block the application.

---

### 4. Database Queries

```jsx
User.find();
```

Database operations may take time and should execute asynchronously.

---

### 5. User Interactions

Waiting for:

- Button clicks
- Form submissions
- Keyboard input

should not freeze the application.

---

## Key Takeaway

| Synchronous | Asynchronous |
| --- | --- |
| Executes line by line | Allows tasks to run in the background |
| Blocking | Non-blocking |
| Next task waits | Other tasks continue |
| Can freeze the application | Keeps application responsive |
| Easier to understand initially | More efficient for real-world applications |

---

## Important Note

JavaScript itself is **single-threaded**, but asynchronous behavior is made possible through:

- Browser APIs
- Node.js APIs
- Event Loop

These components help JavaScript perform asynchronous operations without blocking the main thread.

# Part 2: Callbacks in JavaScript

Callbacks are one of the most important concepts in JavaScript because they are the foundation of asynchronous programming.

---

# What is a Callback?

A **callback** is a function that is passed as an argument to another function and is executed later.

In simple words:

> "When this task is completed, execute this function."
> 

### Example

```jsx
function greet(name) {
  console.log("Hello " + name);
}

function welcome(callback) {
  let user = "Rahul";
  callback(user);
}

welcome(greet);
```

### Output

```jsx
Hello Rahul
```

Here:

- `greet()` is the callback function.
- `welcome()` receives it and executes it later.

# Types of Callbacks

There are two major types of callbacks.

## 1. Synchronous Callback

Runs immediately during normal code execution.

### Example

```jsx
[1, 2, 3].forEach(function(num) {
  console.log(num * 2);
});
```

### Output

```jsx
2
4
6
```

The callback executes instantly for each array element.

---

## 2. Asynchronous Callback

Runs after an asynchronous task completes.

### Example

```jsx
setTimeout(() => {
  console.log("Executed later");
}, 2000);
```

### Output

```jsx
Executed later
```

(after 2 seconds)

---

# setTimeout()

`setTimeout()` executes a function **once** after a specified delay.

## Syntax

```jsx
setTimeout(callback, delay);
```

Where:

- callback → function to execute
- delay → time in milliseconds

---

## Example

```jsx
setTimeout(() => {
  console.log("2 seconds completed");
}, 2000);
```

### Output

```jsx
2 seconds completed
```

(after 2 seconds)

---

# Important Interview Point

Many developers think:

```jsx
setTimeout(fn, 2000);
```

means:

> "Execute exactly after 2 seconds."
> 

This is incorrect.

It actually means:

> "Execute after at least 2 seconds."
> 

The callback must still wait for the Call Stack to become empty.

---

## Example

```jsx
setTimeout(() => {
  console.log("Hello");
}, 0);

console.log("World");
```

### Output

```jsx
World
Hello
```

Even though delay is `0ms`, `"World"` appears first.

You'll understand why when studying the Event Loop.

---

# Passing Arguments to setTimeout

```jsx
function greet(name, city) {
  console.log(`Hello ${name} from ${city}`);
}

setTimeout(greet, 1000, "Rahul", "Indore");
```

### Output

```jsx
Hello Rahul from Indore
```

(after 1 second)

---

# setInterval()

`setInterval()` repeatedly executes a function after every specified interval.

## Syntax

```jsx
setInterval(callback, delay);
```

---

## Example

```jsx
let count = 1;

setInterval(() => {
  console.log(count);
  count++;
}, 1000);
```

### Output

```jsx
1
2
3
4
5
...
```

A new value is printed every second indefinitely.

---

# Difference Between setTimeout and setInterval

| setTimeout | setInterval |
| --- | --- |
| Executes once | Executes repeatedly |
| Stops automatically | Continues forever |
| Used for delayed execution | Used for repeated execution |

---

# clearTimeout()

`clearTimeout()` cancels a pending timeout before it executes.

---

## Example

```jsx
const timerId = setTimeout(() => {
  console.log("This will never run");
}, 3000);

clearTimeout(timerId);
```

### Output

```jsx
Nothing
```

The timeout is cancelled before execution.

---

# clearInterval()

`clearInterval()` stops an interval.

---

## Example

```jsx
let count = 1;

const intervalId = setInterval(() => {
  console.log(count);
  count++;

  if (count > 5) {
    clearInterval(intervalId);
    console.log("Interval Stopped");
  }
}, 1000);
```

### Output

```jsx
1
2
3
4
5
Interval Stopped
```

---

# Real-World Callback Example

Suppose we are fetching data from a server.

```jsx
function fetchData(callback) {
  console.log("Fetching data...");

  setTimeout(() => {
    const data = {
      id: 1,
      name: "Aman"
    };

    callback(data);
  }, 2000);
}
```

Using it:

```jsx
fetchData((result) => {
  console.log(result);
});
```

### Output

```jsx
Fetching data...

{id: 1, name: "Aman"}
```

(after 2 seconds)

---

# Error-First Callback Pattern (Node.js Standard)

In Node.js, callbacks usually follow this pattern:

```jsx
callback(error, data);
```

The first argument is always the error.

---

## Example

```jsx
function fetchData(callback) {
  setTimeout(() => {

    const error = null;

    const data = {
      id: 1,
      name: "Aman"
    };

    if (error) {
      callback(error, null);
    } else {
      callback(null, data);
    }

  }, 1000);
}
```

Usage:

```jsx
fetchData((err, data) => {

  if (err) {
    console.log(err);
    return;
  }

  console.log(data);

});
```

### Output

```jsx
{id: 1, name: "Aman"}
```

---

# What is Callback Hell?

When multiple asynchronous operations depend on each other, callbacks become nested inside callbacks.

The code starts moving towards the right side and becomes difficult to read.

This problem is called:

- Callback Hell
- Pyramid of Doom

---

## Example

```jsx
getUser(1, (user) => {

  getOrders(user.id, (orders) => {

    getOrderDetails(orders[0], (details) => {

      processPayment(details, (payment) => {

        sendConfirmation(payment, (result) => {

          console.log(result);

        });

      });

    });

  });

});
```

Notice how each callback is nested inside another callback.

The code becomes:

❌ Difficult to read

❌ Difficult to debug

❌ Difficult to maintain

❌ Difficult to handle errors

---

# Problems of Callback Hell

## 1. Poor Readability

Code becomes deeply nested.

---

## 2. Complex Error Handling

Every level needs its own error handling.

```jsx
getUser(id, (err, user) => {

  if(err) return console.log(err);

  getOrders(user.id, (err, orders) => {

    if(err) return console.log(err);

    getOrderDetails(orders[0], (err, details) => {

      if(err) return console.log(err);

    });

  });

});
```

Error handling becomes repetitive.

---

## 3. Hard Maintenance

Adding new logic becomes difficult.

---

## 4. Debugging Becomes Painful

Finding bugs inside deeply nested callbacks is challenging.

---

# Solution to Callback Hell

JavaScript introduced **Promises** to solve these problems.

Instead of:

```jsx
callback(callback(callback()));
```

we can write:

```jsx
promise
  .then()
  .then()
  .then()
  .catch();
```

which is much cleaner and easier to maintain.

# Quick Revision

| Concept | Summary |
| --- | --- |
| Callback | Function passed to another function |
| Synchronous Callback | Executes immediately |
| Asynchronous Callback | Executes later |
| setTimeout | Runs once after delay |
| setInterval | Runs repeatedly after interval |
| clearTimeout | Cancels timeout |
| clearInterval | Stops interval |
| Error-First Callback | Node.js standard callback pattern |
| Callback Hell | Deeply nested callbacks |
| Pyramid of Doom | Another name for Callback Hell |
| Solution | Promises |

---

### Practice Questions

1. Create a countdown from 10 to 0 using `setInterval()`.
2. Stop the countdown automatically using `clearInterval()`.
3. Create a callback function that greets a user.
4. Implement a fake API call using `setTimeout()`.
5. Convert a callback hell example into Promises.

# Part 3: Promises in JavaScript 🤝

Promises were introduced to solve the problems of **Callback Hell** and make asynchronous code easier to read and maintain.

---

# What is a Promise?

A **Promise** is an object that represents the eventual result (success or failure) of an asynchronous operation.

Think of it as:

> "I promise that I will give you a result in the future."
> 

---

## Real-Life Example

Suppose you order food online.

Immediately after ordering, you receive a confirmation message.

At this moment:

- Food is not delivered yet.
- Food is not cancelled yet.

The order is still being processed.

This is exactly how a Promise works.

---

# Promise States

A Promise can exist in only one of three states.

| State | Meaning |
| --- | --- |
| Pending | Operation is still running |
| Fulfilled | Operation completed successfully |
| Rejected | Operation failed |

---

### Promise Lifecycle

```
Pending
   |
   | resolve()
   ▼
Fulfilled

OR

Pending
   |
   | reject()
   ▼
Rejected
```

---

## Important Rule

A Promise can be settled only once.

Once it becomes:

- Fulfilled
- Rejected

It can never change again.

---

# Creating a Promise

Syntax:

```jsx
const promise = new Promise((resolve, reject) => {
  // Async work
});
```

---

## Example

```jsx
const myPromise = new Promise((resolve, reject) => {

  const success = true;

  if(success) {
    resolve("Task Completed");
  } else {
    reject("Task Failed");
  }

});
```

---

### Output

```jsx
Promise { fulfilled: "Task Completed" }
```

---

# Understanding resolve()

Used when the operation succeeds.

```jsx
resolve(data);
```

Example:

```jsx
const promise = new Promise((resolve) => {
  resolve("Data Received");
});
```

---

# Understanding reject()

Used when the operation fails.

```jsx
reject(error);
```

Example:

```jsx
const promise = new Promise((resolve, reject) => {
  reject("Server Error");
});
```

---

# Real Async Example

```jsx
function fetchData() {

  return new Promise((resolve, reject) => {

    console.log("Fetching Data...");

    setTimeout(() => {

      const success = true;

      if(success) {
        resolve({
          id: 1,
          name: "Aman"
        });
      } else {
        reject("Server Down");
      }

    }, 2000);

  });

}
```

---

# Consuming a Promise

To use the result of a Promise, we use:

- `.then()`
- `.catch()`
- `.finally()`

---

# then()

Runs when the Promise is fulfilled.

```jsx
promise.then((result) => {
  console.log(result);
});
```

---

## Example

```jsx
fetchData()
  .then((data) => {
    console.log(data);
  });
```

### Output

```jsx
Fetching Data...

{ id: 1, name: "Aman" }
```

---

# catch()

Runs when the Promise is rejected.

```jsx
promise.catch((error) => {
  console.log(error);
});
```

---

## Example

```jsx
fetchData()
  .catch((error) => {
    console.log(error);
  });
```

### Output

```jsx
Server Down
```

---

# finally()

Runs regardless of success or failure.

Useful for:

- Stopping loaders
- Closing connections
- Cleanup tasks

---

## Example

```jsx
fetchData()
  .then((data) => {
    console.log(data);
  })
  .catch((err) => {
    console.log(err);
  })
  .finally(() => {
    console.log("Task Finished");
  });
```

### Output

```jsx
Task Finished
```

always executes.

---

# Promise Chaining

One of the most powerful features of Promises.

Each `.then()` returns a new Promise.

Therefore we can chain them together.

---

## Example

```jsx
function add(num) {

  return new Promise((resolve) => {

    setTimeout(() => {
      resolve(num + 10);
    }, 1000);

  });

}
```

Using Chaining:

```jsx
add(0)

.then((result) => {
  console.log(result);
  return add(result);
})

.then((result) => {
  console.log(result);
  return add(result);
})

.then((result) => {
  console.log(result);
});
```

### Output

```jsx
10
20
30
```

---

# Callback Hell vs Promise Chaining

### Callback Hell

```jsx
getUser(1, (user) => {

  getOrders(user.id, (orders) => {

    getDetails(orders[0], (details) => {

      processPayment(details, (payment) => {

        sendConfirmation(payment);

      });

    });

  });

});
```

---

### Promise Chaining

```jsx
getUser(1)

.then((user) => {
  return getOrders(user.id);
})

.then((orders) => {
  return getDetails(orders[0]);
})

.then((details) => {
  return processPayment(details);
})

.then((payment) => {
  return sendConfirmation(payment);
})

.then((result) => {
  console.log(result);
})

.catch((error) => {
  console.log(error);
});
```

Much cleaner and easier to maintain.

---

# Error Propagation

The biggest advantage of Promises:

A single `.catch()` can handle errors from the entire chain.

---

## Example

```jsx
add(0)

.then((result) => {

  console.log(result);

  throw new Error("Something Went Wrong");

})

.then((result) => {

  console.log(result);

})

.catch((error) => {

  console.log(error.message);

});
```

### Output

```jsx
10
Something Went Wrong
```

All remaining `.then()` blocks are skipped.

# Promise Combinators

JavaScript provides methods for handling multiple Promises together.

---

# 1. Promise.all()

Waits until all Promises succeed.

If one fails → entire Promise fails.

---

## Example

```jsx
const p1 = Promise.resolve("Tea");
const p2 = Promise.resolve("Coffee");
const p3 = Promise.resolve("Juice");

Promise.all([p1, p2, p3])

.then((results) => {
  console.log(results);
});
```

### Output

```jsx
[
  "Tea",
  "Coffee",
  "Juice"
]
```

---

## If One Fails

```jsx
const p1 = Promise.resolve("Tea");
const p2 = Promise.reject("Coffee Failed");
const p3 = Promise.resolve("Juice");

Promise.all([p1, p2, p3])

.catch((error) => {
  console.log(error);
});
```

### Output

```jsx
Coffee Failed
```

---

# Use Cases

- Loading multiple APIs
- Dashboard data
- User profile + posts + notifications

All must succeed.

---

# 2. Promise.race()

Returns whichever Promise settles first.

Success or failure doesn't matter.

---

## Example

```jsx
const fast = new Promise((resolve) => {
  setTimeout(() => resolve("Fast"), 1000);
});

const slow = new Promise((resolve) => {
  setTimeout(() => resolve("Slow"), 3000);
});

Promise.race([fast, slow])

.then((result) => {
  console.log(result);
});
```

### Output

```jsx
Fast
```

---

# Use Cases

- Timeouts
- Fastest server selection
- Mirror APIs

---

# 3. Promise.allSettled()

Waits for every Promise to complete.

Does not fail if one rejects.

Returns status of each Promise.

---

## Example

```jsx
const p1 = Promise.resolve("Success");
const p2 = Promise.reject("Failed");
const p3 = Promise.resolve("Completed");

Promise.allSettled([p1, p2, p3])

.then((results) => {
  console.log(results);
});
```

### Output

```jsx
[
  {
    status: "fulfilled",
    value: "Success"
  },
  {
    status: "rejected",
    reason: "Failed"
  },
  {
    status: "fulfilled",
    value: "Completed"
  }
]
```

---

# Use Cases

- Bulk operations
- Multiple uploads
- Loading multiple user profiles

You want results even if some fail.

---

# 4. Promise.any()

Returns the first successful Promise.

Ignores failures.

---

## Example

```jsx
const p1 = Promise.reject("Failed 1");

const p2 = new Promise((resolve) => {
  setTimeout(() => {
    resolve("Success");
  }, 1000);
});

const p3 = Promise.reject("Failed 3");

Promise.any([p1, p2, p3])

.then((result) => {
  console.log(result);
});
```

### Output

```jsx
Success
```

---

## If All Fail

```jsx
Promise.any([
  Promise.reject("Error 1"),
  Promise.reject("Error 2")
]);
```

### Output

```jsx
AggregateError
```

---

# Promise.race() vs Promise.any()

| Promise.race() | Promise.any() |
| --- | --- |
| First settled Promise wins | First successful Promise wins |
| Success or failure | Success only |
| Can reject immediately | Ignores rejected Promises |

---

# Quick Revision

| Method | Success Condition | Failure Condition |
| --- | --- | --- |
| Promise.all() | All succeed | One fails |
| Promise.race() | First settles | First failure can reject |
| Promise.allSettled() | Always succeeds | Never rejects |
| Promise.any() | First success | All fail |

---

# Interview Questions

### Q1: What are the three states of a Promise?

**Answer:**

- Pending
- Fulfilled
- Rejected

---

### Q2: Difference between resolve() and reject()?

**Answer:**

- resolve() → Success
- reject() → Failure

---

### Q3: Difference between Promise.all() and Promise.allSettled()?

**Answer:**

- Promise.all() fails if one Promise fails.
- Promise.allSettled() returns results for all Promises.

---

### Q4: Why were Promises introduced?

**Answer:**

To solve Callback Hell and improve code readability.

---

### Q5: Can a Promise change state after fulfillment?

**Answer:**

No. Once settled, a Promise cannot change its state.

---

# Part 4: Async / Await in JavaScript ✨

`async/await` is built on top of Promises and provides a cleaner, more readable way to write asynchronous code.

Instead of writing:

```jsx
fetchData()
  .then((data) => {
    console.log(data);
  })
  .catch((err) => {
    console.log(err);
  });
```

We can write:

```jsx
try {
  const data = await fetchData();
  console.log(data);
} catch (err) {
  console.log(err);
}
```

Much easier to read and maintain.

---

# What is async?

The `async` keyword is used before a function declaration.

It tells JavaScript:

> "This function will work with asynchronous operations."
> 

---

## Syntax

```jsx
async function myFunction() {

}
```

---

## Important Rule

An `async` function always returns a Promise.

Even if you return a normal value.

---

### Example

```jsx
async function greet() {
  return "Hello";
}

console.log(greet());
```

### Output

```jsx
Promise { "Hello" }
```

JavaScript automatically wraps the returned value inside a Promise.

Equivalent to:

```jsx
function greet() {
  return Promise.resolve("Hello");
}
```

---

# What is await?

The `await` keyword pauses execution until a Promise settles.

It can only be used inside an async function.

---

## Example

```jsx
function fetchData() {

  return new Promise((resolve) => {

    setTimeout(() => {
      resolve("Data Received");
    }, 2000);

  });

}
```

Using await:

```jsx
async function main() {

  console.log("Start");

  const data = await fetchData();

  console.log(data);

  console.log("End");

}

main();
```

### Output

```jsx
Start

(2 seconds later)

Data Received

End
```

---

# How await Works

Without await:

```jsx
async function main() {

  const data = fetchData();

  console.log(data);

}
```

Output:

```jsx
Promise { <pending> }
```

Because fetchData() returns a Promise.

---

With await:

```jsx
async function main() {

  const data = await fetchData();

  console.log(data);

}
```

Output:

```jsx
Data Received
```

Await extracts the resolved value from the Promise.

---

# Async/Await vs Promise Chaining

---

## Promise Chaining

```jsx
fetchData()

.then((data) => {

  console.log(data);

  return processData(data);

})

.then((result) => {

  console.log(result);

})

.catch((err) => {

  console.log(err);

});
```

---

## Async/Await

```jsx
async function main() {

  try {

    const data = await fetchData();

    const result = await processData(data);

    console.log(result);

  } catch(err) {

    console.log(err);

  }

}
```

Much cleaner.

---

# Error Handling with try/catch

With Promises:

```jsx
fetchData()
  .then((data) => {
    console.log(data);
  })
  .catch((err) => {
    console.log(err);
  });
```

---

With async/await:

```jsx
async function main() {

  try {

    const data = await fetchData();

    console.log(data);

  } catch(err) {

    console.log(err);

  }

}

main();
```

---

# Example

```jsx
function fetchData() {

  return new Promise((resolve, reject) => {

    setTimeout(() => {

      reject("Server Down");

    }, 1000);

  });

}
```

Using try/catch:

```jsx
async function main() {

  try {

    const data = await fetchData();

    console.log(data);

  } catch(error) {

    console.log(error);

  }

}

main();
```

### Output

```jsx
Server Down
```

---

# finally with Async/Await

Just like Promises have `.finally()`.

We can use:

```jsx
try {

}
catch {

}
finally {

}
```

---

## Example

```jsx
async function main() {

  try {

    const data = await fetchData();

  } catch(error) {

    console.log(error);

  } finally {

    console.log("Cleanup Finished");

  }

}
```

The finally block always executes.

---

# Sequential Execution

Tasks run one after another.

---

## Example

```jsx
function task(name, time) {

  return new Promise((resolve) => {

    setTimeout(() => {

      resolve(`${name} Completed`);

    }, time);

  });

}
```

Sequential execution:

```jsx
async function runTasks() {

  console.time("Sequential");

  const a = await task("A", 2000);

  const b = await task("B", 2000);

  const c = await task("C", 2000);

  console.log(a);
  console.log(b);
  console.log(c);

  console.timeEnd("Sequential");

}
```

### Output

```jsx
A Completed
B Completed
C Completed

Sequential: ~6000ms
```

---

## Why 6 Seconds?

Because:

```
A → wait 2 sec
B → wait 2 sec
C → wait 2 sec
```

Total:

```
2 + 2 + 2 = 6 sec
```

---

# Parallel Execution

Sometimes tasks are independent.

Instead of waiting one by one, run them together.

---

## Example

```jsx
async function runTasks() {

  console.time("Parallel");

  const promiseA = task("A", 2000);

  const promiseB = task("B", 2000);

  const promiseC = task("C", 2000);

  const results = await Promise.all([
    promiseA,
    promiseB,
    promiseC
  ]);

  console.log(results);

  console.timeEnd("Parallel");

}
```

### Output

```jsx
[
  "A Completed",
  "B Completed",
  "C Completed"
]

Parallel: ~2000ms
```

# Why Only 2 Seconds?

Because:

```
A
B
C
```

all start together.

Instead of:

```
2 + 2 + 2
```

it becomes:

```
max(2,2,2)
```

which is 2 seconds.

---

# Golden Rule

### Use Sequential Execution

When tasks depend on each other.

```jsx
const user = await getUser();

const orders = await getOrders(user.id);

const details = await getOrderDetails(orders[0]);
```

Each step needs the previous result.

---

### Use Parallel Execution

When tasks are independent.

```jsx
const [users, posts, comments] =
await Promise.all([
  getUsers(),
  getPosts(),
  getComments()
]);
```

Much faster.

---

# Common Mistakes 🚨

---

# Mistake 1: Forgetting await

### Wrong

```jsx
async function main() {

  const data = fetchData();

  console.log(data);

}
```

Output:

```jsx
Promise { <pending> }
```

---

### Correct

```jsx
async function main() {

  const data = await fetchData();

  console.log(data);

}
```

---

# Mistake 2: No try/catch

### Wrong

```jsx
async function main() {

  const data = await fetchData();

  console.log(data);

}
```

If Promise rejects:

```jsx
Unhandled Promise Rejection
```

---

### Correct

```jsx
async function main() {

  try {

    const data = await fetchData();

  } catch(error) {

    console.log(error);

  }

}
```

---

# Mistake 3: Using await inside forEach()

Very common interview question.

---

### Wrong

```jsx
const users = [1,2,3];

users.forEach(async (user) => {

  await processUser(user);

});

console.log("Done");
```

Output:

```jsx
Done
```

appears immediately.

`forEach()` does not wait.

---

### Correct

```jsx
for(const user of users) {

  await processUser(user);

}

console.log("Done");
```

Now it waits correctly.

---

# Mistake 4: Sequential Instead of Parallel

### Wrong

```jsx
const users = await getUsers();

const posts = await getPosts();

const comments = await getComments();
```

Time:

```
2 + 2 + 2 = 6 sec
```

---

### Better

```jsx
const [
  users,
  posts,
  comments
] = await Promise.all([
  getUsers(),
  getPosts(),
  getComments()
]);
```

Time:

```
2 sec
```

---

# Interview Questions

### Q1: What does async do?

**Answer:**

It makes a function return a Promise.

---

### Q2: What does await do?

**Answer:**

It pauses execution until a Promise settles and returns the resolved value.

---

### Q3: Can await be used outside async?

**Answer:**

No (except top-level await in ES Modules).

---

### Q4: Why use async/await?

**Answer:**

- Cleaner code
- Easier debugging
- Better readability
- Simpler error handling

---

### Q5: Which is faster?

```jsx
await taskA();
await taskB();
await taskC();
```

or

```jsx
await Promise.all([
  taskA(),
  taskB(),
  taskC()
]);
```

**Answer:**

`Promise.all()` because tasks execute in parallel.

---

# Quick Revision

| Concept | Summary |
| --- | --- |
| async | Makes a function return a Promise |
| await | Waits for Promise completion |
| try/catch | Handles async errors |
| finally | Runs regardless of success/failure |
| Sequential | One task after another |
| Parallel | Multiple tasks together |
| Promise.all() | Best for parallel execution |
| await in forEach | Wrong approach |
| await in for...of | Correct approach |

---

# Part 5: The JavaScript Event Loop 🤯

This is one of the most important concepts in JavaScript.

If you truly understand the Event Loop, you understand:

- `setTimeout`
- Promises
- `async/await`
- Execution Order
- Output Prediction Questions

and many tricky interview questions become easy.

---

# The Big Question

JavaScript is:

- Single-threaded
- Synchronous

So how can it perform asynchronous operations without blocking the application?

Examples:

```jsx
setTimeout(() => {}, 1000);

fetch("/users");

button.addEventListener("click", handler);
```

How can JavaScript handle these while still executing other code?

The answer is:

> **Event Loop**
> 

---

# Components of the Event Loop System

There are 4 major components.

```
Call Stack
Web APIs
Callback Queue
Microtask Queue
```

and one manager:

```
Event Loop
```

---

# 1. Call Stack

The Call Stack is where JavaScript executes code.

Think of it as a stack of plates.

### Rule

```
LIFO
Last In First Out
```

The last function added is the first one removed.

---

## Example

```jsx
function third() {
  console.log("Third");
}

function second() {
  third();
}

function first() {
  second();
}

first();
```

### Stack Execution

```
first()
  ↓
second()
  ↓
third()
```

---

### Stack State

```
Push first()

[first]

Push second()

[first]
[second]

Push third()

[first]
[second]
[third]

third() completes

[first]
[second]

second() completes

[first]

first() completes

empty
```

---

### Output

```jsx
Third
```

---

# Important Rule

Only one task can execute on the Call Stack at a time.

If something blocks the stack:

```jsx
while(true) {}
```

Everything freezes.

---

# 2. Web APIs

Here comes the magic.

Functions like:

```jsx
setTimeout()
fetch()
addEventListener()
```

are NOT actually part of JavaScript.

They are provided by:

- Browser APIs
- Node.js APIs

---

## Example

```jsx
console.log("Start");

setTimeout(() => {
  console.log("Timer Finished");
}, 2000);

console.log("End");
```

---

### What Happens?

Step 1:

```jsx
console.log("Start");
```

Output:

```
Start
```

---

Step 2:

```jsx
setTimeout(...)
```

The timer is handed over to the Browser.

```
Web API starts timer
```

JavaScript does NOT wait.

---

Step 3:

```jsx
console.log("End");
```

Output:

```
End
```

---

After 2 seconds:

The callback becomes ready.

It moves to:

```
Callback Queue
```

---

# 3. Callback Queue (Macrotask Queue)

When a timer completes:

```jsx
setTimeout(...)
```

its callback goes into the Callback Queue.

---

Think of it as:

```
People waiting in line.
```

First entered.

First executed.

```
FIFO
First In First Out
```

---

Example:

```jsx
setTimeout(() => {
  console.log("A");
}, 1000);

setTimeout(() => {
  console.log("B");
}, 1000);
```

Queue:

```
[A]
[B]
```

Execution:

```
A
B
```

---

# 4. Microtask Queue ⭐

This queue has HIGHER priority.

Contains:

```jsx
Promise.then()
Promise.catch()
Promise.finally()

await

queueMicrotask()
```

---

Example

```jsx
Promise.resolve()
.then(() => {
  console.log("Promise");
});
```

The callback goes into:

```
Microtask Queue
```

not Callback Queue.

---

# Priority Rule

```
Microtask Queue
      ↑
Higher Priority

Callback Queue
      ↑
Lower Priority
```

This rule explains many interview questions.

---

# Event Loop

The Event Loop continuously checks:

```
Is the Call Stack empty?
```

If YES:

1. Execute ALL Microtasks
2. Execute ONE Macrotask
3. Repeat

---

## Event Loop Algorithm

```
1. Run all synchronous code

2. Stack becomes empty

3. Execute all Microtasks

4. Execute one Macrotask

5. Check Microtasks again

6. Execute next Macrotask

7. Repeat forever
```

---

# Visual Architecture

```
               ┌─────────────┐
               │ Call Stack  │
               └──────┬──────┘
                      │
                      ▼

               ┌─────────────┐
               │  Web APIs   │
               └──────┬──────┘
                      │
        ┌─────────────┴─────────────┐
        ▼                           ▼

 ┌─────────────┐           ┌─────────────┐
 │ Microtasks  │           │ Macrotasks  │
 │ (Promises)  │           │ setTimeout  │
 └──────┬──────┘           └──────┬──────┘
        │                         │
        └─────────┬───────────────┘
                  ▼

            Event Loop
```

---

# Example 1

```jsx
console.log("1");

setTimeout(() => {
  console.log("2");
}, 0);

Promise.resolve()
.then(() => {
  console.log("3");
});

console.log("4");
```

---

## Predict Output

Most beginners say:

```
1
2
3
4
```

Wrong ❌

---

### Step-by-Step

### Synchronous Code

```jsx
console.log("1");
```

Output:

```
1
```

---

```jsx
setTimeout(...)
```

Moves to Web API.

---

```jsx
Promise.resolve().then(...)
```

Moves to Microtask Queue.

---

```jsx
console.log("4");
```

Output:

```
4
```

---

Call Stack becomes empty.

---

### Event Loop

Execute all Microtasks:

```
3
```

---

Execute Macrotask:

```
2
```

---

### Final Output

```
1
4
3
2
```

✅ Correct Answer

---

# Why setTimeout(0) Doesn't Run First

Many students think:

```jsx
setTimeout(fn, 0);
```

means:

```
Run immediately
```

Wrong.

It means:

```
Run after current execution finishes
AND
after microtasks complete.
```

---

# Example 2

```jsx
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 0);

Promise.resolve()
.then(() => {
  console.log("Promise");
});

console.log("End");
```

---

### Output

```
Start
End
Promise
Timeout
```

---

Because:

```
Promise → Microtask

setTimeout → Macrotask
```

Microtask wins.

---

# Example 3

```jsx
console.log("A");

Promise.resolve()
.then(() => {
  console.log("B");
});

Promise.resolve()
.then(() => {
  console.log("C");
});

console.log("D");
```

---

### Output

```
A
D
B
C
```

---

Why?

```
A D → Sync

B C → Microtasks
```

---

# Example 4 (Promise Chain)

```jsx
Promise.resolve()

.then(() => {
  console.log("1");
})

.then(() => {
  console.log("2");
})

.then(() => {
  console.log("3");
});
```

---

### Output

```
1
2
3
```

---

Because every `.then()` creates another microtask.

---

# async/await and Event Loop

Many developers don't realize:

```jsx
await
```

also creates a microtask.

---

## Example

```jsx
async function test() {

  console.log("A");

  await Promise.resolve();

  console.log("B");

}

console.log("Start");

test();

console.log("End");
```

---

### Step-by-Step

Output:

```
Start
A
End
B
```

---

Why?

When JavaScript sees:

```jsx
await Promise.resolve();
```

it pauses execution.

Everything after await:

```jsx
console.log("B");
```

becomes a microtask.

# Interview Question 1

```jsx
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

Promise.resolve()

.then(() => {
  console.log("C");
})

.then(() => {
  console.log("D");
});

console.log("E");
```

---

### Answer

```
A
E
C
D
B
```

---

Reason:

```
A E → Sync

C D → Microtasks

B → Macrotask
```

---

# Interview Question 2

```jsx
console.log("Start");

setTimeout(() => {
  console.log("Timeout 1");
}, 0);

Promise.resolve()

.then(() => {

  console.log("Promise 1");

  setTimeout(() => {
    console.log("Timeout 2");
  }, 0);

});

setTimeout(() => {
  console.log("Timeout 3");
}, 0);

console.log("End");
```

---

### Answer

```
Start
End
Promise 1
Timeout 1
Timeout 3
Timeout 2
```

---

Reason:

During Promise execution:

```jsx
setTimeout(() => {
  console.log("Timeout 2");
});
```

gets added later.

Therefore it enters the queue later.

---

# Interview Question 3 (Very Important)

```jsx
async function async1() {

  console.log("async1 start");

  await async2();

  console.log("async1 end");

}

async function async2() {

  console.log("async2");

}

console.log("script start");

async1();

console.log("script end");
```

---

### Output

```
script start
async1 start
async2
script end
async1 end
```

---

Reason:

Everything after:

```jsx
await async2();
```

becomes a microtask.

---

# Golden Rule (Memorize This)

```
1. Run all synchronous code

2. Execute ALL Microtasks
   (Promises, await)

3. Execute ONE Macrotask
   (setTimeout)

4. Repeat
```

---

# Quick Revision

| Concept | Summary |
| --- | --- |
| Call Stack | Executes JavaScript code |
| Web APIs | Browser/Node async features |
| Callback Queue | Stores Macrotasks |
| Microtask Queue | Stores Promise callbacks |
| Event Loop | Coordinates execution |
| Promise.then() | Microtask |
| await | Microtask |
| setTimeout() | Macrotask |
| Priority | Microtask > Macrotask |
| Execution Order | Sync → Microtasks → Macrotasks |

---

# Most Important Interview Rule

```
Synchronous Code
        ↓
Microtasks
(Promise, await)
        ↓
Macrotasks
(setTimeout)
```

If you remember only one thing from the Event Loop chapter, remember this.

---

# Part 6: Fetch API & HTTP Requests 🌐

The Fetch API is the modern way to communicate with servers in JavaScript.

Whenever your React, Node.js, or MERN application needs data from a backend, Fetch API is commonly used.

Examples:

- Login User
- Register User
- Get Products
- Get User Profile
- Submit Forms
- Upload Data

All of these involve HTTP requests.

---

# What is HTTP?

HTTP stands for:

```
HyperText Transfer Protocol
```

It is the communication protocol used between:

```
Client (Browser)
       ↔
Server
```

---

## Example

When you open:

```
https://google.com
```

Your browser sends:

```
HTTP Request
```

Google's server sends:

```
HTTP Response
```

---

# Request → Response Cycle

```
Browser
   |
   | Request
   ▼
Server
   |
   | Response
   ▼
Browser
```

---

# HTTP Methods

Different methods are used for different operations.

| Method | Purpose |
| --- | --- |
| GET | Retrieve data |
| POST | Create data |
| PUT | Replace data |
| PATCH | Update data |
| DELETE | Delete data |

---

# GET Request

Used when fetching data from a server.

Example:

```
Get User
Get Products
Get Posts
Get Comments
```

---

## Fetch Syntax

```jsx
fetch(url);
```

---

## Example

```jsx
fetch("https://jsonplaceholder.typicode.com/users/1");
```

---

# Fetch Returns a Promise

Important interview question.

```jsx
const response = fetch(url);

console.log(response);
```

Output:

```jsx
Promise { <pending> }
```

Because Fetch is asynchronous.

---

# Using .then()

```jsx
fetch("https://jsonplaceholder.typicode.com/users/1")

.then((response) => {
  return response.json();
})

.then((data) => {
  console.log(data);
})

.catch((error) => {
  console.log(error);
});
```

# Why response.json()?

Fetch does NOT directly return data.

It returns a Response object.

---

Example:

```jsx
fetch(url)

.then((response) => {
  console.log(response);
});
```

Output:

```jsx
Response {
  body: ...
  status: 200
  headers: ...
}
```

---

To get actual JSON data:

```jsx
response.json()
```

must be called.

---

# Important Interview Point

`response.json()` also returns a Promise.

Therefore:

```jsx
response.json()
```

needs:

```jsx
.then()
```

or

```jsx
await
```

---

# Fetch Using Async/Await

Preferred in modern applications.

```jsx
async function getUser() {

  const response =
    await fetch(
      "https://jsonplaceholder.typicode.com/users/1"
    );

  const data =
    await response.json();

  console.log(data);

}

getUser();
```

---

# Understanding the Two Awaits

### First Await

```jsx
const response =
await fetch(url);
```

Waits for the server response.

---

### Second Await

```jsx
const data =
await response.json();
```

Waits for JSON parsing.

---

# Visual Flow

```
fetch()
   ↓
Response Object
   ↓
response.json()
   ↓
JavaScript Object
```

---

# Example Response

Server sends:

```json
{
  "id": 1,
  "name": "Leanne Graham"
}
```

After:

```jsx
const data =
await response.json();
```

You get:

```jsx
{
  id: 1,
  name: "Leanne Graham"
}
```

---

# Accessing Data

```jsx
console.log(data.name);
```

Output:

```jsx
Leanne Graham
```

---

# Error Handling

Always use try/catch with async/await.

---

## Example

```jsx
async function getUser() {

  try {

    const response =
      await fetch(
        "https://jsonplaceholder.typicode.com/users/1"
      );

    const data =
      await response.json();

    console.log(data);

  } catch(error) {

    console.log(error);

  }

}
```

---

# Important Fetch Gotcha 🚨

Many beginners think:

```jsx
404
500
```

automatically trigger catch.

Wrong.

---

# Example

```jsx
try {

  const response =
    await fetch("/wrong-url");

} catch(error) {

  console.log(error);

}
```

Catch only executes when:

- Internet fails
- DNS fails
- Network error occurs

---

# 404 Does NOT Trigger catch

Example:

```jsx
const response =
await fetch("/wrong-url");
```

Server returns:

```
404 Not Found
```

Fetch still resolves successfully.

---

# Checking response.ok

Always check:

```jsx
response.ok
```

---

## Example

```jsx
async function getUser() {

  try {

    const response =
      await fetch(
        "https://jsonplaceholder.typicode.com/users/99999"
      );

    if(!response.ok) {

      throw new Error(
        `HTTP Error: ${response.status}`
      );

    }

    const data =
      await response.json();

    console.log(data);

  } catch(error) {

    console.log(error.message);

  }

}
```

---

# Common HTTP Status Codes

| Code | Meaning |
| --- | --- |
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |

---

# POST Request

Used to send data to the server.

Examples:

```
Login
Register
Create Product
Create Post
```

---

# POST Request Structure

```jsx
fetch(url, {
  method: "POST",
  headers: {},
  body: ""
});
```

---

# Example

```jsx
async function createUser() {

  const response =
    await fetch(
      "https://jsonplaceholder.typicode.com/users",
      {
        method: "POST",

        headers: {
          "Content-Type":
            "application/json"
        },

        body: JSON.stringify({
          name: "Rahul",
          email: "rahul@gmail.com"
        })
      }
    );

  const data =
    await response.json();

  console.log(data);

}
```

---

# Understanding Headers

Headers provide additional information about the request.

---

## Common Header

```jsx
headers: {
  "Content-Type": "application/json"
}
```

Meaning:

```
I am sending JSON data.
```

---

# Understanding Body

Body contains the actual data being sent.

---

Example:

```jsx
body: JSON.stringify({
  name: "Rahul",
  email: "rahul@gmail.com"
})
```

---

# Why JSON.stringify()?

Because servers cannot directly understand JavaScript objects.

This:

```jsx
{
  name: "Rahul"
}
```

must become:

```json
{
  "name":"Rahul"
}
```

before being transmitted.

---

# PUT Request

Replace existing data.

```jsx
await fetch(url, {
  method: "PUT",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify(updatedData)
});
```

# PATCH Request

Partially update data.

```jsx
await fetch(url, {
  method: "PATCH",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    name: "Updated Name"
  })
});
```

---

# DELETE Request

Remove data from server.

```jsx
await fetch(url, {
  method: "DELETE"
});
```

---

# Real MERN Example

### Login Request

```jsx
const response =
await fetch(
  "http://localhost:5000/api/auth/login",
  {
    method: "POST",

    headers: {
      "Content-Type":
      "application/json"
    },

    body: JSON.stringify({
      email,
      password
    })
  }
);
```

---

# Common Interview Questions

### Q1: Does fetch return data directly?

**Answer:**

No.

It returns a Promise.

---

### Q2: Why do we use response.json()?

**Answer:**

To convert the response body into a JavaScript object.

---

### Q3: Does response.json() return a Promise?

**Answer:**

Yes.

---

### Q4: Does fetch reject on 404?

**Answer:**

No.

Only network failures reject.

---

### Q5: How do we check request success?

**Answer:**

```jsx
response.ok
```

or

```jsx
response.status
```

---

# Quick Revision

| Concept | Summary |
| --- | --- |
| fetch() | Sends HTTP requests |
| GET | Retrieve data |
| POST | Create data |
| PUT | Replace data |
| PATCH | Update data |
| DELETE | Remove data |
| response.json() | Converts response into JS object |
| response.ok | Checks request success |
| try/catch | Handles network errors |
| JSON.stringify() | Converts object → JSON |

---

## MERN Interview Tip

Memorize this pattern:

```jsx
try {

  const response =
    await fetch(url);

  if(!response.ok) {
    throw new Error("Request Failed");
  }

  const data =
    await response.json();

  console.log(data);

} catch(error) {

  console.log(error);

}
```

You'll use this structure repeatedly in React and Node.js projects.

# Part 7: JSON, JSON.parse() & JSON.stringify() 📦

JSON is one of the most important topics in JavaScript because almost all frontend-backend communication happens through JSON.

Whenever you:

- Login
- Register
- Fetch products
- Send user data
- Call APIs

you are working with JSON.

---

# What is JSON?

JSON stands for:

```
JavaScript Object Notation
```

It is a lightweight text format used for storing and transferring data.

---

## Why JSON?

JavaScript objects cannot be sent directly over the internet.

Example:

```jsx
const user = {
  name: "Rahul",
  age: 25
};
```

Servers cannot understand JavaScript objects directly.

Therefore we convert them into JSON.

---

# Example JSON

```json
{
  "name": "Rahul",
  "age": 25,
  "isStudent": true
}
```

---

# JSON vs JavaScript Object

### JavaScript Object

```jsx
const user = {
  name: "Rahul",
  age: 25
};
```

---

### JSON

```json
{
  "name": "Rahul",
  "age": 25
}
```

---

# Differences

| JavaScript Object | JSON |
| --- | --- |
| Keys can be without quotes | Keys must be in double quotes |
| Can contain functions | Functions not allowed |
| Can contain undefined | Undefined not allowed |
| Actual object | String format |

---

# JSON Rules

### Rule 1: Keys Must Use Double Quotes

✅ Correct

```json
{
  "name": "Rahul"
}
```

❌ Wrong

```json
{
  name: "Rahul"
}
```

---

### Rule 2: Strings Must Use Double Quotes

✅ Correct

```json
{
  "city": "Indore"
}
```

❌ Wrong

```json
{
  "city": 'Indore'
}
```

---

### Rule 3: Functions Are Not Allowed

❌ Invalid JSON

```jsx
{
  "name": "Rahul",
  "greet": function(){}
}
```

---

### Rule 4: Undefined Is Not Allowed

❌ Invalid

```jsx
{
  "age": undefined
}
```

---

# JSON.stringify()

Converts:

```
JavaScript Object
        ↓
JSON String
```

---

## Syntax

```jsx
JSON.stringify(value);
```

---

## Example

```jsx
const user = {
  name: "Rahul",
  age: 25
};

const jsonString =
JSON.stringify(user);

console.log(jsonString);
```

---

### Output

```jsx
'{"name":"Rahul","age":25}'
```

# Checking Type

```jsx
console.log(typeof jsonString);
```

Output:

```jsx
string
```

After stringify, it becomes a string.

---

# Why Use JSON.stringify()?

When sending data to a server.

Example:

```jsx
fetch(url, {
  method: "POST",

  headers: {
    "Content-Type":
    "application/json"
  },

  body: JSON.stringify({
    name: "Rahul"
  })
});
```

Without stringify, the server may not understand the data.

---

# Pretty Printing JSON

Syntax:

```jsx
JSON.stringify(
  value,
  replacer,
  spaces
);
```

---

## Example

```jsx
const user = {
  name: "Rahul",
  age: 25
};

console.log(
  JSON.stringify(
    user,
    null,
    2
  )
);
```

---

### Output

```json
{
  "name": "Rahul",
  "age": 25
}
```

Much easier to read.

---

# What stringify Removes

Example:

```jsx
const user = {
  name: "Rahul",
  age: undefined,

  greet() {
    console.log("Hello");
  }
};

console.log(
  JSON.stringify(user)
);
```

---

### Output

```json
{
  "name":"Rahul"
}
```

Notice:

```jsx
age
greet
```

are removed.

---

# JSON.parse()

Converts:

```
JSON String
       ↓
JavaScript Object
```

---

## Syntax

```jsx
JSON.parse(string);
```

---

## Example

```jsx
const jsonString =
'{"name":"Rahul","age":25}';

const user =
JSON.parse(jsonString);

console.log(user);
```

---

### Output

```jsx
{
  name: "Rahul",
  age: 25
}
```

---

# Checking Type

```jsx
console.log(typeof user);
```

Output:

```jsx
object
```

# Accessing Values

```jsx
console.log(user.name);
```

Output:

```jsx
Rahul
```

---

# Why Use JSON.parse()?

When receiving data from a server.

Example:

```jsx
const response =
await fetch(url);

const data =
await response.json();
```

Internally:

```jsx
JSON.parse(...)
```

is being used.

---

# Invalid JSON Example

```jsx
JSON.parse(
  "{name:'Rahul'}"
);
```

---

### Output

```jsx
SyntaxError
```

Because JSON rules are violated.

---

# Safe Parsing Using try/catch

```jsx
try {

  const data =
    JSON.parse(
      "{name:'Rahul'}"
    );

} catch(error) {

  console.log(
    "Invalid JSON"
  );

}
```

---

# stringify + parse Together

```jsx
const user = {
  name: "Rahul",
  age: 25
};

const str =
JSON.stringify(user);

const obj =
JSON.parse(str);

console.log(obj);
```

---

### Output

```jsx
{
  name: "Rahul",
  age: 25
}
```

---

# Deep Copy Trick

A classic JavaScript trick:

```jsx
const original = {
  name: "Rahul",
  address: {
    city: "Indore"
  }
};

const copy =
JSON.parse(
  JSON.stringify(original)
);
```

---

# Why?

Because nested objects are copied as well.

---

## Example

```jsx
copy.address.city =
"Bhopal";

console.log(
  original.address.city
);
```

Output:

```jsx
Indore
```

Original remains unchanged.

---

# Limitation of Deep Copy Trick

This method does NOT copy:

- Functions
- Dates
- Maps
- Sets
- Undefined
- Symbols

Modern solution:

```jsx
structuredClone(object);
```

---

# Complete Flow of API Data

### Sending Data

```jsx
JavaScript Object
       ↓
JSON.stringify()
       ↓
JSON String
       ↓
Server
```

---

### Receiving Data

```jsx
Server JSON
      ↓
JSON.parse()
      ↓
JavaScript Object
```

---

# Most Asked Interview Questions

### Q1: What is JSON?

**Answer:**

A lightweight text format used to store and exchange data.

---

### Q2: Difference between JSON and JavaScript Object?

**Answer:**

JSON is a string format.

JavaScript Object is an actual object.

---

### Q3: What does JSON.stringify() do?

**Answer:**

Converts:

```jsx
Object → JSON String
```

---

### Q4: What does JSON.parse() do?

**Answer:**

Converts:

```jsx
JSON String → Object
```

---

### Q5: Why use JSON.stringify() in Fetch?

**Answer:**

Because request bodies must be sent as strings.

---

### Q6: Why does JSON.parse() throw errors?

**Answer:**

Because the string is not valid JSON.

---

### Q7: Can JSON store functions?

**Answer:**

No.

---

### Q8: Can JSON store undefined?

**Answer:**

No.

---

# Final Async JavaScript Revision Sheet 🚀

| Topic | Key Point |
| --- | --- |
| Synchronous | Executes line by line |
| Asynchronous | Non-blocking execution |
| Callback | Function passed into another function |
| Callback Hell | Deep nested callbacks |
| Promise | Future result of async operation |
| States | Pending, Fulfilled, Rejected |
| then() | Success handler |
| catch() | Error handler |
| finally() | Runs always |
| Promise.all() | All promises must succeed |
| Promise.race() | First settled promise wins |
| Promise.any() | First successful promise wins |
| Promise.allSettled() | Returns all results |
| async | Makes function return Promise |
| await | Waits for Promise completion |
| try/catch | Handles async errors |
| Event Loop | Manages async execution |
| Microtask Queue | Promises & await |
| Macrotask Queue | setTimeout & setInterval |
| Fetch API | Sends HTTP requests |
| response.json() | Converts response to object |
| response.ok | Checks request success |
| JSON.stringify() | Object → JSON |
| JSON.parse() | JSON → Object |

---

## Async JavaScript Learning Path (Interview Order)

```
Synchronous vs Asynchronous
          ↓
Callbacks
          ↓
setTimeout / setInterval
          ↓
Callback Hell
          ↓
Promises
          ↓
Promise Methods
          ↓
Async / Await
          ↓
Event Loop
          ↓
Fetch API
          ↓
JSON
```

If you master these 7 parts thoroughly and practice the output-based Event Loop questions, you'll have a strong foundation in Async JavaScript for React, Node.js, MERN stack development, and technical interviews.