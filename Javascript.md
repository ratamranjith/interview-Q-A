# Javascript-Interview--questions
# Basic:
## 1. What is the event loop in JavaScript, and how does it work?

The event loop is a mechanism in JavaScript that handles asynchronous operations. It continuously checks the call stack and the callback queue. If the call stack is empty, it pushes the first task from the callback queue into the stack.

### **How It Works:**
1. The call stack executes synchronous code.
2. Asynchronous operations (e.g., setTimeout, Promises, fetch API) are delegated to the browser APIs.
3. Once completed, the callback functions are placed in the event queue.
4. The event loop pushes these callbacks into the call stack when it is empty.

## 2. Explain `this` keyword in different contexts

- **Global Context:** `this` refers to the global object (`window` in browsers, `global` in Node.js).
- **Inside a Function:** In strict mode, `this` is `undefined`. In non-strict mode, it refers to the global object.
- **Inside an Object Method:** `this` refers to the object.
- **Inside a Class Method:** `this` refers to the class instance.
- **Arrow Functions:** `this` is lexically inherited from the surrounding function.

## 3. Difference between `==` and `===`

- `==` (Abstract Equality): Converts the operands to a common type before comparison.
- `===` (Strict Equality): Compares both value and type.

```javascript
console.log(5 == '5'); // true
console.log(5 === '5'); // false
```

## 4. Explain closure and provide an example

A **closure** is a function that remembers the variables from its outer lexical scope even after the outer function has finished execution.

```javascript
function outerFunction(outerVariable) {
    return function innerFunction(innerVariable) {
        console.log(`Outer: ${outerVariable}, Inner: ${innerVariable}`);
    };
}
const newFunction = outerFunction('Hello');
newFunction('World');
// Output: Outer: Hello, Inner: World
```

## 5. What is the difference between `var`, `let`, and `const`?

| Feature | var | let | const |
|---------|-----|-----|-------|
| Scope | Function-scoped | Block-scoped | Block-scoped |
| Hoisting | Hoisted (initialized with `undefined`) | Hoisted but not initialized | Hoisted but not initialized |
| Reassignment | Allowed | Allowed | Not allowed |

## 6. Explain Debouncing and Throttling

- **Debouncing**: Limits the execution of a function until after a specified delay. Useful for handling search input.
- **Throttling**: Ensures a function is executed at most once in a given time interval. Useful for handling scroll events.

```javascript
// Debounce
function debounce(fn, delay) {
    let timer;
    return function (...args) {
        clearTimeout(timer);
        timer = setTimeout(() => fn.apply(this, args), delay);
    };
}

// Throttle
function throttle(fn, interval) {
    let lastTime = 0;
    return function (...args) {
        const now = Date.now();
        if (now - lastTime >= interval) {
            fn.apply(this, args);
            lastTime = now;
        }
    };
}
```

## 7. Explain Promise Chaining and Async/Await with examples

### **Promise Chaining**
```javascript
fetch('https://jsonplaceholder.typicode.com/todos/1')
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.log(error));
```

### **Async/Await**
```javascript
async function fetchData() {
    try {
        let response = await fetch('https://jsonplaceholder.typicode.com/todos/1');
        let data = await response.json();
        console.log(data);
    } catch (error) {
        console.log(error);
    }
}
fetchData();
```

## 8. Implement a deep clone function

```javascript
function deepClone(obj) {
    if (obj === null || typeof obj !== 'object') return obj;
    let clone = Array.isArray(obj) ? [] : {};
    for (let key in obj) {
        if (obj.hasOwnProperty(key)) {
            clone[key] = deepClone(obj[key]);
        }
    }
    return clone;
}
```

## 9. Explain `call`, `apply`, and `bind`

```javascript
const obj = { name: 'John' };
function greet(greeting) {
    console.log(`${greeting}, ${this.name}`);
}
greet.call(obj, 'Hello');  // Hello, John
greet.apply(obj, ['Hi']);   // Hi, John
const boundGreet = greet.bind(obj);
boundGreet('Hey'); // Hey, John
```

