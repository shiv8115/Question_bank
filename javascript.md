# JavaScript Interview Questions Bank
**Chunk 1 of 6** | **70 Questions Total** | **Easy → Intermediate → Advanced**

---

## 🟢 EASY LEVEL (Questions 1-25)

### **Q1: What are the different data types in JavaScript?**
**Answer:** 
- **Primitive:** `string`, `number`, `boolean`, `undefined`, `null`, `symbol`, `bigint`
- **Non-primitive:** `object` (includes arrays, functions, dates, etc.)

### **Q2: What's the difference between `var`, `let`, and `const`?**
**Answer:**
- `var`: Function-scoped, hoisted, can be redeclared
- `let`: Block-scoped, hoisted but not initialized, cannot be redeclared
- `const`: Block-scoped, must be initialized, cannot be reassigned

### **Q3: Explain hoisting in JavaScript.**
**Answer:** Variables and function declarations are moved to the top of their scope. `var` is initialized with `undefined`, `let`/`const` are in temporal dead zone.

```javascript
console.log(x); // undefined (not error)
var x = 5;

console.log(y); // ReferenceError
let y = 10;
```

### **Q4: What is the difference between `==` and `===`?**
**Answer:**
- `==`: Loose equality, performs type coercion
- `===`: Strict equality, checks type and value

```javascript
'5' == 5   // true
'5' === 5  // false
```

### **Q5: How do you check if a variable is an array?**
**Answer:**
```javascript
Array.isArray(arr)           // Best method
arr instanceof Array         // Works in same context
Object.prototype.toString.call(arr) === '[object Array]'
```

### **Q6: What is the difference between `null` and `undefined`?**
**Answer:**
- `undefined`: Variable declared but not assigned
- `null`: Intentional absence of value

### **Q7: How do you clone an object in JavaScript?**
**Answer:**
```javascript
// Shallow clone
const clone = { ...obj };
const clone2 = Object.assign({}, obj);

// Deep clone
const deepClone = JSON.parse(JSON.stringify(obj)); // Limited
const deepClone2 = structuredClone(obj); // Modern approach
```

### **Q8: What are template literals?**
**Answer:** String literals with embedded expressions using backticks:
```javascript
const name = 'John';
const message = `Hello, ${name}! Today is ${new Date().toDateString()}`;
```

### **Q9: Explain the `this` keyword.**
**Answer:** `this` refers to the object that is executing the current function:
- Method: `this` = owner object
- Function: `this` = global object (or undefined in strict mode)
- Arrow function: `this` = lexically inherited

### **Q10: What is a callback function?**
**Answer:** A function passed as an argument to another function:
```javascript
function greet(name, callback) {
  console.log('Hello ' + name);
  callback();
}
```

### **Q11: How do you convert a string to a number?**
**Answer:**
```javascript
Number('123')    // 123
parseInt('123')  // 123
parseFloat('123.45') // 123.45
+'123'          // 123 (unary plus)
```

### **Q12: What is the difference between `for...in` and `for...of`?**
**Answer:**
- `for...in`: Iterates over enumerable properties (keys)
- `for...of`: Iterates over iterable values

```javascript
const arr = ['a', 'b', 'c'];
for (let i in arr) console.log(i);    // 0, 1, 2
for (let val of arr) console.log(val); // a, b, c
```

### **Q13: How do you add/remove elements from an array?**
**Answer:**
```javascript
// Add
arr.push(item)     // End
arr.unshift(item)  // Beginning
arr.splice(index, 0, item) // At index

// Remove
arr.pop()          // Last
arr.shift()        // First
arr.splice(index, 1) // At index
```

### **Q14: What is event bubbling?**
**Answer:** When an event occurs on an element, it first runs handlers on it, then on its parent, then ancestors.

### **Q15: How do you stop event propagation?**
**Answer:**
```javascript
event.stopPropagation()     // Stops bubbling
event.stopImmediatePropagation() // Stops all handlers
event.preventDefault()      // Prevents default action
```

### **Q16: What are arrow functions?**
**Answer:** Shorter syntax for functions with lexical `this` binding:
```javascript
const add = (a, b) => a + b;
const square = x => x * x;
```

