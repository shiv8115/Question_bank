# JavaScript Interview Questions

> **Section:** JavaScript
> **Total Questions:** 55
> **Difficulty:** 40% Basic · 40% Intermediate · 20% Practical
> **Focus:** Core JS, ES6+, async, closures, prototypes, debugging, real-world scenarios

---

## Table of Contents
- [Basic Questions (Q1–Q22)](#basic-questions)
- [Intermediate Questions (Q23–Q44)](#intermediate-questions)
- [Practical / Scenario Questions (Q45–Q55)](#practical--scenario-questions)

---

## Basic Questions

---

## Q1. What are the different data types in JavaScript?
**Difficulty:** Basic
**Type:** Conceptual

### Question
List all primitive and non-primitive data types in JavaScript.

### Answer
**Primitives (7):** `string`, `number`, `bigint`, `boolean`, `undefined`, `null`, `symbol`

**Non-primitive:** `object` (includes arrays, functions, and plain objects)

### Explanation
Primitives are immutable and stored by value. Objects are stored by reference. `typeof null === "object"` is a well-known JavaScript quirk/bug that has existed since the language's beginning.

### Code
```javascript
typeof "hello"      // "string"
typeof 42           // "number"
typeof true         // "boolean"
typeof undefined    // "undefined"
typeof null         // "object" ← quirk!
typeof Symbol()     // "symbol"
typeof {}           // "object"
typeof []           // "object"
typeof function(){} // "function"
```

---

## Q2. What is the difference between `var`, `let`, and `const`?
**Difficulty:** Basic
**Type:** Conceptual

### Question
Explain the key differences between `var`, `let`, and `const` in terms of scope, hoisting, and reassignment.

### Answer
| Feature        | `var`            | `let`          | `const`        |
|----------------|------------------|----------------|----------------|
| Scope          | Function-scoped  | Block-scoped   | Block-scoped   |
| Hoisting       | Yes (undefined)  | Yes (TDZ)      | Yes (TDZ)      |
| Reassignment   | Yes              | Yes            | No             |
| Re-declaration | Yes              | No             | No             |

### Explanation
`var` leaks out of blocks (like `if` or `for`) but is contained in functions. `let` and `const` are contained in any block `{}`. The Temporal Dead Zone (TDZ) means the variable exists but cannot be accessed before its declaration line.

### Code
```javascript
function example() {
  if (true) {
    var x = 1;   // accessible outside the if block
    let y = 2;   // block-scoped, not accessible outside
    const z = 3; // block-scoped, not reassignable
  }
  console.log(x); // 1
  console.log(y); // ReferenceError
}

// TDZ example
console.log(a); // undefined (var is hoisted + initialized)
console.log(b); // ReferenceError (let is hoisted but in TDZ)
var a = 5;
let b = 10;
```

---

## Q3. What is `==` vs `===` in JavaScript?
**Difficulty:** Basic
**Type:** Conceptual

### Question
What is the difference between loose equality (`==`) and strict equality (`===`)?

### Answer
- `==` performs **type coercion** before comparing
- `===` compares both **value AND type** without coercion

### Explanation
`==` can produce surprising results. As a best practice, always use `===` in production code unless you have a specific reason for type coercion.

### Code
```javascript
0 == false    // true  (coercion: false → 0)
0 === false   // false (different types)

"" == false   // true  (coercion)
"" === false  // false

null == undefined  // true  (special case)
null === undefined // false

1 == "1"   // true  (string coerced to number)
1 === "1"  // false
```

---

## Q4. What is hoisting in JavaScript?
**Difficulty:** Basic
**Type:** Conceptual

### Question
Explain hoisting. What gets hoisted and what doesn't?

### Answer
Hoisting is JavaScript's default behavior of moving **declarations** (not initializations) to the top of their scope during the compilation phase.

- `var` declarations are hoisted and initialized to `undefined`
- `function` declarations are fully hoisted (including their body)
- `let` and `const` are hoisted but NOT initialized (TDZ)
- `class` declarations are hoisted but in TDZ

### Code
```javascript
// Function declaration — fully hoisted
sayHello(); // "Hello!"
function sayHello() { console.log("Hello!"); }

// var — declaration hoisted, not initialization
console.log(name); // undefined (not ReferenceError)
var name = "Alice";

// Function expression — NOT hoisted
greet(); // TypeError: greet is not a function
var greet = function() { console.log("Hi"); };
```

---

## Q5. What is a closure in JavaScript?
**Difficulty:** Basic
**Type:** Conceptual

### Question
What is a closure? Give a simple example.

### Answer
A closure is a function that **remembers the variables from its outer scope** even after that outer function has returned.

### Explanation
Closures are created every time a function is created. The inner function has access to: its own variables, the outer function's variables, and global variables. This is the foundation of data privacy in JavaScript.

### Code
```javascript
function makeCounter() {
  let count = 0; // this variable is "closed over"

  return function() {
    count++;
    return count;
  };
}

const counter = makeCounter();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3

// count is not accessible from outside
console.log(count); // ReferenceError
```

---

## Q6. What is the difference between `null` and `undefined`?
**Difficulty:** Basic
**Type:** Conceptual

### Question
When would you encounter `null` vs `undefined` in JavaScript?

### Answer
- `undefined` — a variable has been declared but **not assigned** a value (also default return value of functions)
- `null` — an **intentional absence** of value, explicitly set by the developer

### Code
```javascript
let a;
console.log(a); // undefined

let b = null;
console.log(b); // null (intentionally empty)

function doNothing() {}
console.log(doNothing()); // undefined

typeof undefined // "undefined"
typeof null      // "object" (historical bug)
```

---

## Q7. What are arrow functions and how do they differ from regular functions?
**Difficulty:** Basic
**Type:** Conceptual

### Question
What are arrow functions? What is the main behavioral difference from regular functions?

### Answer
Arrow functions are a concise function syntax introduced in ES6. The key difference: **arrow functions do NOT have their own `this`** — they inherit `this` from the enclosing lexical scope.

### Code
```javascript
// Regular function — this depends on how it's called
const obj = {
  name: "Alice",
  greet: function() {
    console.log(this.name); // "Alice"
  }
};

// Arrow function — this is lexically bound
const obj2 = {
  name: "Bob",
  greet: () => {
    console.log(this.name); // undefined (this = outer/global)
  }
};

// Arrow functions are great for callbacks
[1, 2, 3].map(n => n * 2); // [2, 4, 6]

// Arrow functions cannot be used as constructors
const Foo = () => {};
new Foo(); // TypeError
```

---

## Q8. Explain the concept of `this` in JavaScript.
**Difficulty:** Basic
**Type:** Conceptual

### Question
What does `this` refer to in different contexts?

### Answer
`this` refers to the **object that is executing the current function**. Its value depends on how the function is called:

| Context                    | `this` value           |
|----------------------------|------------------------|
| Global scope               | `window` / `global`    |
| Object method              | The object             |
| Constructor (`new`)        | New instance           |
| `call`, `apply`, `bind`    | First argument passed  |
| Arrow function             | Lexical (outer) scope  |
| Strict mode global         | `undefined`            |

### Code
```javascript
const user = {
  name: "Alice",
  getName() {
    return this.name; // "Alice"
  }
};

function show() {
  console.log(this); // window (non-strict) / undefined (strict)
}

const bound = show.bind({ name: "Bob" });
bound(); // { name: "Bob" }
```

---

## Q9. What is the difference between `call`, `apply`, and `bind`?
**Difficulty:** Basic
**Type:** Conceptual

### Question
Explain `call`, `apply`, and `bind` with examples.

### Answer
All three explicitly set `this` for a function:
- `call(thisArg, arg1, arg2)` — calls immediately, args passed individually
- `apply(thisArg, [args])` — calls immediately, args passed as array
- `bind(thisArg, arg1)` — returns a new function with `this` bound, does NOT call immediately

### Code
```javascript
function greet(greeting, punctuation) {
  return `${greeting}, ${this.name}${punctuation}`;
}

const person = { name: "Alice" };

greet.call(person, "Hello", "!");   // "Hello, Alice!"
greet.apply(person, ["Hi", "."]);   // "Hi, Alice."
const greetAlice = greet.bind(person, "Hey");
greetAlice("?");                    // "Hey, Alice?"
```

---

## Q10. What are template literals?
**Difficulty:** Basic
**Type:** Conceptual

### Question
What are template literals and when should you use them?

### Answer
Template literals (backtick strings) allow **embedded expressions**, **multi-line strings**, and **tagged templates**. Introduced in ES6.

### Code
```javascript
const name = "Alice";
const age = 30;

// String interpolation
console.log(`Hello, ${name}! You are ${age} years old.`);

// Multi-line strings
const message = `Line 1
Line 2
Line 3`;

// Expressions inside
console.log(`2 + 2 = ${2 + 2}`);  // "2 + 2 = 4"
console.log(`${age > 18 ? "Adult" : "Minor"}`); // "Adult"
```

---

## Q11. What is destructuring in JavaScript?
**Difficulty:** Basic
**Type:** Conceptual

### Question
Explain array and object destructuring with examples.

### Answer
Destructuring allows you to **unpack values** from arrays or properties from objects into distinct variables.

### Code
```javascript
// Object destructuring
const user = { name: "Alice", age: 30, city: "Delhi" };
const { name, age, city: location } = user; // rename city → location
console.log(name, age, location); // "Alice" 30 "Delhi"

// With defaults
const { country = "India" } = user;
console.log(country); // "India"

// Array destructuring
const [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first);  // 1
console.log(second); // 2
console.log(rest);   // [3, 4, 5]

// In function parameters
function display({ name, age }) {
  console.log(`${name} is ${age}`);
}
display(user);
```

---

## Q12. What is the spread operator?
**Difficulty:** Basic
**Type:** Conceptual

### Question
What does the spread operator (`...`) do? Give examples for arrays and objects.

### Answer
The spread operator **expands** an iterable (array, string, object) into individual elements.

### Code
```javascript
// Arrays
const a = [1, 2, 3];
const b = [4, 5, 6];
const combined = [...a, ...b]; // [1, 2, 3, 4, 5, 6]

// Copying arrays (shallow)
const copy = [...a]; // [1, 2, 3]

// Objects
const defaults = { color: "blue", size: "medium" };
const custom = { ...defaults, size: "large", weight: 10 };
// { color: "blue", size: "large", weight: 10 }

// Function arguments
function sum(x, y, z) { return x + y + z; }
const nums = [1, 2, 3];
sum(...nums); // 6
```

---

## Q13. What are Promises in JavaScript?
**Difficulty:** Basic
**Type:** Conceptual

### Question
What is a Promise? What are its three states?

### Answer
A Promise is an object representing the **eventual completion or failure** of an asynchronous operation.

**States:**
1. `pending` — initial state
2. `fulfilled` — operation completed successfully
3. `rejected` — operation failed

### Code
```javascript
const fetchData = new Promise((resolve, reject) => {
  const success = true;
  if (success) {
    resolve({ data: "Here's your data" });
  } else {
    reject(new Error("Something went wrong"));
  }
});

fetchData
  .then(result => console.log(result.data))
  .catch(err => console.error(err.message))
  .finally(() => console.log("Always runs"));
```

---

## Q14. What is `async/await`?
**Difficulty:** Basic
**Type:** Conceptual

### Question
What is `async/await` and how does it relate to Promises?

### Answer
`async/await` is **syntactic sugar over Promises**. An `async` function always returns a Promise. `await` pauses execution until the Promise resolves, making async code look synchronous.

### Code
```javascript
// Promise-based
function getUser() {
  return fetch("/api/user")
    .then(res => res.json())
    .then(data => data)
    .catch(err => console.error(err));
}

// Equivalent async/await
async function getUser() {
  try {
    const res = await fetch("/api/user");
    const data = await res.json();
    return data;
  } catch (err) {
    console.error(err);
  }
}
```

---

## Q15. What is the difference between synchronous and asynchronous code?
**Difficulty:** Basic
**Type:** Conceptual

### Question
Explain the difference between sync and async code execution in JavaScript.

### Answer
- **Synchronous**: Code executes line-by-line, each operation blocks the next.
- **Asynchronous**: Long-running tasks (I/O, network) are offloaded; execution continues while they complete.

### Explanation
JavaScript is single-threaded. Async operations use the **event loop** — they're handled by the browser/Node.js APIs and their callbacks are queued when done.

### Code
```javascript
// Sync — blocking
console.log("Start");
const result = expensiveCalculation(); // blocks here
console.log("End");

// Async — non-blocking
console.log("Start");
setTimeout(() => console.log("Timeout"), 0);
console.log("End");
// Output: Start → End → Timeout
```

---

## Q16. What is the Event Loop?
**Difficulty:** Basic
**Type:** Conceptual

### Question
Briefly explain how the JavaScript event loop works.

### Answer
The event loop constantly checks the **call stack**. When it's empty, it picks tasks from the **task queue** (macro-tasks like `setTimeout`) or **microtask queue** (Promises) and pushes them to the stack.

**Priority:** Microtasks run before macro-tasks.

### Code
```javascript
console.log("1");

setTimeout(() => console.log("2"), 0);  // macro-task

Promise.resolve().then(() => console.log("3")); // microtask

console.log("4");

// Output: 1 → 4 → 3 → 2
```

---

## Q17. What are higher-order functions? Give examples.
**Difficulty:** Basic
**Type:** Conceptual

### Question
What is a higher-order function? Name three built-in examples.

### Answer
A higher-order function is a function that **takes another function as an argument** and/or **returns a function**.

Common examples: `map`, `filter`, `reduce`

### Code
```javascript
const numbers = [1, 2, 3, 4, 5];

// map — transform each element
const doubled = numbers.map(n => n * 2);        // [2, 4, 6, 8, 10]

// filter — select elements matching condition
const evens = numbers.filter(n => n % 2 === 0); // [2, 4]

// reduce — accumulate to a single value
const sum = numbers.reduce((acc, n) => acc + n, 0); // 15

// Custom higher-order function
function withLogging(fn) {
  return function(...args) {
    console.log("Calling with", args);
    return fn(...args);
  };
}
const loggedAdd = withLogging((a, b) => a + b);
loggedAdd(2, 3); // logs "Calling with [2, 3]", returns 5
```

---

## Q18. What is the difference between `map`, `filter`, and `forEach`?
**Difficulty:** Basic
**Type:** Conceptual

### Question
Compare `map`, `filter`, and `forEach` — when would you use each?

### Answer
| Method    | Returns          | Use When                          |
|-----------|------------------|-----------------------------------|
| `map`     | New array        | Transform every element           |
| `filter`  | New array        | Select elements matching criteria |
| `forEach` | `undefined`      | Side effects only (no return)     |

### Code
```javascript
const items = [1, 2, 3, 4, 5];

items.map(x => x * 10);       // [10, 20, 30, 40, 50]
items.filter(x => x > 3);     // [4, 5]
items.forEach(x => console.log(x)); // prints each, returns undefined

// Common mistake: using forEach expecting a new array
const bad = items.forEach(x => x * 2); // bad = undefined!
const good = items.map(x => x * 2);    // good = [2, 4, 6, 8, 10]
```

---

## Q19. What is `Array.reduce()`?
**Difficulty:** Basic
**Type:** Conceptual

### Question
How does `Array.reduce()` work? Show an example beyond simple summation.

### Answer
`reduce(callback, initialValue)` executes the callback for each element, **accumulating** a single result. The accumulator can be a number, string, array, or object.

### Code
```javascript
// Sum
[1, 2, 3].reduce((acc, n) => acc + n, 0); // 6

// Flatten array
[[1, 2], [3, 4], [5]].reduce((acc, arr) => [...acc, ...arr], []);
// [1, 2, 3, 4, 5]

// Count occurrences
const words = ["apple", "banana", "apple", "cherry", "banana", "apple"];
const count = words.reduce((acc, word) => {
  acc[word] = (acc[word] || 0) + 1;
  return acc;
}, {});
// { apple: 3, banana: 2, cherry: 1 }
```

---

## Q20. What is `typeof` and when does it behave unexpectedly?
**Difficulty:** Basic
**Type:** Conceptual

### Question
What does `typeof` return for various values? What is the most surprising result?

### Answer
`typeof` returns a string describing the type. The most surprising: `typeof null === "object"` — this is a known bug in JS from its origin.

### Code
```javascript
typeof "hello"      // "string"
typeof 42           // "number"
typeof true         // "boolean"
typeof undefined    // "undefined"
typeof null         // "object"   ← bug!
typeof {}           // "object"
typeof []           // "object"   ← also "object", not "array"
typeof function(){} // "function"
typeof Symbol()     // "symbol"
typeof 42n          // "bigint"

// Better checks
Array.isArray([]);           // true
null === null;               // true
Object.prototype.toString.call([]); // "[object Array]"
```

---

## Q21. What are default parameters in functions?
**Difficulty:** Basic
**Type:** Conceptual

### Question
How do default parameters work in ES6?

### Answer
Default parameters allow function parameters to have **preset values** if no value or `undefined` is passed.

### Code
```javascript
function greet(name = "Guest", greeting = "Hello") {
  return `${greeting}, ${name}!`;
}

greet();                   // "Hello, Guest!"
greet("Alice");            // "Hello, Alice!"
greet("Bob", "Hi");        // "Hi, Bob!"
greet(undefined, "Hey");   // "Hey, Guest!" (undefined triggers default)
greet(null, "Hey");        // "Hey, null!" (null does NOT trigger default)
```

---

## Q22. What is short-circuit evaluation?
**Difficulty:** Basic
**Type:** Conceptual

### Question
Explain short-circuit evaluation with `&&` and `||`. What is the `??` operator?

### Answer
- `||` returns the first **truthy** value (or last value)
- `&&` returns the first **falsy** value (or last value)
- `??` (nullish coalescing) returns the right side only if left is `null` or `undefined`

### Code
```javascript
// || for defaults (has a bug with falsy values)
const port = userInput || 3000;
// If userInput = 0, port = 3000 (wrong!)

// ?? for safer defaults (only null/undefined triggers)
const port2 = userInput ?? 3000;
// If userInput = 0, port2 = 0 (correct!)

// && for conditional execution
const user = { isAdmin: true };
user.isAdmin && performAdminAction(); // only runs if isAdmin is truthy

// Optional chaining ?.
const city = user?.address?.city; // undefined instead of throwing error
```

---

## Intermediate Questions

---

## Q23. Explain prototypal inheritance in JavaScript.
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
What is prototypal inheritance? How does the prototype chain work?

### Answer
Every JavaScript object has a hidden `[[Prototype]]` property pointing to another object. When you access a property, JS traverses the **prototype chain** until it finds it or reaches `null`.

### Code
```javascript
const animal = {
  breathe() { return "breathing"; }
};

const dog = Object.create(animal); // dog's prototype = animal
dog.bark = function() { return "woof"; };

console.log(dog.bark());    // "woof" (own property)
console.log(dog.breathe()); // "breathing" (from prototype)
console.log(dog.hasOwnProperty("bark"));    // true
console.log(dog.hasOwnProperty("breathe")); // false

// ES6 Classes are syntactic sugar over prototypes
class Animal {
  breathe() { return "breathing"; }
}
class Dog extends Animal {
  bark() { return "woof"; }
}
const d = new Dog();
d.bark();    // "woof"
d.breathe(); // "breathing"
```

---

## Q24. What is debouncing and throttling?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
Explain debouncing and throttling. When would you use each?

### Answer
- **Debouncing**: Delays function execution until after a pause in calls. Use for search inputs, window resize.
- **Throttling**: Limits function execution to once per time window. Use for scroll events, button click spam.

### Code
```javascript
// Debounce — waits until user stops typing for 300ms
function debounce(fn, delay) {
  let timer;
  return function(...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), delay);
  };
}

const handleSearch = debounce((query) => {
  fetch(`/search?q=${query}`);
}, 300);

// Throttle — fires at most once every 300ms
function throttle(fn, limit) {
  let lastCall = 0;
  return function(...args) {
    const now = Date.now();
    if (now - lastCall >= limit) {
      lastCall = now;
      fn.apply(this, args);
    }
  };
}

const handleScroll = throttle(() => {
  console.log("Scroll event processed");
}, 300);
```

---

## Q25. What is memoization?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
What is memoization? Implement a simple memoize function.

### Answer
Memoization is an **optimization technique** that caches the results of expensive function calls and returns the cached result when the same inputs occur again.

### Code
```javascript
function memoize(fn) {
  const cache = new Map();
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key);
    }
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

// Expensive function
function fibonacci(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

const memoFib = memoize(fibonacci);
memoFib(40); // slow first time
memoFib(40); // instant (cached)
```

---

## Q26. What is the difference between shallow copy and deep copy?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
What is the difference between shallow and deep copying? How do you deep copy an object?

### Answer
- **Shallow copy**: Copies top-level properties. Nested objects still share references.
- **Deep copy**: Recursively copies all levels. Completely independent.

### Code
```javascript
const original = { a: 1, nested: { b: 2 } };

// Shallow copy (nested still linked)
const shallow = { ...original };
shallow.nested.b = 99;
console.log(original.nested.b); // 99 (mutated!)

// Deep copy options:
// 1. JSON (simple, but loses functions/undefined/Date)
const deep1 = JSON.parse(JSON.stringify(original));

// 2. structuredClone (modern, preferred)
const deep2 = structuredClone(original);

// 3. Recursive function
function deepClone(obj) {
  if (obj === null || typeof obj !== "object") return obj;
  if (Array.isArray(obj)) return obj.map(deepClone);
  return Object.fromEntries(
    Object.entries(obj).map(([k, v]) => [k, deepClone(v)])
  );
}
```

---

## Q27. What are WeakMap and WeakSet?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
What are `WeakMap` and `WeakSet`? How do they differ from `Map` and `Set`?

### Answer
`WeakMap` and `WeakSet` hold **weak references** to objects — if no other reference exists, the object can be garbage collected. They're not iterable and have no `size` property.

**Use case**: Storing metadata about objects without preventing GC (e.g., caching DOM node data).

### Code
```javascript
// WeakMap — keys must be objects
const weakMap = new WeakMap();
let user = { name: "Alice" };
weakMap.set(user, { loggedIn: true });

console.log(weakMap.get(user)); // { loggedIn: true }

user = null; // object can now be garbage collected
// weakMap entry is automatically cleaned up

// Regular Map would hold a reference and prevent GC
const map = new Map();
map.set(user, "data"); // user still referenced by map
```

---

## Q28. Explain `Promise.all`, `Promise.allSettled`, `Promise.race`, and `Promise.any`.
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
When would you use `Promise.all` vs `Promise.allSettled`?

### Answer
| Method              | Resolves When          | Rejects When        |
|---------------------|------------------------|---------------------|
| `Promise.all`       | All resolve            | Any one rejects     |
| `Promise.allSettled`| All settle (any result)| Never               |
| `Promise.race`      | First settles          | First rejects       |
| `Promise.any`       | First resolves         | All reject          |

### Code
```javascript
const p1 = Promise.resolve(1);
const p2 = Promise.resolve(2);
const p3 = Promise.reject(new Error("Failed"));

// all — fails if any fails
Promise.all([p1, p2, p3]).catch(e => console.log(e.message)); // "Failed"

// allSettled — gives status of all
Promise.allSettled([p1, p2, p3]).then(results => {
  results.forEach(r => console.log(r.status, r.value || r.reason));
});
// fulfilled 1, fulfilled 2, rejected Error

// Use case: fetch user + posts + settings in parallel
const [user, posts, settings] = await Promise.all([
  fetchUser(id),
  fetchPosts(id),
  fetchSettings(id)
]);
```

---

## Q29. What is the difference between a module and a script?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
What are ES Modules? How do `import`/`export` differ from CommonJS `require`?

### Answer
| Feature         | ES Modules (`import`)       | CommonJS (`require`)     |
|-----------------|------------------------------|--------------------------|
| Loading         | Static (compile-time)        | Dynamic (runtime)        |
| Execution       | Deferred                     | Synchronous              |
| `this` in top   | `undefined`                  | `module.exports`         |
| File extension  | `.mjs` or `"type":"module"`  | `.js` default in Node    |
| Tree shaking    | ✅ Supported                  | ❌ Not supported          |

### Code
```javascript
// ES Modules
// math.js
export const add = (a, b) => a + b;
export default function multiply(a, b) { return a * b; }

// app.js
import multiply, { add } from "./math.js";

// CommonJS
// math.js
module.exports = { add: (a, b) => a + b };
// app.js
const { add } = require("./math");
```

---

## Q30. What is the Temporal Dead Zone (TDZ)?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
What is the Temporal Dead Zone? Can you demonstrate it?

### Answer
The TDZ is the period from when a `let` or `const` variable **enters scope** until its **declaration is reached**. Accessing the variable during this period throws a `ReferenceError`.

### Code
```javascript
// The TDZ for `x` starts here
try {
  console.log(x); // ReferenceError: Cannot access 'x' before initialization
} catch (e) {
  console.log(e.message);
}
let x = 5; // TDZ ends here

// var has no TDZ — just returns undefined
console.log(y); // undefined
var y = 5;

// TDZ in class fields
class A {
  // Accessing 'b' in the TDZ of 'a' would also fail in certain scenarios
}
```

---

## Q31. How does JavaScript handle errors? Explain try/catch/finally.
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
How does error handling work in JS? What does `finally` guarantee?

### Answer
`try` runs the code. `catch` handles any thrown error. `finally` **always runs** regardless of success or failure — useful for cleanup (closing connections, hiding loaders).

### Code
```javascript
async function fetchUser(id) {
  let connection;
  try {
    connection = await db.connect();
    const user = await connection.query(`SELECT * FROM users WHERE id = $1`, [id]);
    if (!user) throw new Error("User not found");
    return user;
  } catch (err) {
    if (err.message === "User not found") {
      return null; // expected case
    }
    throw err; // re-throw unexpected errors
  } finally {
    connection?.close(); // always clean up
  }
}

// Custom error classes
class NotFoundError extends Error {
  constructor(message) {
    super(message);
    this.name = "NotFoundError";
    this.statusCode = 404;
  }
}
```

---

## Q32. What are generators in JavaScript?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
What is a generator function? When is it useful?

### Answer
Generators are functions that can **pause and resume** execution using `yield`. They return an iterator. Useful for lazy evaluation, infinite sequences, and async flow control.

### Code
```javascript
function* counter(start = 0) {
  while (true) {
    yield start++;
  }
}

const gen = counter(1);
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
console.log(gen.next().value); // 3

// Finite generator
function* range(start, end, step = 1) {
  for (let i = start; i < end; i += step) {
    yield i;
  }
}

[...range(0, 10, 2)]; // [0, 2, 4, 6, 8]
```

---

## Q33. What is `Object.freeze()` vs `const`?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
What is the difference between `const` and `Object.freeze()`?

### Answer
- `const` prevents **re-assignment** of the variable binding, but the object's properties can still be changed.
- `Object.freeze()` makes the object's **properties immutable** (shallow freeze).

### Code
```javascript
const obj = { name: "Alice" };
obj.name = "Bob"; // ✅ Allowed — const only prevents reassignment
// obj = {}; // ❌ TypeError

const frozen = Object.freeze({ name: "Alice", address: { city: "Delhi" } });
frozen.name = "Bob";         // silently ignored (or throws in strict mode)
frozen.address.city = "Mumbai"; // ✅ Allowed! Freeze is shallow

// Deep freeze
function deepFreeze(obj) {
  Object.getOwnPropertyNames(obj).forEach(name => {
    const value = obj[name];
    if (typeof value === "object" && value !== null) deepFreeze(value);
  });
  return Object.freeze(obj);
}
```

---

## Q34. What is event delegation?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
What is event delegation and why is it useful?

### Answer
Event delegation is attaching **one event listener to a parent element** instead of multiple listeners to each child. It works because events **bubble up** the DOM.

**Benefits**: Performance (fewer listeners), works for dynamically added elements.

### Code
```javascript
// ❌ Inefficient — listener on every button
document.querySelectorAll(".btn").forEach(btn => {
  btn.addEventListener("click", handleClick);
});

// ✅ Event delegation — one listener on parent
document.getElementById("button-container").addEventListener("click", (e) => {
  if (e.target.matches(".btn")) {
    handleClick(e);
  }
  // Route to specific handlers
  if (e.target.dataset.action === "delete") {
    deleteItem(e.target.dataset.id);
  }
});
```

---

## Q35. Explain currying in JavaScript.
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
What is currying? Implement a curried function.

### Answer
Currying transforms a function with multiple arguments into a **sequence of functions**, each taking one argument. Enables partial application and function composition.

### Code
```javascript
// Non-curried
const add = (a, b) => a + b;
add(2, 3); // 5

// Curried
const curriedAdd = a => b => a + b;
const add5 = curriedAdd(5);
add5(3); // 8
add5(10); // 15

// General curry utility
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    }
    return function(...moreArgs) {
      return curried.apply(this, args.concat(moreArgs));
    };
  };
}

const multiply = curry((a, b, c) => a * b * c);
multiply(2)(3)(4); // 24
multiply(2, 3)(4); // 24
multiply(2)(3, 4); // 24
```

---

## Q36. What is `Symbol` in JavaScript?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
What is `Symbol`? When would you use one?

### Answer
`Symbol` creates a **guaranteed unique** value. Often used as object property keys to avoid name collisions, or as constants.

### Code
```javascript
const id = Symbol("id");
const id2 = Symbol("id");
console.log(id === id2); // false — always unique

const user = {
  name: "Alice",
  [id]: 12345 // symbol as key — won't clash with string keys
};

console.log(user[id]);    // 12345
console.log(user["id"]);  // undefined

// Symbols are NOT enumerable
console.log(Object.keys(user));        // ["name"]
console.log(Object.getOwnPropertySymbols(user)); // [Symbol(id)]

// Built-in symbols
// Symbol.iterator, Symbol.toPrimitive, Symbol.hasInstance
```

---

## Q37. What is the difference between `for...in` and `for...of`?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
When should you use `for...in` vs `for...of`?

### Answer
- `for...in` — iterates over **enumerable property keys** (strings). Includes inherited properties. Use for objects.
- `for...of` — iterates over **iterable values** (arrays, strings, maps, sets). Does not work on plain objects.

### Code
```javascript
const arr = [10, 20, 30];

for (const index in arr) {
  console.log(index); // "0", "1", "2" (string keys!)
}

for (const value of arr) {
  console.log(value); // 10, 20, 30
}

const obj = { a: 1, b: 2 };
for (const key in obj) {
  console.log(key, obj[key]); // "a" 1, "b" 2
}

// for...of on object throws (not iterable)
// for (const val of obj) {} // TypeError

// Use Object.entries for both
for (const [key, val] of Object.entries(obj)) {
  console.log(key, val);
}
```

---

## Q38. What is the difference between `Map` and a plain object?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
When would you use a `Map` over a plain object `{}`?

### Answer
| Feature         | `Map`              | Plain Object         |
|-----------------|---------------------|----------------------|
| Key types       | Any type            | String/Symbol only   |
| Insertion order | Preserved           | Mostly preserved     |
| Size            | `.size` property    | `Object.keys().length` |
| Iteration       | Directly iterable   | Need `Object.keys()` |
| Prototype keys  | No inherited keys   | Has inherited keys   |
| Performance     | Better for frequent add/delete | Better for static data |

### Code
```javascript
const map = new Map();
map.set("name", "Alice");
map.set(42, "number key");
map.set({ id: 1 }, "object key");
map.set(true, "boolean key");

map.size; // 4
map.has("name"); // true
map.get(42); // "number key"

for (const [key, value] of map) {
  console.log(key, value);
}
```

---

## Q39. What is the Proxy object in JavaScript?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
What is a `Proxy`? Give a practical use case.

### Answer
`Proxy` allows you to **intercept and customize** fundamental operations on objects (get, set, delete, etc.) using **handler traps**.

### Code
```javascript
const handler = {
  get(target, key) {
    return key in target ? target[key] : `Property "${key}" not found`;
  },
  set(target, key, value) {
    if (typeof value !== "number") throw new TypeError("Value must be a number");
    target[key] = value;
    return true;
  }
};

const scores = new Proxy({}, handler);
scores.alice = 95;
console.log(scores.alice);  // 95
console.log(scores.bob);    // "Property "bob" not found"
scores.charlie = "high";    // TypeError: Value must be a number

// Use cases: validation, logging, reactive systems (Vue 3 uses Proxy)
```

---

## Q40. What are getters and setters?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
What are JavaScript getters and setters? Why use them?

### Answer
Getters and setters are **accessor properties** that look like regular properties but execute code when accessed or assigned. They enable computed properties and validation.

### Code
```javascript
const person = {
  _firstName: "John",
  _lastName: "Doe",
  get fullName() {
    return `${this._firstName} ${this._lastName}`;
  },
  set fullName(value) {
    const [first, last] = value.split(" ");
    this._firstName = first;
    this._lastName = last;
  }
};

console.log(person.fullName); // "John Doe"
person.fullName = "Jane Smith";
console.log(person._firstName); // "Jane"

// In classes
class Temperature {
  constructor(celsius) { this._celsius = celsius; }
  get fahrenheit() { return this._celsius * 9/5 + 32; }
  set fahrenheit(f) { this._celsius = (f - 32) * 5/9; }
}

const t = new Temperature(0);
console.log(t.fahrenheit); // 32
t.fahrenheit = 212;
console.log(t._celsius); // 100
```

---

## Q41. What is `Object.assign()` and when does it fall short?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
How does `Object.assign()` work? When would you NOT use it?

### Answer
`Object.assign(target, ...sources)` copies **enumerable own properties** from source objects to the target. It's a **shallow merge** — nested objects are copied by reference.

### Code
```javascript
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const result = Object.assign(target, source);
console.log(result); // { a: 1, b: 4, c: 5 }
console.log(target === result); // true — mutates target!

// Safer: use spread for new object
const merged = { ...target, ...source }; // doesn't mutate

// Limitation — shallow copy
const obj1 = { a: { b: 1 } };
const obj2 = Object.assign({}, obj1);
obj2.a.b = 999;
console.log(obj1.a.b); // 999 — mutated!
```

---

## Q42. What is the difference between function declaration and function expression?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
How do function declarations and expressions differ in terms of hoisting and usage?

### Answer
- **Function declaration**: Hoisted fully (can be called before the line it's defined)
- **Function expression**: Not hoisted (only the variable is, as `undefined`)

### Code
```javascript
// Declaration — hoisted, callable before definition
greet(); // ✅ "Hello"
function greet() { console.log("Hello"); }

// Expression — NOT callable before definition
greet2(); // ❌ TypeError: greet2 is not a function
var greet2 = function() { console.log("Hi"); };

// Named function expression — name only visible inside
const factorial = function fact(n) {
  return n <= 1 ? 1 : n * fact(n - 1); // can call fact() inside
};
// fact(5); // ❌ ReferenceError (not accessible outside)
factorial(5); // ✅ 120
```

---

## Q43. What is the optional chaining operator (`?.`)?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
How does optional chaining work? What problem does it solve?

### Answer
Optional chaining (`?.`) short-circuits and returns `undefined` instead of throwing a `TypeError` when accessing a property on `null` or `undefined`.

### Code
```javascript
const user = {
  name: "Alice",
  address: {
    city: "Delhi"
  }
};

// Without optional chaining
const zip = user && user.address && user.address.zip; // tedious

// With optional chaining
const zip2 = user?.address?.zip;       // undefined (not an error)
const phone = user?.contact?.phone;    // undefined

// On methods
user?.getPreferences?.();  // only calls if method exists

// On arrays
const firstOrder = user?.orders?.[0];

// Combining with nullish coalescing
const city = user?.address?.city ?? "Unknown";
```

---

## Q44. What is the difference between `Object.keys()`, `Object.values()`, and `Object.entries()`?
**Difficulty:** Intermediate
**Type:** Conceptual

### Question
Compare these three Object methods and when would you use each?

### Answer
- `Object.keys(obj)` — returns array of **key strings**
- `Object.values(obj)` — returns array of **values**
- `Object.entries(obj)` — returns array of **[key, value] pairs**

### Code
```javascript
const config = { host: "localhost", port: 3000, debug: true };

Object.keys(config);    // ["host", "port", "debug"]
Object.values(config);  // ["localhost", 3000, true]
Object.entries(config); // [["host","localhost"], ["port",3000], ["debug",true]]

// Practical: transform object values
const doubled = Object.fromEntries(
  Object.entries({ a: 1, b: 2, c: 3 }).map(([k, v]) => [k, v * 2])
);
// { a: 2, b: 4, c: 6 }

// Check if value exists
Object.values(config).includes(3000); // true
```

---

## Practical / Scenario Questions

---

## Q45. You notice a memory leak in a long-running JS app. How would you investigate it?
**Difficulty:** Practical
**Type:** Scenario / Debugging

### Question
A Node.js application's memory usage keeps growing over hours. How would you approach diagnosing and fixing a memory leak?

### Answer
**Investigation steps:**
1. Monitor memory with `process.memoryUsage()` over time
2. Take heap snapshots using Chrome DevTools (or `--inspect` flag)
3. Compare snapshots to find growing object counts
4. Look for: uncleaned event listeners, closures holding references, unbounded caches, global variables, unresolved Promises

### Code
```javascript
// Common leak — event listener not removed
class EventManager {
  constructor(emitter) {
    this.emitter = emitter;
    // ❌ Leak: listener added but never removed
    this.emitter.on("data", this.handleData.bind(this));
  }
  // Fix: add cleanup
  destroy() {
    this.emitter.removeListener("data", this.handleData.bind(this));
    // ❌ Still wrong — bind() creates new function each time!
    // Store reference:
    // this.boundHandler = this.handleData.bind(this);
    // this.emitter.on("data", this.boundHandler);
    // destroy: this.emitter.removeListener("data", this.boundHandler);
  }
}

// Monitor memory
setInterval(() => {
  const { heapUsed, heapTotal } = process.memoryUsage();
  console.log(`Heap: ${Math.round(heapUsed / 1024 / 1024)}MB / ${Math.round(heapTotal / 1024 / 1024)}MB`);
}, 5000);
```

---

## Q46. How would you implement a retry mechanism for a failed API call?
**Difficulty:** Practical
**Type:** Scenario / Coding

### Question
Implement a `fetchWithRetry(url, options, retries, delay)` function that retries on failure with exponential backoff.

### Answer
Retry logic is essential for resilience against transient failures. Exponential backoff avoids overwhelming a struggling server.

### Code
```javascript
async function fetchWithRetry(url, options = {}, retries = 3, delay = 500) {
  for (let attempt = 1; attempt <= retries; attempt++) {
    try {
      const response = await fetch(url, options);
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}: ${response.statusText}`);
      }
      return await response.json();
    } catch (error) {
      console.warn(`Attempt ${attempt} failed: ${error.message}`);
      if (attempt === retries) {
        throw new Error(`All ${retries} attempts failed: ${error.message}`);
      }
      // Exponential backoff: 500ms, 1000ms, 2000ms...
      const backoff = delay * Math.pow(2, attempt - 1);
      await new Promise(resolve => setTimeout(resolve, backoff));
    }
  }
}