## 10. Tricky JavaScript Questions

### **10.1 What will be the output?**
```javascript
console.log([] + []);  // ""
console.log([] + {});  // "[object Object]"
console.log({} + []);  // "[object Object]"
```

### **10.2 Explain the output of this snippet**
```javascript
console.log(0.1 + 0.2 == 0.3);  // false
```
**Explanation**: Due to floating-point precision errors in JavaScript, `0.1 + 0.2` results in `0.30000000000000004`, which is not exactly `0.3`.

### **10.3 What is the output of the following?**
```javascript
console.log(true + false);  // 1
console.log([] == ![]);  // true
```
**Explanation**: `true` is `1`, `false` is `0`, so `true + false = 1`.
For `[] == ![]`, `![]` is `false`, so `[] == false`, which is `true` because `[]` is coerced to an empty string (`''`), and `'' == false` evaluates to `true`.

### **10.4 What will this output?**
```javascript
let a = { x: 1 }, b = { x: 1 };
console.log(a == b);  // false
console.log(a === b); // false
```
**Explanation**: Objects are compared by reference, not by value. `a` and `b` are two different objects in memory, so they are not equal.

### **10.5 What is the output here?**
```javascript
(function() {
    var a = b = 5;
})();
console.log(typeof a); // "undefined"
console.log(typeof b); // "number"
```
**Explanation**: `b = 5` creates a global variable `b`, while `var a = b` is scoped inside the IIFE, so `a` is not accessible globally.

# Tricky JavaScript Problems on Promises, Async/Await, Arrays, Map, Set, and  much more

## 1. Tricky Promise and Async/Await Questions

### **1.1 What will be the output?**
```javascript
console.log('Start');
setTimeout(() => console.log('Timeout'), 0);
Promise.resolve().then(() => console.log('Promise'));
console.log('End');
```
**Output:**
```
Start
End
Promise
Timeout
```
**Explanation:**
- `setTimeout` is placed in the **callback queue** and executed later.
- `Promise.then()` goes into the **microtask queue**, which executes before the callback queue.

---

### **1.2 Can you explain the difference between `Promise.all()`, `Promise.race()`, `Promise.allSettled()`, and `Promise.any()`?**

- `Promise.all(promises)`: Resolves when **all** promises resolve; rejects if any promise rejects.
- `Promise.race(promises)`: Resolves or rejects as soon as the **first** promise settles.
- `Promise.allSettled(promises)`: Resolves when **all** promises settle (either fulfilled or rejected).
- `Promise.any(promises)`: Resolves when **any one** promise resolves; rejects if all promises reject.

---

## 2. Tricky Array Problems

### **2.1 What will be the output?**
```javascript
const arr = [1, 2, 3];
arr[10] = 99;
console.log(arr.length);
console.log(arr);
```
**Output:**
```
11
[1, 2, 3, empty × 7, 99]
```
**Explanation:**
- The array length expands to `11`, but elements `3` to `9` are empty slots.

---

### **2.2 Implement `flatMap` manually**
```javascript
Array.prototype.myFlatMap = function(callback) {
    return this.reduce((acc, curr, index, array) => acc.concat(callback(curr, index, array)), []);
};

console.log([1, 2, 3].myFlatMap(x => [x, x * 2]));
// Output: [1, 2, 2, 4, 3, 6]
```

---

## 3. Tricky Map and Set Questions

### **3.1 What will be the output?**
```javascript
const map = new Map();
map.set('1', 'string key');
map.set(1, 'number key');
console.log(map.get('1'));
console.log(map.get(1));
```
**Output:**
```
string key
number key
```
**Explanation:**
- Maps differentiate between `string` and `number` keys.

---

### **3.2 What happens when you add duplicate values to a Set?**
```javascript
const set = new Set([1, 1, 2, 2, 3, 3]);
console.log(set);
```
**Output:**
```
Set(3) {1, 2, 3}
```
**Explanation:**
- A `Set` only stores unique values.