### **Q17: How do you handle errors in JavaScript?**
**Answer:**
```javascript
try {
  // Code that might throw
} catch (error) {
  // Handle error
} finally {
  // Always executes
}
```

### **Q18: What is the difference between `forEach` and `map`?**
**Answer:**
- `forEach`: Executes function for each element, returns `undefined`
- `map`: Creates new array with results of calling function

### **Q19: How do you check if an object has a property?**
**Answer:**
```javascript
obj.hasOwnProperty('prop')
'prop' in obj
obj.prop !== undefined
Object.hasOwn(obj, 'prop') // Modern
```

### **Q20: What is the spread operator?**
**Answer:** `...` expands iterables or objects:
```javascript
const arr = [1, 2, 3];
const newArr = [...arr, 4, 5]; // [1, 2, 3, 4, 5]
const obj = { ...otherObj, newProp: 'value' };
```

### **Q21: How do you merge two arrays?**
**Answer:**
```javascript
const merged = [...arr1, ...arr2];
const merged2 = arr1.concat(arr2);
```

### **Q22: What is destructuring?**
**Answer:** Extract values from arrays/objects:
```javascript
const [a, b] = [1, 2];
const { name, age } = person;
```

### **Q23: How do you find an element in an array?**
**Answer:**
```javascript
arr.find(item => item.id === 1)      // First match
arr.findIndex(item => item.id === 1) // Index
arr.includes(value)                  // Boolean
arr.indexOf(value)                   // Index or -1
```

### **Q24: What is the ternary operator?**
**Answer:** Shorthand for if-else:
```javascript
const result = condition ? valueIfTrue : valueIfFalse;
```

### **Q25: How do you convert an object to JSON?**
**Answer:**
```javascript
JSON.stringify(obj)  // Object to JSON string
JSON.parse(jsonStr)  // JSON string to object
```

---

## 🟡 INTERMEDIATE LEVEL (Questions 26-50)

### **Q26: Explain closures with an example.**
**Answer:** Inner function has access to outer function's variables:
```javascript
function outer(x) {
  return function inner(y) {
    return x + y; // inner can access x
  };
}
const addFive = outer(5);
console.log(addFive(3)); // 8
```

### **Q27: What is the difference between call, apply, and bind?**
**Answer:**
```javascript
// call: invoke immediately with arguments
func.call(thisArg, arg1, arg2);

// apply: invoke immediately with array of arguments
func.apply(thisArg, [arg1, arg2]);

// bind: return new function with bound this
const boundFunc = func.bind(thisArg, arg1);
```

### **Q28: Explain prototypal inheritance.**
**Answer:** Objects can inherit properties from other objects via prototype chain:
```javascript
function Person(name) {
  this.name = name;
}
Person.prototype.greet = function() {
  return `Hello, I'm ${this.name}`;
};

const john = new Person('John');
john.greet(); // "Hello, I'm John"
```

### **Q29: What are Promises and how do they work?**
**Answer:** Objects representing eventual completion of async operations:
```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve('Success!'), 1000);
});

promise.then(result => console.log(result))
       .catch(error => console.error(error));
```

### **Q30: Explain async/await.**
**Answer:** Syntactic sugar for Promises:
```javascript
async function fetchData() {
  try {
    const response = await fetch('/api/data');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error:', error);
  }
}
```

### **Q31: What is event delegation?**
**Answer:** Handle events on parent instead of individual children:
```javascript
document.getElementById('list').addEventListener('click', (e) => {
  if (e.target.tagName === 'LI') {
    console.log('List item clicked:', e.target.textContent);
  }
});
```

### **Q32: How do you deep clone an object?**
**Answer:**
```javascript
// Method 1: JSON (limited)
const clone = JSON.parse(JSON.stringify(obj));

// Method 2: Recursive function
function deepClone(obj) {
  if (obj === null || typeof obj !== 'object') return obj;
  if (obj instanceof Date) return new Date(obj);
  if (obj instanceof Array) return obj.map(item => deepClone(item));
  
  const cloned = {};
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      cloned[key] = deepClone(obj[key]);
    }
  }
  return cloned;
}
```

### **Q33: What is debouncing and throttling?**
**Answer:**
```javascript
// Debouncing: Delay execution until after calls have stopped
function debounce(func, delay) {
  let timeoutId;
  return function(...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => func.apply(this, args), delay);
  };
}

