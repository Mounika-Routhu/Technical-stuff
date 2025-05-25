## INTRO to JS
1. JavaScript (often abbreviated as JS) is a high-level, interpreted programming language that was created to enable **dynamic interaction** on websites.
2. Year of Introduction: JavaScript was introduced in **1995**.
3. Creator: It was created by **Brendan Eich**, a programmer at **Netscape Communications Corporation.**
4. Original Name: Initially, the language was called **Mocha**, then it was briefly named **LiveScript**, and finally, it was renamed to **JavaScript** as a marketing strategy to capitalize on the popularity of Java at the time (though the two languages are very different).
5. **high-level:** closer to human languages (like English) than to machine code. Developer friendly - to write & read
6. **interpreted**: not compiled beforehand but rather interpreted(executed) line-by-line at runtime by the JS engine (e.g., V8 in Chrome, SpiderMonkey in Firefox).
8. **Dynamic**: don’t need to declare the type of a variable when you create it. The type is determined at runtime - flexible can be restricted with const
9. A **scripting language** is a programming language designed for automating tasks and controlling software, typically executed by an interpreter rather than being compiled.
   JavaScript (JS) is considered a scripting language, as it is primarily used for automating tasks, manipulating web pages, and controlling web browsers, and it is executed by an interpreter (the browser) rather than being compiled.
12. **single threaded**: describes **how** JavaScript runs code: There is only one thread(single call stack), so only one piece of code runs at a time.
13. **Synchronous**: describes **when** JavaScript runs code: Each line runs one after another, waiting for the previous to complete

   Imagine a cashier:
   1. Single-threaded = only one cashier open (one task at a time).
   2. Synchronous = each customer waits in line until the previous one is done.
   
   JavaScript is single-threaded by design, and it runs synchronously by default, but you can make it asynchronous with tools like `setTimeout`, `Promise`, or `async/await`.

11. **Loosely Coupled**: Loosely coupled refers to a design principle where different parts of code are independent of each other, changes made in one part of the system won't heavily affect other parts, making it more flexible, maintainable, and easier to update.

```JS
const button = document.querySelector('button');

// The function to be triggered when the button is clicked
const showAlert = () => alert('Button clicked!');

// The button is loosely coupled to the function, it just knows to trigger showAlert when clicked
// button doesn't care(doesn't know) about the functionality of function showAlert
button.addEventListener('click', showAlert);
```
 JS can be written as tightly coupled
 ```JS
 // Tightly coupled example
const button = document.querySelector('button');
const alertMessage = document.querySelector('.message');

button.addEventListener('click', () => {
    alertMessage.innerText = 'Button clicked!'; // Directly updates a specific part of the page
    alert('Button clicked!');
});
```
1. The button not only triggers an alert but also changes the text in a specific element (.message). If the .message element changes, you’ll have to modify this code.
2. The button and the message element are tightly bound together in this example.

**compilation** By definition it is the process of transforming a program written in a high-level programming language (like C, Java, or Python) into machine code or bytecode that a computer can understand and execute.
For example:
1. In C, you might write hello.c. The compiler turns this into a hello.exe or equivalent file that you can run.
2. In Java, the compiler creates bytecode (.class files) that are then run on the Java Virtual Machine (JVM).


### complilation in JS & JIT
1. Compilation in JS happens differentsly modren VS engines uses both compilation & interpretation for optimazation
2. **Just-In-Time (JIT)** compilation doesn't typically compile the entire page all at once. Instead, it compiles code incrementally as needed.
3. **Initial Interpretation**: Code is interpreted first, running directly without immediate compilation into machine code.
4. **Hot Spot Detection**: The JIT compiler identifies frequently executed code (hot spots) for optimization like loops or frequently executed functions.
5. **Compilation on Demand**: The JIT compiler compiles hot spots into optimized machine code just before execution.
6. **Reuse Optimized Code**: Optimized code is stored to avoid recompilation in future executions.

### Execution of a JS file
1. **Loading JS file**
   When you open a webpage, the browser loads the HTML and any linked JavaScript files. For example:
   ```HTML
   <script src="script.js"></script>
   ```
   The browser reads this line and loads the script.js file into memory.