// Usage
try {
  const data = await fetchWithRetry("/api/users", {}, 3, 300);
} catch (err) {
  console.error("Final failure:", err.message);
}
```

---

## Q47. What happens when you have multiple `await` statements? How can you optimize them?
**Difficulty:** Practical
**Type:** Scenario / Debugging

### Question
Review this code and identify the performance problem. How would you fix it?

```javascript
async function loadDashboard(userId) {
  const user = await getUser(userId);
  const posts = await getPosts(userId);
  const notifications = await getNotifications(userId);
  return { user, posts, notifications };
}
```

### Answer
The three `await` statements execute **sequentially** — each waits for the previous one to finish. Since they're independent, they should run in **parallel**.

### Code
```javascript
// ❌ Sequential — total time = time(user) + time(posts) + time(notifs)
async function loadDashboard(userId) {
  const user = await getUser(userId);        // waits
  const posts = await getPosts(userId);      // then waits
  const notifications = await getNotifications(userId); // then waits
  return { user, posts, notifications };
}

// ✅ Parallel — total time = max(time(user), time(posts), time(notifs))
async function loadDashboard(userId) {
  const [user, posts, notifications] = await Promise.all([
    getUser(userId),
    getPosts(userId),
    getNotifications(userId)
  ]);
  return { user, posts, notifications };
}