// Throttling: Limit execution to once per interval
function throttle(func, delay) {
  let lastCall = 0;
  return function(...args) {
    const now = Date.now();
    if (now - lastCall >= delay) {
      lastCall = now;
      func.apply(this, args);
    }
  };
}
```

### **Q34: Explain the module pattern.**
**Answer:**
```javascript
const myModule = (function() {
  let privateVar = 0;
  
  function privateFunction() {
    console.log('Private function');
  }
  
  return {
    publicMethod() {
      privateVar++;
      return privateVar;
    },
    get count() {
      return privateVar;
    }
  };
})();
```

### **Q35: What are generators?**
**Answer:** Functions that can pause and resume execution:
```javascript
function* countGenerator() {
  let count = 0;
  while (true) {
    yield count++;
  }
}

const counter = countGenerator();
console.log(counter.next().value); // 0
console.log(counter.next().value); // 1
```

### **Q36: How do you handle multiple async operations?**
**Answer:**
```javascript
// Parallel execution
const [result1, result2] = await Promise.all([
  fetch('/api/data1'),
  fetch('/api/data2')
]);

// Sequential execution
const result1 = await fetch('/api/data1');
const result2 = await fetch('/api/data2');

// Handle failures
const results = await Promise.allSettled([
  fetch('/api/data1'),
  fetch('/api/data2')
]);
```

### **Q37: What is currying?**
**Answer:** Transform function with multiple arguments into sequence of functions:
```javascript
// Regular function
function add(a, b, c) {
  return a + b + c;
}

// Curried function
function curriedAdd(a) {
  return function(b) {
    return function(c) {
      return a + b + c;
    };
  };
}

const add5 = curriedAdd(5);
const add5And3 = add5(3);
console.log(add5And3(2)); // 10
```

### **Q38: Explain memoization.**
**Answer:** Cache function results to avoid recalculation:
```javascript
function memoize(fn) {
  const cache = {};
  return function(...args) {
    const key = JSON.stringify(args);
    if (key in cache) {
      return cache[key];
    }
    const result = fn.apply(this, args);
    cache[key] = result;
    return result;
  };
}

const expensiveFunction = memoize((n) => {
  // Expensive calculation
  return n * n;
});
```

### **Q39: What is the difference between synchronous and asynchronous code?**
**Answer:**
- **Synchronous:** Executes line by line, blocks until complete
- **Asynchronous:** Non-blocking, allows other code to run

### **Q40: How do you create a custom error?**
**Answer:**
```javascript
class CustomError extends Error {
  constructor(message, code) {
    super(message);
    this.name = 'CustomError';
    this.code = code;
  }
}

throw new CustomError('Something went wrong', 'E001');
```

### **Q41: What are WeakMap and WeakSet?**
**Answer:**
- **WeakMap:** Keys must be objects, no size property, not enumerable
- **WeakSet:** Values must be objects, garbage collected when no references

```javascript
const weakMap = new WeakMap();
const obj = {};
weakMap.set(obj, 'value');
// When obj is garbage collected, entry is removed
```

### **Q42: Explain the concept of hoisting with functions.**
**Answer:**
```javascript
// Function declarations are fully hoisted
console.log(foo()); // "Hello" - works

function foo() {
  return "Hello";
}

// Function expressions are not hoisted
console.log(bar()); // TypeError: bar is not a function

var bar = function() {
  return "World";
};
```

### **Q43: What is the difference between `Object.freeze()`, `Object.seal()`, and `Object.preventExtensions()`?**
**Answer:**
- `Object.freeze()`: Cannot add, remove, or modify properties
- `Object.seal()`: Cannot add or remove properties, can modify existing
- `Object.preventExtensions()`: Cannot add properties, can remove/modify

### **Q44: How do you implement inheritance in JavaScript?**
**Answer:**
```javascript
// ES6 Classes
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(`${this.name} makes a sound`);
  }
}

class Dog extends Animal {
  speak() {
    console.log(`${this.name} barks`);
  }
}

// Prototype-based
function Animal(name) {
  this.name = name;
}
Animal.prototype.speak = function() {
  console.log(`${this.name} makes a sound`);
};