2. **Parsing the Code**
   The JavaScript engine (like Chrome's V8 engine) starts reading and analyzing the JavaScript code line-by-line. It creates an Abstract Syntax Tree (AST), which is like a blueprint of how the code is structured
   
   **parse**: By def, anlayse & convert into a strctured way
4. **Executing the Code**
   Once the code is parsed, the engine executes it:
   1. It may interpret the code directly, or
   2. It may compile parts of it into bytecode for faster execution (this is where JIT - Just-In-Time Compilation happens).
   3. Excecuted in Browser(front end) & Node.js is a JavaScript runtime environment (backend) it uses same V8 engine.
5. **Event Loop (if there’s asynchronous code)**
   1. If your JavaScript has things like setTimeout, fetch, or event listeners, the engine uses the Event Loop to manage these tasks.
   2. The event loop makes sure asynchronous tasks (like waiting for data to load) don’t block the rest of the code from running.
6. **Execution Environment**
   1. Each JavaScript file is executed in a global execution context (for the entire file), and for each function, a local execution context is created.
   2. Variables and functions are stored in memory as they are executed.

## Data Types in JavaScript
JavaScript has two main categories of data types:
1. Primitive Types - to store simple values
   1. Stored directly by value
   2. Immutable: their value cannot be changed once created(Any modification creates a new value instead of altering the original.)
      ```JS
      let str = "hello";
      str.toUpperCase(); // Returns new string "HELLO"
      console.log(str);  // Still "hello" — original unchanged
      ```
   3. No methods or properties (but JS temporarily “boxes” them to allow method calls)
   4. Compared by value (==)

| Type                   | Description              | Example                      | `typeof` Result |
| ---------------------- | ------------------------ | ---------------------------- | --------------- |
| **Number**             | Numeric values(integer & float)           | `42`, `3.14`, `NaN`          | `"number"`      |
| **String**             | Textual data             | `"hello"`, `'JS'`            | `"string"`      |
| **Boolean**            | True or false            | `true`, `false`              | `"boolean"`     |
| **Undefined**          | Variable declared but not assigned(implicit)    | `undefined`                  | `"undefined"`   |
| **Null**               | Explicit absence of any value      | `null`                       | `"object"` \*   |
| **Symbol**             | Unique identifiers       | `Symbol('id')`               | `"symbol"`      |
| **BigInt**             | Arbitrary large integers | `12345678901234567890n`      | `"bigint"`      |

2. Non-Primitive Types/reference values (Objects) - to store complex data
   1. Stored by reference (variable holds a pointer to the data)
   2. Mutable: properties and contents can be changed
      ```JS
      let arr = [1, 2, 3];
      arr.push(4);
      console.log(arr); // [1, 2, 3, 4] — original array modified
      ```
   4. Have methods and properties
   5. Compared by reference(===)(two distinct objects with same content are different)
  
| Type                   | Description              | Example                      | `typeof` Result |
| ---------------------- | ------------------------ | ---------------------------- | --------------- |
| **Object**             | Key-value pairs          | `{ name: 'Alice', age: 30 }` | `"object"`      |
| **Array**              | Ordered collection       | `[1, 2, 3]`                  | `"object"`      |
| **Function**           | Callable objects         | `function() {}`, `() => {}`  | `"function"`(but obj underneath)    |
| **Date, RegExp, etc.** | Built-in complex objects | `new Date()`, `/abc/`        | `"object"`      |

<img width="1381" alt="Screenshot 2025-05-25 at 12 29 53 PM" src="https://github.com/user-attachments/assets/9a4aafe8-3edb-40ef-84b9-34764a520550" />

### Why null type is object - JS popular legacy bug/quirk?
1. How null became "object": Early JavaScript stored values with a type tag(a label to identify type of a value), and since null was represented by the null pointer (0x00) which matched the object type tag (0). So, typeof null incorrectly returned "object".
2. How it’s handled now: Modern engines use distinct internal tags for null (separating it from objects), so they know null isn’t actually an object internally.
3. Why it’s not changed: Changing typeof null now would break existing code that depends on this behavior, so the legacy "object" result is kept for backward compatibility.
4. So, since there is an practical impact, be mindful during comparison
   
```JS
let a = null;
let b = undefined;

console.log(a == null); // true
console.log(b == null); // true

console.log(a === null); // true
console.log(b === null); // false
```

## NULL vs Undefined

| Feature     | `undefined`                       | `null`                                     |
| ----------- | --------------------------------- | ------------------------------------------ |
| Type        | Primitive, type: `"undefined"`    | Primitive, type: `"object"` (legacy quirk) |
| Assigned by | JavaScript (automatically)        | Developer (manually)                       |
| Meaning     | No value has been assigned yet    | Value intentionally set to "nothing"       |
| when to use | to identify uninitialized vars (hoisting), missing props so best practise is to avoid using manually| for Resetting, clearing, or empty values       |

| Feature         | `undefined`                                                                                      | `null`                                     |
| --------------- | ------------------------------------------------------------------------------------------------ | ------------------------------------------ |
| **Type**        | Primitive, type: `"undefined"`                                                                   | Primitive, type: `"object"` *(legacy bug)* |
| **Assigned by** | JavaScript (automatically): uninitialized vars, missing object properties, or function arguments | Developer (manually)                       |
| **Meaning**     | No value has been assigned yet                                                                   | Value intentionally set to "nothing"       |
| **Typical use** | Uninitialized vars, missing props or arguments                                                   | Resetting, clearing, or empty values       |


### where can we see implicit undefined
1. Variable declared, not assigned	`let x; console.log(x); // undefined`
2. Function parameter not provided	`function greet(name) { console.log(name); } greet(); // undefined`
3. Object property doesn’t exist	`const user = {}; console.log(user.age); // undefined`
4. No return in function	`function doNothing() {} console.log(doNothing()); // undefined`
5. Empty slot in array `const arr = [1, , 3]; console.log(arr[1]); // undefined`

### explanation: JS temporarily “boxes” them to allow method calls

## HOF
1. HOF stands for Higher-Order Function. A higher-order function is a function that can:
    - Take one or more **functions as arguments**, and/or
    - **Return a function** as its result.
2. In other words, HOFs deal with functions as first-class citizens, meaning they can accept them as arguments or return them as values.
3. **What is first-class citizen?** In a given programming language design, a first-class citizen is an entity which supports all the operations generally available to other entities. These operations typically include being **passed as an argument, returned from a function, and assigned to a variable**
4. Eg of HOF:
  - Built in funcions in JS: map(), filter(), reduce(), forEach(). These methods work with arrays and accept callback functions, making them higher-order functions.
  - Currying, a function is return from parent function
  - Any custom function which accepts and/or return a function

    ```JS
    function multiplyBy(factor) {
    return function(number) {
        return number * factor;
        };
    }

    const double = multiplyBy(2);
    const triple = multiplyBy(3);

    console.log(double(5)); // Output: 10
    console.log(triple(5)); // Output: 15
    ```

## this
1. this in global => window in browser, global obj in node js
2. in regular func => undefined in strict mode, window in non strict mode(this substituation, when this is null /undefined implicitly window obj is attached)
3. in method invokation => obj.x() => this referes to the obj that the method is invoked on here "obj"
4. call, apply, bind
5. in arrow functions => no binding of this => this referes to enclosing lexical scope meaning where the function is created => window or enclosing scope
6. in DOM -> HTML elements on which event is called
7. in class based this.handler => this refers to class instead of event so we have explicitely bind the function => (e) => this.eventHandler.bind(e)


## call, apply, bind
<img width="990" alt="Screenshot 2025-03-18 at 12 25 00 AM" src="https://github.com/user-attachments/assets/7c2fe9b8-e0c8-4a17-be8f-0eb5e8f171b5" />

## Prototype   
1. When a function(except arrow function) is created JS automatically add a property to it, call Prototype - an object
2. A prototype is an object that defines properties and methods which other objects(all instances created by new keyword) can inherit.
3. It acts like a blueprint or template for objects created by a constructor function.
4. constructor function - is a regular JavaScript function that is used to create and initialize objects using new keyword.

```JS
//constructor function
function Person(name) {
  this.name = name;
}
```

```JS
const user = new Person("Alice");
```
1. A new object is created.
2. The new object’s internal [[Prototype]] (accessed using `obj.__proto__`) is set to Person.prototype.
3. `this` inside the function refers to that new object.
4. prototype is used to **define** what future objects will inherit.
5. `__proto__` is used to **access** what this object has inherited.
   
<img width="1086" alt="Screenshot 2025-05-24 at 8 41 03 PM" src="https://github.com/user-attachments/assets/c705d498-0cb2-49da-b8b3-4b6dcc52575c" />

in chrome:
<img width="243" alt="Screenshot 2025-05-24 at 5 54 39 PM" src="https://github.com/user-attachments/assets/1475856a-e7d6-4fbf-944a-2cc712a9b6f0" />

**NOTE:**
1. normal functions also gets prototype by default, but the .prototype is just unused. It's only meaningful with constructor function.
<img width="1027" alt="Screenshot 2025-05-24 at 8 39 54 PM" src="https://github.com/user-attachments/assets/cd108746-f7eb-437c-8317-0694a31140f1" />
2. arrow function will not get prototype object property

```JS
const printSum = (x,y) => console.log(x+y)
console.log(printSum.prototype); // undefined
```

## Polyfill
1. A polyfill is a piece of JavaScript code that **adds a missing feature** to environments (like old browsers) that don’t support it natively.-> means, no build-in feature available
2. It lets developers use modern JS features while maintaining backward compatibility.
3. Example: Older browsers may not support `Array.prototype.includes.` A polyfill would add it if it doesn't exist.
4. How to implement: You use feature detection: check if a method exists, and if not, define it.
   
For methods that instances use → polyfill goes on .prototype.
```JS
if (!Array.prototype.includes) {
  Array.prototype.includes = function (searchElement, fromIndex) {
    const len = this.length;
    let i = fromIndex || 0;
    while (i < len) {
      if (this[i] === searchElement) return true;
      i++;
    }
    return false;
  };
}
```

For static methods → polyfill goes directly on the constructor (function) itself.
```JS
// Polyfill for Object.assign (a static method)
if (!Object.assign) {
  Object.assign = function(target, ...sources) {
    // implementation here
  };
}
```
1. A static method is a method that belongs directly to the constructor function (or class) itself, not to instances created by it.
2. You call static methods on the class or constructor, not on an object created from it.

## Class
1. constructor function works fine, but it’s a bit old-school and verbose.
2. so, new class feature introduced in ES6. which is cleaner and easier to read & No need to touch .prototype — JavaScript handles it for you.

```JS
class Person {
  constructor(name) {
    this.name = name;
  }

  sayHello() {
    console.log("Hi, I'm " + this.name);
  }
}

const p1 = new Person("Alice");
p1.sayHello();
```

Even though you don’t see it, JavaScript still builds the same structure using prototypes:

Behind the scenes:
1. Person is still a constructor function & get prototype object as a default property.
2. sayHello is placed on Person.prototype.
3. Instances (p1) have an internal link to Person.prototype via [[prototype]](accessed using `__proto__`).

```JS
console.log(typeof Person); // "function"
console.log(p1.__proto__ === Person.prototype); // true
console.log(employee.prototype); // {}
console.log(e1.__proto__); // {}
console.log(Object.getOwnPropertyNames(Person.prototype)); // ['constructor', 'sayHello']
```
### Why are console.log(employee.prototype) and console.log(e1.__proto__) showing {} (an empty object)?
1. JavaScript automatically marks class methods (like sayHello) as non-enumerable, meaning they don't show up in Object.keys() & in plain console.log(). But they still exist on the object!
2. Use `Object.getOwnPropertyNames(employee.prototype)` or `console.dir(employee.prototype)`(in browser) to see full content

<img width="430" alt="Screenshot 2025-05-24 at 7 58 30 PM" src="https://github.com/user-attachments/assets/4ebdc78d-e956-4b05-9d45-2551d565ad5c" />

<img width="356" alt="Screenshot 2025-05-24 at 8 16 59 PM" src="https://github.com/user-attachments/assets/abe75830-8725-4b2b-9365-29912f5e7ffb" />

## intresting fact: All functions & classes will get length as a property by default
```JS
function test(a, b, c) {}
console.log(test.length); // 3 → because function takes 3 parameters/args

const printSum = (x,y) => console.log(x+y)
console.log(printSum.length); // 2 because function takes 2 parameters/args
```

```JS
class Employee {
  constructor(name) {
    this.name = name;
  }
}
console.log(Employee.length); // 1 → because constructor takes one parameter
```
##  IIFE (Immediately Invoked Function Expression)
1. syntax -> (function)()
2. function() -> JS will throw err, `Function statements require a function name`

```JS
(function(y){
     return y;
 })(4)
```
## How functions are objects in JS
1. Stored in variables - Assigned to variables like numbers/strings/obj
   
2. Pass as arguments - Passed to other functions (e.g. callbacks)

3. Return from functions - Returned from another function (e.g. closures)
   
4. Store in data structures - Kept in arrays, objects, etc.
5. Add properties	Functions can have custom properties
Every function in JavaScript is an object created by the Function constructor, and inherits from Function.prototype, which provides methods like call, apply, and bind
Functions in JavaScript behave just like data.
That's why they're first-class citizens.
In society, a first-class citizen is someone who enjoys full rights and privileges.

In programming, a "first-class" entity means it has full privileges in the language.

So, when we say functions are first-class citizens, we mean:

They have all the rights and privileges that other core data types (like numbers, strings, objects) have.



## Functions are first class citizens

## Closures

## Currying
for n elements understand recursiveness, .end, ...args, fn.length

## micro fronend

## splice vs slice

## shallow vs deep copy

## react fiber

## bugnub