// ✅ With error isolation (if one failing shouldn't break all)
async function loadDashboard(userId) {
  const results = await Promise.allSettled([
    getUser(userId),
    getPosts(userId),
    getNotifications(userId)
  ]);
  return {
    user: results[0].status === "fulfilled" ? results[0].value : null,
    posts: results[1].status === "fulfilled" ? results[1].value : [],
    notifications: results[2].status === "fulfilled" ? results[2].value : []
  };
}
```

---

## Q48. Implement a function to deep compare two objects.
**Difficulty:** Practical
**Type:** Coding

### Question
Write a `deepEqual(a, b)` function that returns `true` if two values are deeply equal.

### Answer
Deep equality requires recursively comparing all nested values, handling arrays, objects, and primitives.

### Code
```javascript
function deepEqual(a, b) {
  // Same reference or primitive equality
  if (a === b) return true;

  // Null check
  if (a === null || b === null) return false;

  // Type check
  if (typeof a !== typeof b) return false;

  // Arrays
  if (Array.isArray(a) && Array.isArray(b)) {
    if (a.length !== b.length) return false;
    return a.every((item, i) => deepEqual(item, b[i]));
  }

  // One is array, other isn't
  if (Array.isArray(a) !== Array.isArray(b)) return false;

  // Objects
  if (typeof a === "object") {
    const keysA = Object.keys(a);
    const keysB = Object.keys(b);
    if (keysA.length !== keysB.length) return false;
    return keysA.every(key => deepEqual(a[key], b[key]));
  }

  return false;
}