---

## 4. Tricky Object Questions

### **4.1 What will be the output?**
```javascript
const obj = {};
console.log(obj.toString());
console.log(obj + '');
```
**Output:**
```
[object Object]
[object Object]
```
**Explanation:**
- The default `toString()` method for objects returns `"[object Object]"`.

---

### **4.2 Clone an object without using JSON.stringify**
```javascript
function cloneObject(obj) {
    return Object.assign({}, obj);
}
const obj1 = { a: 1, b: 2 };
const obj2 = cloneObject(obj1);
console.log(obj2);
```

---

## 5. Tricky Function Execution Problems

### **5.1 What will be the output?**
```javascript
(function() {
    var a = b = 5;
})();
console.log(typeof a);
console.log(typeof b);
```
**Output:**
```
undefined
number
```
**Explanation:**
- `b = 5` creates a **global** variable.
- `var a = b` makes `a` function-scoped.

---

### **5.2 How does JavaScript handle function hoisting?**
```javascript
console.log(foo());
function foo() {
    return 'Hello';
}
console.log(bar());
var bar = function() {
    return 'Hi';
};
```
**Output:**
```
Hello
TypeError: bar is not a function
```
**Explanation:**
- Function declarations (`foo`) are hoisted completely.
- Function expressions (`bar`) are hoisted but remain `undefined`.

---

## 6. Tricky Event Loop Questions

### **6.1 What will be the output?**
```javascript
console.log('A');
setTimeout(() => console.log('B'), 0);
Promise.resolve().then(() => console.log('C'));
console.log('D');
```
**Output:**
```
A
D
C
B
```
**Explanation:**
- Synchronous code runs first (`A`, `D`).
- `Promise.then()` is in the microtask queue (`C`).
- `setTimeout` runs last (`B`).

---

## 7. Tricky `this` Keyword Questions

### **7.1 What will be the output?**
```javascript
const obj = {
    value: 42,
    method: function() {
        return this.value;
    }
};
const fn = obj.method;
console.log(fn());
```
**Output:**
```
undefined
```
**Explanation:**
- `fn` is called in the global context where `this` is `undefined` in strict mode.

---

## 8. Tricky Type Coercion Questions

### **8.1 What will be the output?**
```javascript
console.log([] == ![]);
```
**Output:**
```
true
```
**Explanation:**
- `![]` converts to `false`.
- `[] == false` → `[]` coerces to an empty string (`''`).
- `'' == false` is `true`.

---

## 1. Advanced Promise and Async/Await Questions

### **1.1 What will be the output?**
```javascript
console.log('Start');
setTimeout(() => console.log('Timeout'), 0);
Promise.resolve().then(() => console.log('Promise'));
console.log('End');
```
**Output:**
```
Start
End
Promise
Timeout
```
**Explanation:**
- `setTimeout` is placed in the **callback queue** and executed later.
- `Promise.then()` goes into the **microtask queue**, which executes before the callback queue.

---

### **1.2 Implement a function to execute promises in sequence**
```javascript
async function runInSequence(tasks) {
    for (const task of tasks) {
        console.log(await task());
    }
}

const task1 = () => Promise.resolve('Task 1 done');
const task2 = () => new Promise(res => setTimeout(() => res('Task 2 done'), 1000));
const task3 = () => Promise.resolve('Task 3 done');

runInSequence([task1, task2, task3]);
```
**Explanation:**
- This ensures that each promise is executed only after the previous one completes.

---

### **1.3 Implement a Promise timeout mechanism**
```javascript
function promiseWithTimeout(promise, ms) {
    let timeout = new Promise((_, reject) => setTimeout(() => reject(new Error('Timeout')), ms));
    return Promise.race([promise, timeout]);
}

const slowTask = new Promise(res => setTimeout(() => res('Slow task done'), 3000));

promiseWithTimeout(slowTask, 2000).then(console.log).catch(console.error);
```
**Explanation:**
- `Promise.race()` ensures that whichever promise settles first (either the actual task or the timeout) determines the result.