function Dog(name) {
  Animal.call(this, name);
}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;
```

### **Q45: What is the event loop?**
**Answer:** Mechanism that handles execution of code, events, and callbacks:
1. Call stack executes synchronous code
2. Web APIs handle async operations
3. Callback queue holds completed callbacks
4. Event loop moves callbacks from queue to stack when stack is empty

### **Q46: How do you handle race conditions in JavaScript?**
**Answer:**
```javascript
let latestRequestId = 0;

async function searchWithRaceConditionHandling(query) {
  const requestId = ++latestRequestId;
  
  try {
    const results = await fetch(`/search?q=${query}`);
    
    // Only process if this is still the latest request
    if (requestId === latestRequestId) {
      return await results.json();
    }
  } catch (error) {
    if (requestId === latestRequestId) {
      throw error;
    }
  }
}
```

### **Q47: What are Sets and Maps?**
**Answer:**
```javascript
// Set: Collection of unique values
const mySet = new Set([1, 2, 3, 3]); // [1, 2, 3]
mySet.add(4);
mySet.has(2); // true

// Map: Key-value pairs with any type of keys
const myMap = new Map();
myMap.set('string', 'value');
myMap.set(1, 'number key');
myMap.set(true, 'boolean key');
```

### **Q48: How do you implement a simple observer pattern?**
**Answer:**
```javascript
class EventEmitter {
  constructor() {
    this.events = {};
  }
  
  on(event, callback) {
    if (!this.events[event]) {
      this.events[event] = [];
    }
    this.events[event].push(callback);
  }
  
  emit(event, data) {
    if (this.events[event]) {
      this.events[event].forEach(callback => callback(data));
    }
  }
  
  off(event, callback) {
    if (this.events[event]) {
      this.events[event] = this.events[event].filter(cb => cb !== callback);
    }
  }
}
```

### **Q49: What is the difference between arrow functions and regular functions?**
**Answer:**
- **Arrow functions:** Lexical `this`, no `arguments` object, cannot be constructors
- **Regular functions:** Dynamic `this`, have `arguments`, can be constructors

### **Q50: How do you implement a polyfill for `Array.prototype.map`?**
**Answer:**
```javascript
Array.prototype.myMap = function(callback, thisArg) {
  if (this == null) {
    throw new TypeError('Array.prototype.map called on null or undefined');
  }
  
  const result = [];
  const O = Object(this);
  const len = parseInt(O.length) || 0;
  
  for (let i = 0; i < len; i++) {
    if (i in O) {
      result[i] = callback.call(thisArg, O[i], i, O);
    }
  }
  
  return result;
};
```

---

## 🔴 ADVANCED/TRICKY LEVEL (Questions 51-70)

### **Q51: Implement a function that flattens a nested array to any depth.**
**Answer:**
```javascript
function flattenDeep(arr) {
  return arr.reduce((acc, val) => 
    Array.isArray(val) ? acc.concat(flattenDeep(val)) : acc.concat(val), []
  );
}

// Or using modern flat method
const flattened = arr.flat(Infinity);
```

### **Q52: Create a function that can bind multiple functions together (composition).**
**Answer:**
```javascript
const compose = (...fns) => (value) => fns.reduceRight((acc, fn) => fn(acc), value);

const pipe = (...fns) => (value) => fns.reduce((acc, fn) => fn(acc), value);

// Usage
const addOne = x => x + 1;
const double = x => x * 2;
const square = x => x * x;

const composedFn = compose(square, double, addOne);
console.log(composedFn(3)); // square(double(addOne(3))) = square(8) = 64
```

### **Q53: Implement a retry mechanism with exponential backoff.**
**Answer:**
```javascript
async function retryWithBackoff(fn, maxRetries = 3, baseDelay = 1000) {
  for (let attempt = 0; attempt <= maxRetries; attempt++) {
    try {
      return await fn();
    } catch (error) {
      if (attempt === maxRetries) {
        throw error;
      }
      
      const delay = baseDelay * Math.pow(2, attempt);
      await new Promise(resolve => setTimeout(resolve, delay));
    }
  }
}