// Tests
deepEqual({ a: 1, b: { c: 2 } }, { a: 1, b: { c: 2 } }); // true
deepEqual([1, [2, 3]], [1, [2, 3]]);                        // true
deepEqual({ a: 1 }, { a: 2 });                              // false
deepEqual(null, null);                                       // true
deepEqual(null, undefined);                                  // false
```

---

## Q49. What is an Immediately Invoked Function Expression (IIFE)?
**Difficulty:** Practical
**Type:** Conceptual / Coding

### Question
What is an IIFE? Why was it used before ES modules?

### Answer
An IIFE is a function that is **defined and immediately called**. It creates a private scope, preventing variable pollution of the global namespace. This was the primary pattern before ES modules existed.

### Code
```javascript
// Basic IIFE
(function() {
  const privateVar = "I'm private";
  console.log("IIFE runs immediately");
})();

// console.log(privateVar); // ReferenceError

// Arrow function IIFE
(() => {
  const config = { debug: true };
  // setup code
})();

// IIFE with return value
const counter = (function() {
  let count = 0;
  return {
    increment: () => ++count,
    decrement: () => --count,
    getCount: () => count
  };
})();

counter.increment();
counter.increment();
counter.getCount(); // 2
```

---

## Q50. Fix the bug in this code involving `var` in a loop.
**Difficulty:** Practical
**Type:** Debugging

### Question
What is the output of this code? Why? How do you fix it?

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
```