---

## 2. Advanced Array Operations

### **2.1 Implement a function to remove duplicates without using Set**
```javascript
function removeDuplicates(arr) {
    return arr.filter((item, index) => arr.indexOf(item) === index);
}
console.log(removeDuplicates([1, 2, 2, 3, 4, 4, 5]));
```

---

### **2.2 Implement a function to chunk an array**
```javascript
function chunkArray(arr, size) {
    return arr.reduce((acc, _, i) => (i % size ? acc : [...acc, arr.slice(i, i + size)]), []);
}
console.log(chunkArray([1, 2, 3, 4, 5, 6, 7], 3));
```

---

## 3. Advanced Event Loop and Concurrency

### **3.1 What will be the output?**
```javascript
console.log('A');
setTimeout(() => console.log('B'), 0);
Promise.resolve().then(() => console.log('C'));
console.log('D');
```
**Output:**
```
A
D
C
B
```
**Explanation:**
- Synchronous code runs first (`A` and `D`).
- Promises (microtasks) run next (`C`).
- Finally, `setTimeout` (macro task) runs (`B`).

---

## 4. Advanced `this` Keyword and Prototypes

### **4.1 What will be the output?**
```javascript
function Person(name) {
    this.name = name;
    return { name: 'Override' };
}
const person = new Person('John');
console.log(person.name);
```
**Output:**
```
Override
```
**Explanation:**
- Since the constructor explicitly returns an object, it overrides `this`.

---

### **4.2 Create a custom method on `Function.prototype`**
```javascript
Function.prototype.defer = function(ms) {
    let fn = this;
    return function(...args) {
        setTimeout(() => fn.apply(this, args), ms);
    };
};

function greet() {
    console.log('Hello!');
}

greet.defer(1000)();
```
**Explanation:**
- `Function.prototype.defer` delays execution of any function by the specified time.

---

## 5. Advanced Functional Programming Concepts

### **5.1 Implement a Memoization function**
```javascript
function memoize(fn) {
    const cache = new Map();
    return function (...args) {
        const key = JSON.stringify(args);
        if (cache.has(key)) return cache.get(key);
        const result = fn(...args);
        cache.set(key, result);
        return result;
    };
}

const fib = memoize(n => (n <= 1 ? n : fib(n - 1) + fib(n - 2)));
console.log(fib(10));
```

---

## 6. Advanced Web APIs & Multithreading

### **6.1 Using Web Workers for parallel execution**
```javascript
// worker.js
self.onmessage = function(e) {
    let sum = 0;
    for (let i = 0; i < 1e9; i++) sum += i;
    self.postMessage(sum);
};

// main.js
const worker = new Worker('worker.js');
worker.onmessage = e => console.log('Worker result:', e.data);
worker.postMessage('start');
```
**Explanation:**
- Web Workers allow computation-heavy tasks to run on separate threads.

---

## 7. JavaScript Security & Performance Optimization

### **7.1 Prevent memory leaks using WeakMap**
```javascript
const cache = new WeakMap();
function fetchData(obj) {
    if (!cache.has(obj)) cache.set(obj, { data: 'Expensive data' });
    return cache.get(obj);
}
let obj = {};
fetchData(obj);
console.log(cache.has(obj)); // true
obj = null; // Now WeakMap allows garbage collection
```
**Explanation:**
- `WeakMap` holds weak references, preventing memory leaks.

---

### **7.2 Avoid synchronous reflows in the DOM**
```javascript
document.body.style.width = '100px';
const width = document.body.offsetWidth; // Forces reflow
```
**Optimization:**
```javascript
document.body.style.width = '100px';
requestAnimationFrame(() => {
    const width = document.body.offsetWidth;
});
```
**Explanation:**
- `requestAnimationFrame` batches UI updates, preventing layout thrashing.

---