// Usage
const fetchData = () => fetch('/api/data').then(res => res.json());
const data = await retryWithBackoff(fetchData, 3, 500);
```

### **Q54: How would you implement a simple state management system?**
**Answer:**
```javascript
class Store {
  constructor(initialState = {}) {
    this.state = initialState;
    this.listeners = [];
  }
  
  getState() {
    return { ...this.state };
  }
  
  setState(newState) {
    this.state = { ...this.state, ...newState };
    this.listeners.forEach(listener => listener(this.state));
  }
  
  subscribe(listener) {
    this.listeners.push(listener);
    return () => {
      this.listeners = this.listeners.filter(l => l !== listener);
    };
  }
}

// Usage
const store = new Store({ count: 0 });
const unsubscribe = store.subscribe(state => console.log('State:', state));
store.setState({ count: 1 });
```

### **Q55: Implement a LRU (Least Recently Used) cache.**
**Answer:**
```javascript
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.cache = new Map();
  }
  
  get(key) {
    if (this.cache.has(key)) {
      const value = this.cache.get(key);
      this.cache.delete(key);
      this.cache.set(key, value);
      return value;
    }
    return -1;
  }
  
  put(key, value) {
    if (this.cache.has(key)) {
      this.cache.delete(key);
    } else if (this.cache.size >= this.capacity) {
      const firstKey = this.cache.keys().next().value;
      this.cache.delete(firstKey);
    }
    this.cache.set(key, value);
  }
}
```

### **Q56: Create a deep comparison function for objects.**
**Answer:**
```javascript
function deepEqual(a, b) {
  if (a === b) return true;
  
  if (a == null || b == null) return false;
  
  if (typeof a !== typeof b) return false;
  
  if (typeof a !== 'object') return false;
  
  const keysA = Object.keys(a);
  const keysB = Object.keys(b);
  
  if (keysA.length !== keysB.length) return false;
  
  for (let key of keysA) {
    if (!keysB.includes(key)) return false;
    if (!deepEqual(a[key], b[key])) return false;
  }
  
  return true;
}
```

### **Q57: Implement a function that converts callback-based functions to Promises.**
**Answer:**
```javascript
function promisify(fn) {
  return function(...args) {
    return new Promise((resolve, reject) => {
      fn(...args, (error, result) => {
        if (error) {
          reject(error);
        } else {
          resolve(result);
        }
      });
    });
  };
}

// Usage
const fs = require('fs');
const readFileAsync = promisify(fs.readFile);
const data = await readFileAsync('file.txt', 'utf8');
```

### **Q58: Create a scheduler that can limit concurrent async operations.**
**Answer:**
```javascript
class AsyncScheduler {
  constructor(concurrency = 1) {
    this.concurrency = concurrency;
    this.running = 0;
    this.queue = [];
  }
  
  async add(asyncFn) {
    return new Promise((resolve, reject) => {
      this.queue.push({
        asyncFn,
        resolve,
        reject
      });
      
      this.process();
    });
  }
  
  async process() {
    if (this.running >= this.concurrency || this.queue.length === 0) {
      return;
    }
    
    this.running++;
    const { asyncFn, resolve, reject } = this.queue.shift();
    
    try {
      const result = await asyncFn();
      resolve(result);
    } catch (error) {
      reject(error);
    } finally {
      this.running--;
      this.process();
    }
  }
}
```

### **Q59: Implement a reactive system (similar to Vue's reactivity).**
**Answer:**
```javascript
let activeEffect = null;

class Reactive {
  constructor(target) {
    this.deps = new Map();
    
    return new Proxy(target, {
      get: (target, key) => {
        this.track(key);
        return target[key];
      },
      
      set: (target, key, value) => {
        target[key] = value;
        this.trigger(key);
        return true;
      }
    });
  }
  
  track(key) {
    if (activeEffect) {
      if (!this.deps.has(key)) {
        this.deps.set(key, new Set());
      }
      this.deps.get(key).add(activeEffect);
    }
  }
  
  trigger(key) {
    if (this.deps.has(key)) {
      this.deps.get(key).forEach(effect => effect());
    }
  }
}

function effect(fn) {
  activeEffect = fn;
  fn();
  activeEffect = null;
}