### Answer
**Output: 3, 3, 3** — not 0, 1, 2 as expected.

Because `var` is function-scoped, all three callbacks share the **same `i`** variable. By the time the timeout fires, the loop has finished and `i === 3`.

### Code
```javascript
// ❌ Bug — var is shared
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 3, 3, 3

// ✅ Fix 1 — use let (block-scoped, new binding per iteration)
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 0, 1, 2

// ✅ Fix 2 — IIFE to capture current value
for (var i = 0; i < 3; i++) {
  ((j) => setTimeout(() => console.log(j), 100))(i);
}
// Output: 0, 1, 2

// ✅ Fix 3 — pass i as argument to setTimeout
for (var i = 0; i < 3; i++) {
  setTimeout(console.log, 100, i);
}
// Output: 0, 1, 2
```

---

## Q51. Implement a simple event emitter (pub/sub pattern).
**Difficulty:** Practical
**Type:** Coding

### Question
Implement a class `EventEmitter` with `on`, `off`, and `emit` methods.

### Answer
The pub/sub pattern decouples producers and consumers of events. Node.js's `EventEmitter` works this way.

### Code
```javascript
class EventEmitter {
  constructor() {
    this.listeners = {};
  }

  on(event, callback) {
    if (!this.listeners[event]) {
      this.listeners[event] = [];
    }
    this.listeners[event].push(callback);
    return this; // chainable
  }

  off(event, callback) {
    if (!this.listeners[event]) return this;
    this.listeners[event] = this.listeners[event].filter(fn => fn !== callback);
    return this;
  }

  emit(event, ...args) {
    if (!this.listeners[event]) return false;
    this.listeners[event].forEach(fn => fn(...args));
    return true;
  }

  once(event, callback) {
    const wrapper = (...args) => {
      callback(...args);
      this.off(event, wrapper);
    };
    return this.on(event, wrapper);
  }
}

// Usage
const emitter = new EventEmitter();
const handler = (msg) => console.log("Received:", msg);

emitter.on("message", handler);
emitter.emit("message", "Hello!"); // "Received: Hello!"
emitter.off("message", handler);
emitter.emit("message", "World!"); // nothing
```

---

## Q52. What will this code output? Explain the output.
**Difficulty:** Practical
**Type:** Debugging

### Question
Predict and explain the output:
```javascript
console.log(1);
setTimeout(() => console.log(2), 0);
Promise.resolve().then(() => console.log(3));
console.log(4);
```

### Answer
**Output: 1 → 4 → 3 → 2**

### Explanation
1. `console.log(1)` — synchronous, runs first
2. `setTimeout(..., 0)` — added to **macro-task queue**
3. `Promise.resolve().then(...)` — added to **microtask queue**
4. `console.log(4)` — synchronous, runs next
5. **Microtasks drain first**: Promise callback → logs `3`
6. **Then macro-tasks**: setTimeout callback → logs `2`

### Code
```javascript
console.log(1);                           // sync → 1
setTimeout(() => console.log(2), 0);      // macro-task queue
Promise.resolve().then(() => console.log(3)); // microtask queue
console.log(4);                           // sync → 4

// Event loop: sync → microtasks → macro-tasks
// Output: 1, 4, 3, 2
```

---

## Q53. Implement `flatten` — flatten a nested array to any depth.
**Difficulty:** Practical
**Type:** Coding

### Question
Write a function `flatten(arr, depth)` without using `Array.flat()`.

### Answer

### Code
```javascript
// Recursive solution
function flatten(arr, depth = Infinity) {
  return arr.reduce((acc, item) => {
    if (Array.isArray(item) && depth > 0) {
      acc.push(...flatten(item, depth - 1));
    } else {
      acc.push(item);
    }
    return acc;
  }, []);
}

flatten([1, [2, [3, [4]], 5]]);       // [1, 2, 3, 4, 5]
flatten([1, [2, [3, [4]], 5]], 1);    // [1, 2, [3, [4]], 5]
flatten([1, [2, [3, [4]], 5]], 2);    // [1, 2, 3, [4], 5]

// Iterative solution (stack-based, no recursion limit)
function flattenIterative(arr) {
  const stack = [...arr];
  const result = [];
  while (stack.length) {
    const item = stack.pop();
    if (Array.isArray(item)) {
      stack.push(...item);
    } else {
      result.unshift(item);
    }
  }
  return result;
}
```