// Usage
const data = new Reactive({ count: 0 });
effect(() => console.log('Count:', data.count));
data.count = 1; // Logs: Count: 1
```

### **Q60: How would you implement a virtual DOM diff algorithm?**
**Answer:**
```javascript
function diff(oldNode, newNode) {
  const patches = [];
  
  if (!newNode) {
    patches.push({ type: 'REMOVE', node: oldNode });
  } else if (!oldNode) {
    patches.push({ type: 'CREATE', node: newNode });
  } else if (oldNode.type !== newNode.type) {
    patches.push({ type: 'REPLACE', oldNode, newNode });
  } else {
    // Diff attributes
    const attrPatches = diffAttributes(oldNode.props, newNode.props);
    if (attrPatches.length > 0) {
      patches.push({ type: 'PROPS', patches: attrPatches });
    }
    
    // Diff children
    const childPatches = diffChildren(oldNode.children, newNode.children);
    patches.push(...childPatches);
  }
  
  return patches;
}

function diffAttributes(oldProps, newProps) {
  const patches = [];
  
  // Check for changed/removed props
  for (let key in oldProps) {
    if (!(key in newProps)) {
      patches.push({ type: 'REMOVE_PROP', key });
    } else if (oldProps[key] !== newProps[key]) {
      patches.push({ type: 'SET_PROP', key, value: newProps[key] });
    }
  }
  
  // Check for new props
  for (let key in newProps) {
    if (!(key in oldProps)) {
      patches.push({ type: 'SET_PROP', key, value: newProps[key] });
    }
  }
  
  return patches;
}
```

### **Q61: Create a function that implements lazy evaluation.**
**Answer:**
```javascript
class Lazy {
  constructor(fn) {
    this.fn = fn;
    this.evaluated = false;
    this.value = undefined;
  }
  
  get() {
    if (!this.evaluated) {
      this.value = this.fn();
      this.evaluated = true;
    }
    return this.value;
  }
  
  map(fn) {
    return new Lazy(() => fn(this.get()));
  }
  
  filter(predicate) {
    return new Lazy(() => {
      const value = this.get();
      return predicate(value) ? value : undefined;
    });
  }
}

// Usage
const lazyValue = new Lazy(() => {
  console.log('Computing expensive operation...');
  return Math.random() * 1000;
});

const doubled = lazyValue.map(x => x * 2);
console.log(doubled.get()); // Only computed when accessed
```

### **Q62: Implement a publish-subscribe pattern with namespaces.**
**Answer:**
```javascript
class PubSub {
  constructor() {
    this.events = {};
  }
  
  subscribe(event, callback, namespace = 'default') {
    const fullEvent = `${namespace}:${event}`;
    
    if (!this.events[fullEvent]) {
      this.events[fullEvent] = [];
    }
    
    this.events[fullEvent].push(callback);
    
    return () => {
      this.events[fullEvent] = this.events[fullEvent].filter(cb => cb !== callback);
    };
  }
  
  publish(event, data, namespace = 'default') {
    const fullEvent = `${namespace}:${event}`;
    
    if (this.events[fullEvent]) {
      this.events[fullEvent].forEach(callback => callback(data));
    }
  }
  
  unsubscribeNamespace(namespace) {
    Object.keys(this.events).forEach(event => {
      if (event.startsWith(`${namespace}:`)) {
        delete this.events[event];
      }
    });
  }
}
```

### **Q63: Create a function that implements function overloading.**
**Answer:**
```javascript
function createOverload() {
  const fnMap = new Map();
  
  function overloadedFunction(...args) {
    const key = args.map(arg => typeof arg).join(',');
    
    if (fnMap.has(key)) {
      return fnMap.get(key).apply(this, args);
    }
    
    throw new Error(`No overload found for signature: ${key}`);
  }
  
  overloadedFunction.addOverload = function(types, fn) {
    const key = types.join(',');
    fnMap.set(key, fn);
    return this;
  };
  
  return overloadedFunction;
}

// Usage
const myFunction = createOverload()
  .addOverload(['string'], (str) => `Hello, ${str}!`)
  .addOverload(['number'], (num) => num * 2)
  .addOverload(['string', 'number'], (str, num) => `${str}: ${num}`);

console.log(myFunction('World')); // "Hello, World!"
console.log(myFunction(5)); // 10
console.log(myFunction('Count', 42)); // "Count: 42"
```