---

## Q54. How would you handle rate limiting in a JavaScript API client?
**Difficulty:** Practical
**Type:** Scenario

### Question
You're building a client that calls a third-party API limited to 10 requests/second. How would you implement a rate limiter on the client side?

### Answer
Use a queue + token bucket or sliding window approach to pace requests.

### Code
```javascript
class RateLimiter {
  constructor(maxRequests, windowMs) {
    this.maxRequests = maxRequests;
    this.windowMs = windowMs;
    this.queue = [];
    this.activeRequests = 0;
    this.requestTimes = [];
  }

  async execute(fn) {
    return new Promise((resolve, reject) => {
      this.queue.push({ fn, resolve, reject });
      this.processQueue();
    });
  }

  processQueue() {
    while (this.queue.length > 0) {
      const now = Date.now();
      // Remove timestamps outside the window
      this.requestTimes = this.requestTimes.filter(t => now - t < this.windowMs);

      if (this.requestTimes.length >= this.maxRequests) {
        const waitTime = this.windowMs - (now - this.requestTimes[0]);
        setTimeout(() => this.processQueue(), waitTime);
        return;
      }

      const { fn, resolve, reject } = this.queue.shift();
      this.requestTimes.push(now);
      fn().then(resolve).catch(reject);
    }
  }
}

const limiter = new RateLimiter(10, 1000); // 10 req/sec

// Wrap your API calls
async function apiCall(endpoint) {
  return limiter.execute(() => fetch(endpoint).then(r => r.json()));
}
```

---

## Q55. Implement a `pipe` function for function composition.
**Difficulty:** Practical
**Type:** Coding

### Question
Implement a `pipe` function that applies functions left-to-right.

### Answer
`pipe(f, g, h)(x)` should equal `h(g(f(x)))`. This is the opposite of `compose` and is common in functional programming.

### Code
```javascript
const pipe = (...fns) => (value) => fns.reduce((acc, fn) => fn(acc), value);

// Example usage
const double = x => x * 2;
const addTen = x => x + 10;
const square = x => x * x;

const transform = pipe(double, addTen, square);
transform(5); // pipe(5): 5*2=10 → 10+10=20 → 20*20=400

// Async pipe
const pipeAsync = (...fns) => (value) =>
  fns.reduce((promise, fn) => promise.then(fn), Promise.resolve(value));

const asyncTransform = pipeAsync(
  async x => x * 2,
  async x => x + 10,
  async x => x * x
);

await asyncTransform(5); // 400

// Real-world usage: middleware-style transformations
const processUser = pipe(
  validateUser,
  normalizeEmail,
  hashPassword,
  saveToDatabase
);
```

---

*End of JavaScript Questions — 55 questions total*
*Next: → [React Questions](../react/questions.md)*
