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

**explanation: JS temporarily “boxes” them to allow method calls**
JavaScript temporarily wraps(‘boxes’) primitive values into their corresponding object wrappers (like String, Number, etc.) to enable method calls, then discards the wrapper immediately.
```JS
const str = "hello";
console.log(str.toUpperCase()); // "HELLO"
```

Behind the scenes:
1. JavaScript temporarily wraps "hello" in a String object:
→ new String("hello")
2. Calls the .toUpperCase() method on that object.
3. Returns the result.
4. Discards the temporary object.

**that means can we store properties for primitive values?**
```JS
const str = "abc";
str.newProp = 123;
console.log(str.newProp); // undefined (because the boxed object is discarded)
```

## Symbol
1. A Symbol is a primitive data type introduced in ES6, used to create unique and immutable identifiers
3. `const mySymbol = Symbol('description');` — creates a unique symbol with an optional label for debugging.
4. assiging value = `obj[mySymbol] = value;` — use square brackets to set symbol-keyed properties.
7. Access value: `obj[mySymbol]` — must use the same symbol reference with square brackets to get the value.
5. mainly they are used to create non-colliding, hidden object key.
6. non-collinding:
   1. Even if we create a symbol with description or we have a same key, original symbol won't be effected.
   2. example : Hence used In libraries or frameworks, Symbols are used for internal data so that users can’t accidentally clash with those keys.
```JS
const _internal = Symbol('internal');

function setupComponent(comp) {
  comp[_internal] = { mounted: true }; // safe, non-colliding key
}

const userComponent = {
  _internal: 'user-defined value' // totally separate
};

setupComponent(userComponent);

console.log(userComponent._internal); // ✅ 'user-defined value' (untouched)
console.log(userComponent[_internal]); // ✅ { mounted: true } — only accessible with Symbol
```
7. hidden : Not visible in object loops or JSON only access through direct Symbol reference or to get all sumbols -> Object.getOwnPropertySymbols(obj);.
      1. example: to create metadata of library, private keys like a user wants to hide phone number, balance application - hide bank balance - this basicallyt supports encasulation.
      ```JS
      const phoneNumber = Symbol('metaData');
      const anotherHiddenKey = Symbol('anotherHidden')
      const Comp = {
         name: 'Alice',
         [phoneNumber]: 98765432234,
         [anotherHiddenKey] : 'secret456'
      };
      
      console.log(Object.keys(user));        // ['name']
      console.log(JSON.stringify(user));     // {"name":"Alice"}
      console.log(user[phoneNumber]);          //  98765432234
      console.log(Object.getOwnPropertySymbols(user)) // [ Symbol(phoneNumber), Symbol(anotherHidden) ]
      ```

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
| when to use | to identify uninitialized vars (hoisting), missing props| for Resetting, clearing, or empty values       |

Also
```JS
null == undefined // true
null === undefined // false
```
BEST PRACTISE : avoid using undefined manually, so we can identify system implicit behaviour like below

**where can we see implicit undefined**
1. Variable declared, not assigned(hoisting)	`let x; console.log(x); // undefined`
2. * Function parameter not provided	`function greet(name) { console.log(name); } greet(); // undefined`
3. * Object property doesn’t exist	`const user = {}; console.log(user.age); // undefined`
4. * No return in function	`function doNothing() {} console.log(doNothing()); // undefined`
5. * Empty slot in array `const arr = [1, , 3]; console.log(arr[1]); // undefined`

## == VS ===
1. **`==` (loose equality)**: compares two values for equality **after type coercion**.
2. **`===` (strict equality)**: compares both **value and type**, **no type coercion**.

**Implicit coercion in loose equality**
1. With == before comparing, JavaScript tries to convert operands((one or both sometimes) to the same type,
2. JS understands user intensions & does coercion, but sometimes the output can be a bit strange

| Rule                        | Logic Behind It |
|----------------------------|------------------|
| **Boolean → Number**        | Booleans are numeric in JS (`true → 1` (truthy), `false → 0` (falsy)). |
| **String → Number**         | Strings often represent numeric input (e.g. `"5"` from a user input field). |
| **Object → Primitive**      | Objects must be converted to compare values (via `.valueOf()` or `.toString()`),<br>because JavaScript assumes you want to compare the object’s **content**, not its reference.<br> First JS use `.valueOf()`, after this if the value is still obj it's not useful to compare, next it uses `.toString()` to convert to a string |
| **null == undefined**       | Both represent “no value” and are loosely equal.<br><br>But **not equal** to `0`, `false`, or `""` — because those are **actual values**:<br>• `0` is a valid number<br>• `false` is a boolean - represents not true<br>• `""` is a string (empty, but still a string without characters). |
| **NaN == anything → false** | `NaN` means "invalid number" — it's **not equal to anything**, not even to itself.<br><br>**Why?**<br>`NaN` can result from `0/0` or `"hello" * 5`.<br>Are two `NaN`s the same? We can't say.<br>Think of `NaN` as an “unknown value.”<br>Two unknowns can't be confirmed as equal. |
| **Symbol can't be coerced** | Symbols are **unique identifiers** by design and cannot be automatically converted to a string or number — coercion throws a **TypeError**. |

3. JavaScript tries hard to make things "work", but sometimes it guesses wrong & output seems wierd. So:
   1. == use this, when you expect implicit coercion to happen but be careful as it's risky
   2. === try to use this as this is safe and predictable

Not studied completely - ignore below
implicit coercion during with arthemtic operators like +, - etc
1. JS always try to preserve most information possible
2. This is to avoid accidental data loss and to keep operations meaningful — even if sometimes the results look a bit odd.
3. So, the non-primitive or less "specific" type usually gets converted.

## var, let, const
1. In JavaScript, var, let, and const are used to declare variables, but they have some differences in terms of scope and mutability:
2. var: It is the oldest way to declare a variable in JavaScript. var is function scoped, meaning that it can be accessed from anywhere within the function it is declared in, including nested functions. However, var does not have block scope, which means that it can also be accessed outside the block it was declared in. var can be **re-declared(only using another var)** and re-assigned
3. let intro in ES6 but it is block-scoped. It can only be accessed within the block it was declared in, including nested blocks. let variables can be reassigned, but not redeclared.
4. const intro in ES6 It is also block-scoped and cannot be reassigned or redeclared. However, if the constant is an object or an array, its properties can still be modified.
5. let & const can be function scoped, not technically, function's body is a block, if let & const are declared inside a function, they are scoped to nearest block which is function's body, hence they seem like they are function scoped.
6. All 3 can have global scope if declarabled in global space(outside any function or block).

## What is a block?
1. Block is defined by curly braces {}
2. block allows us to group multiple statements. Like in if & for, while loops
   ```JS
   if(true){
      console.log("HI");
      let x = 10;
      console.log(x);
   }
   ```

## Scoping
1. **Scope** defines the **accessibility of variables** in the code, below are the types
2. **Global scope:** Variables declared(using var, let, const) outside any function/block are accessible from **anywhere** in the script.
```JS
var a = 10; // global
function test() {
  console.log(a); // 10
}
test();
```
3. **Function scope:** Variables declared(using var, let, const) inside a function are only accessible **inside that function**
   ```JS
   function test() {
      var x = 5;
      console.log(x); // 5
   }
   console.log(x); // ❌ Error: x is not defined
   ```
4. **Block Scope (ES6+):** variables declared inside {} using let & const are only accessible **within that {}**
   ```JS
   {
     let a = 10;
     const b = 20;
   }
   console.log(a); // ❌ Error: a is not defined
   ```
   **IMP:** var is doesn't respect a {} block (like if, for, {}) — variables declared with **var are scoped to the nearest function, or global if no function.** <br>
   **Note:** so is let & const are function scoped here? NO. let & const are scoped to the nearest block({}), since entire function body is a block({}), let & const are block scoped to that function.
   
   ```JS
   {
     var x = 5;
   }
   console.log(x); // ✅ 5
   ```
6. **Lexical Scope:** Variables declared in an enclosing scope (outer function or block) are accessible within that scope—this is called function scope if declared with var, and block scope if declared with let or const. These variables are also accessible to any nested (inner) scopes (inner functions or blocks). This behavior is known as lexical scope.
   ```JS
   // function
   function outer() {
      let x = 10;
      function inner() {
         console.log(x); // ✅ 10
      }
      inner();
   }

   //block
   function outer(){
       let z = 10;
       {
           console.log(z)
       }
    }
   outer();
   ```
 7. **function/block define/creates scope, variables get scope**

## Scope of function paramater:
1. Function parameters are scoped to the function body, meaning they exist only inside the function.
2. When a function is invoked, a new scope is created where parameters are initialized before the function body runs.
   ```JS
   function run(x, f = () => x){
       var x = 20;
       console.log(x, f()) // f forms a closure with x as these params get evaluted before function run
   }
   run(30);
   ```
4. Parameters share the same scope as local variables declared inside the function (e.g., just like var).
5. Because of this shared scope:
   1. You can redeclare a parameter using var — the declaration alone is ignored, but reassignment works.
   ```JS
   function test(x) {
      var x = 10;
      console.log(x) // 10 - declared with var
   }
   test(20)
   ```
   2. You cannot redeclare a parameter using let or const in the same scope(in nested scope possible, next topic) — this causes a syntax error.
   ```JS
   function test(x) {
     let x = 10; // ❌ SyntaxError: Identifier 'x' has already been declared
   }
   test(20)
   ```
   3. only assignment without declaration doesn't create implicit global(bcz, function params & local variables share same scope, JS thinks like x is already created)
   ```JS 
   function run(x){
       x = 20;
       console.log(x) //20
   }
   
   run(30)
   console.log(x)// x is not defined
   ```
   4. Even though these params act like var they **won't get HOISTING, functions params won't get hoisted)**
   ```JS
   function run(x){
       console.log(x) //undefined bcz we didn't passed any param while invoking, hence JS implicitly assigns undefined when param is not passed 
   }
   run();
   ```

## What is Shadowing in JavaScript?
1. Shadowing in JavaScript is when a variable declared in an inner scope (like inside a function or block) has the same name as a variable in an outer scope.**(doesn't consider declaration type(var, let & const))**
2. The inner variable “shadows” or hides the outer one within its scope, making the outer variable inaccessible in that region.
3. **so, basically shadowing - redeclaration in a nested (different) scope, doesn't effect outside variable — It works not just with var, but also with let and const — because you're declaring it in a new scope, not redeclaring in the same one.**
```JS
let message = "Hello from global";

function greet() {
  let message = "Hello from function"; // shadows the outer `message`
  console.log(message); // "Hello from function"
}

greet();
console.log(message); // "Hello from global"
```

**Also deep nesting & declaration type(var, let & const) isn't considered for shadowing**
```JS
var message = "Hello from global";

function greet() {
  let message = "Hello from function"; // shadows the outer `message`
  console.log(message); // "Hello from function"
  {
      const message = "hellow from nested"
      console.log(message)
  }
}

greet();
console.log(message); // "Hello from global"
```

## Implicit global
1. Implicit global is created when you assign a value to an undeclared identifier(without using var, let, or const) inside a function or block or globally.
2. JavaScript automatically creates a global variable on the global object (window in browsers).

```JS
function implicit(){
    {
        x = 10;
        console.log(x) // 10
    }
    console.log(x) // 10
}

implicit()
console.log(x); // 10
```

tricky guess below code o/p
```JS
function implicit(){
    x = 20;
    {
        var x = 10;
        console.log(x)
    }
    console.log(x)
}

implicit()
console.log(x);
```
o/p
```JS
10
10
reference error: x is not defined
```
explanation: inside function scope hoisting happens **ONLY** bcz **we have a var declaration,  no hoisting without declaration**, so JS treats it as
```JS
function implicit(){
    var x; // hoisting during memery allocation phase of execution context for function
    x = 20;
    {
        x = 10;
        console.log(x)
    }
    console.log(x)
}

implicit()
console.log(x);
```

## GEC - Global execution context
1. In JavaScript, the Global Execution Context is the default environment where code is evaluated and executed.
2. The GEC is created when your JavaScript code first starts running.  
   - In the **browser**, it represents the `window` object.  
   - In **Node.js**, it represents the `global` object.
3. **Phases of GEC**
   1. **Creation Phase / Memory Allocation** : Memory is allocated for variables and functions. Variables are initialized to `undefined`, Functions are stored completely in memory.
   2. **Execution Phase / Thread of Execution**: Code is executed line by line. Variables are assigned actual values.
4. The Call Stack is a **LIFO (Last In, First Out)** structure that manages **execution contexts**. It keeps track of which function is currently running and what happens next.
5. First, the **GEC is created and pushed onto the Call Stack**. For **every function invocation** (e.g., `square(n)`), a new **Function Execution Context (e.g., E1)** is created and pushed onto the Call Stack. Once the function finishes executing, its execution context is **popped off** the Call Stack.
6. When all functions have returned and all code is finished executing, the **GEC is finally popped off** the Call Stack, leaving it empty.
7. **Function Definition vs. Invocation**
   - **Definition** - When a function is defined, it is **stored in memory** during the creation phase (e.g., inside the GEC).
   - **Invocation** - When a function is called, a **new Execution Context is created** and **pushed onto the Call Stack** for execution.

<img width="855" alt="Screenshot 2025-05-25 at 5 19 25 PM" src="https://github.com/user-attachments/assets/19a000a8-b8f8-4d3b-a40e-347d3225b652" />

## HOISTING 
1. Hoisting is JavaScript's default behavior of moving declarations to the top of their containing scope (Global - outside function/block, function scope - inside function, block scope - inside {}(let & const not var)) during the creation phase of the Execution Context.
2. It applies to:
   1. Variable declarations (var)
   2. Function declarations
3. Variables declared with var are hoisted but initialized with undefined.
4. Function declarations are fully hoisted (you can call them before they appear in code).
5. let and const declarations are hoisted but are in a **Temporal Dead Zone (TDZ)**
6. **Temporal Dead Zone (TDZ):**
   1. The Temoporal Dead Zone is the region of code where a variable is known to exist (because it is hoisted) but cannot yet be accessed because it hasn't been initialized.
   2. Meaning, undefined won't be assigned to the variable unlike var
   3. The variable is in a TDZ from the start of the block or scope where the variable is declared until the line where it is initialized.
   4. During this period, accessing the variable will throw a ReferenceError
   5. It applies to variables declared with let and const, but not to var

```JS
console.log(a); // undefined due to hoisting of var declaration
var a = 5;

foo(); // works fine because function declaration is hoisted
function foo() {
  console.log("Hello");
}

console.log(b); // (Actually code stops here at first err) ReferenceError: Cannot access 'b' before initialization
console.log(c); // ReferenceError: Cannot access 'c' before initialization
let b = 10;
const c = 10;
```
Arrow functions act like variables
```JS
sayHi(); // (Actually code stops here at first err) TypeError: b is not a function, undefined();
var sayHi = () => {
    console.log("Hi");
}

sayBye(); // ReferenceError: Cannot access 'sayBye' before initialization
let sayBye = () => {
    console.log("Bye");
}

saySeeYa(); // ReferenceError: Cannot access 'saySeeYa' before initialization
let saySeeYa = () => {
    console.log("SeeYa");
}
```

## EVENT LOOP
1. JavaScript runs code using a single-threaded call stack, handling one task at a time in a synchronous manner.(immediately executes, like explained in GEC)
2. For asynchronous behavior (like timers, network calls, or events), JavaScript relies on the browser’s Web APIs since it doesn't have built-in async capabilities.
3. When an async function like `setTimeout` or `fetch` is called, it’s handed off to the browser. The timer or request runs in the background, separate from the main thread.
4. After the async operation finishes, its callback is not immediately executed.
5. Because, it disturbs the javaScript's single-threaded nature
   1. Interrupt current code execution, causing unpredictable behavior.
   2. Crash the stack, especially if multiple async callbacks tried to run while the stack was already busy.
   3. Lose control over execution order, especially when multiple async operations finish around the same time.
6. A queue ensures callbacks wait their turn, preserving the correct and safe execution order.
6. So, it's placed in a queue with FIFO pattern:
   - **Promises** and Mutation observer's go to the **microtask queue**.
   - **Timers**, I/O, and events go to the **macrotask queue** (also called the callback or task queue).
7. The event loop is a mechanism which continuously checks the call stack. When it’s empty, it first processes all tasks from the **microtask queue**, in order.
8. Only after the microtask queue is empty, the event loop take the next task from the **macrotask queue** and push it onto the call stack.
9. If a microtask (like a `.then()` handler) queues another microtask, it runs the new microtask after the current one, before moving on to any macrotasks.
10. This prioritization can delay macrotasks for a long time if microtasks keep chaining — this situation is known as **macrotask starvation**.
11. Through this system — call stack, Web APIs, task queues, and the event loop — JavaScript can handle asynchronous operations without blocking synchronous code.

## How functions are objects in JS
1. Every function in JavaScript is an object created by the Function constructor, and inherits from `Function.prototype`, which provides methods like `call, apply, and bind`
   ```JS
   function sayHello(name) {
   return `Hello, ${name}`;
   }
   console.log(sayHello.__proto__ === Function.prototype); // true
   ```
3. Add properties	- Functions can have custom properties
   ```JS
   //there are many - one is to avoid globals creation
   function increment() {
   increment.value++;
   return increment.value;
   }
   increment.value = 0;
   
   console.log(increment()); // 1
   console.log(increment()); // 2
   ```
5. Stored in variables - Assigned to variables like numbers/strings/obj   
6. Pass as arguments - Passed to other functions (e.g. callbacks)
7. Return from functions - Returned from another function (e.g. closures)
8. Store in data structures - Kept in arrays, objects, etc.
   ```JS
   const arr = [() => console.log("I'm a func"), 2, 5, "hi"]
   ```

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

## call, apply, bind
1. use apply when you already have a array like object to pass or when you don't know no. of params(need to use ...args to accept all args)
<img width="990" alt="Screenshot 2025-03-18 at 12 25 00 AM" src="https://github.com/user-attachments/assets/7c2fe9b8-e0c8-4a17-be8f-0eb5e8f171b5" />

2. modren syntax is to use ...spread instead of apply
```JS
console.log(Math.max.apply(null, [1,2,7,3,6])) // 7
console.log(Math.max(...[1,2,7,3,6])) // 7
```

## Closures
1. A closure is a function along with its lexical environment.
2. The lexical environment consists of the function’s local environment and a reference to the lexical environment of its parent. This chain continues up to the global scope
3. since, there is no parent to global, lexical environment of global is global scope it self.
4. In other words, a function "remembers" the variables that are in same scope at the time it was defined.
5. For example, if an inner function is defined inside an outer function, the inner function can access variables declared in the outer function.
```JS
// let x = 30;
// let y = 30;
// let z = 30;

let x = y = z = 30; // same as individually declaring like above

function outer(){
    let x = 10;
    function inner(){
        let y = 20;
        console.log(x,y,z)
    }
    inner();
}

outer();
```
7. The key feature of closures is that this access remains intact even after the outer function has finished executing.
```JS
let x = y = z = 30; // same as individually declaring

function outer(){
    let x = 10;
    function inner(){
        let y = 20;
        console.log(x,y,z)
    }
    return inner;
}

const inner = outer();
inner();
```
9. Closures remember references to the parent’s variables, not their values. If the value of a variable changes later in the closure shared environment(within counter function(includes incrementCounter, or any other closures inside it)), the closure sees the updated value not the value at the time of definition.

**Not shared = new environment(every call to counter(), creates a new environment)**
```JS
function counter(){
    let count = 0;
    function incrementCounter(){
        count++; // will increment evertime closure is invoked
        console.log(count)
    }
      
    count++ //initially increment by 1
    return incrementCounter;
}

const incrementCounter = counter();
incrementCounter(); //2
incrementCounter(); //3

const incrementCounter2 = counter(); //new environment
incrementCounter2(); //2
```
9. Closures have many uses like
   1. Encapsulation - Encapsulation is the concept of **restricting direct access** to some of an object's components and **exposing only necessary behavior.** In JavaScript, closures can be used to **create private variables**.

      Eg: above counter, where we aren't exposing count value directly but only through log method. Real world example **Bank app example** - set, withdraw, deposit, getBalance

```JS
   function bank(){
       let userAccounts = {} ;
       
       const operations = {
           setUserBalance: (accountId, balance) => {
               userAccounts[accountId] = balance
               console.log("New user added suucessfully");
           },
           withdraw: (accountId, amount) => {
               userAccounts[accountId] -= amount;
               console.log("Widthdraw successful, your remaining balance:", userAccounts[accountId]);
           },
           deposit: (accountId, amount) => {
               userAccounts[accountId] += amount;
               console.log("Deposit successful, your new balance:", userAccounts[accountId]);
           },
           fetchBalance: (accountId) => {
              console.log("Your current balance is", userAccounts[accountId]);
           },
       }
       return operations;
   }
   
   const gachibowliBranch = bank(); 
   gachibowliBranch.setUserBalance(123, 4000) // New user added suucessfully
   gachibowliBranch.withdraw(123, 300); // Widthdraw successful, your remaining balance: 3700
   gachibowliBranch.deposit(123, 500); // Deposit successful, your new balance: 4200
   gachibowliBranch.fetchBalance(123); // Your current balance is 4200
   
   console.log("--------------------------------");
   
   const kondapurBranch = bank(); // new environment created
   kondapurBranch.setUserBalance(456, 7000) // New user added suucessfully
   kondapurBranch.withdraw(456, 300); // Widthdraw successful, your remaining balance: 6700
   kondapurBranch.deposit(456, 500); // Deposit successful, your new balance: 7200
   kondapurBranch.fetchBalance(456); // Your current balance is 7200
```

   2. **Currying** - next topic
   3. **Event Handlers - In React functional components** , event handlers often capture values from the component scope. Here, handleClick is closure, which has **access to setCount & count even after the Counter component re-rendered**. 
    ```JS
   import { useState } from "react";

   const Counter = () => {
     const [count, setCount] = useState(0);
   
     function handleClick() {
       setCount(prev => prev + 1);
     }
   
     return <button onClick={handleClick}>Clicked {count} times</button>;
   }
    ```
   4. **Debouncing** - study in later notes
   
   5. **setTimeout** - In setTimeout, **each callback forms a closure** by **remembering variables from its outer scope**, even after the outer function has finished execution.
      ```JS
      for (var i = 0; i < 3; i++) {
        setTimeout(() => console.log(i), 1000);
      }
      ```
   6. once fn - to make a function execute only once - only study if you have time

## Currying
1. Currying is a functional programming **technique** where a function with multiple arguments is transformed into a series of functions, each taking one argument at a time.
2. **Uses: enabling reusability & partial application** : we partially applied logic & got a function & we wait for next argument to apply & also multiply2 method can be reused many times

```JS
function multiply(a) {
  return function (b) {
    return a * b;
  };
}

const double = multiply(2);
const triple = multiply(3);
console.log(double(5)); // 10
console.log(triple(5)); // 15
```
3. **Real world example:** In a website, you want to log messages with different tags for different parts of the app (e.g., `[Auth], [Payment], [UI]`).

```JS
function createCustomLog(tag){
    function customLog(log){
        console.log("[" + tag + "]: " + log)
    }
    return customLog;
}

const authLog = createCustomLog("Auth");
const paymentLog = createCustomLog("Payment");

authLog("User logged in successfully"); // [Auth]: User logged in successfully
paymentLog("Payment failed due to insufficient balance"); // [Payment]: Payment failed due to insufficient balance
```

**Very imp :** for n elements understand recursiveness, inner.end, arguments.length

**arrow function syntax**
```JS
const multiply = a => b => a * b;
```

## What is functional programing?
Functional programming is a way of writing code where you:
   1. **Use functions** to build your program - functions as first class citizens - pass as an argument, store in variable, resturn from another funtion - resusablity - modularity - USE: easy to write complex code.
   2. **Immutability** - no updates to variables or data - Once you create something, don’t change it - make a new one. USE: Fewer bugs because you don’t change things
      ```JS
      const nums = [1, 2, 3];
      const doubled = nums.map(n => n * 2); // makes a new array
      ```
   3. Try to keep functions **pure** – they don’t depend external world - Always give the same output for the same input. - USE: easy to debug
      ```JS
      function add(a, b) {
        return a + b;
      }
      ```

## Prototype - INHERITANCE
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
console.log(Person.prototype); // {}
console.log(p1.__proto__); // {}
console.log(Object.getOwnPropertyNames(Person.prototype)); // ['constructor', 'sayHello']
```
### Why are console.log(Person.prototype) and console.log(p1.__proto__) showing {} (an empty object)?
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

## this
1. this in global => window in browser, global obj in node js
2. in regular func => undefined in strict mode, window in non strict mode(this substituation, when this is null /undefined implicitly window obj is attached)
3. in method invokation => obj.x() => this referes to the obj that the method is invoked on here "obj"
4. call, apply, bind
5. in arrow functions => no binding of this => this referes to enclosing lexical scope meaning where the function is created => window or enclosing scope
6. in DOM -> HTML elements on which event is called
7. in class based this.handler => this refers to class instead of event so we have explicitely bind the function => (e) => this.eventHandler.bind(e)
   
##  IIFE (Immediately Invoked Function Expression)
1. syntax -> (function)()
2. function() -> JS will throw err, `Function statements require a function name`

```JS
(function(y){
     return y;
 })(4)
```

## Mutation observer
1. A JavaScript API that watches for changes in the DOM (like element additions, removals, or attribute updates) and triggers a callback asynchronously.
2. Earlier, developers used Older DOM mutation events like DOMNodeInserted, DOMSubtreeModified to track DOM changes
3. But, They fired synchronously and too frequently, even for small changes, causing performance issues, main-thread blocking, and browser inconsistencies — leading to deprecation.
4. MO is more efficient bcz it batches multiple changes & runs once per event loop tick.
5. means MO waits until the current JS call stack finishes(synchronous), groups all changes together — e.g., multiple appendChild() calls trigger a single callback with all mutations, then runs the callback once asynchronously via the microtask queue per 1 event loop cycle(synchronous call stack -> micro stask queue -> macro task queue).
6. It also allows control over what to observe — all these features making it lightweight and performant.
7. Real world scenerio - there is an e-commerce website, where produts get loaded as user scrolls down, so requirement is to send data to google analytics data on what products user has viewed.

```JS
const target = document.getElementById('product-container');

const observer = new MutationObserver((mutations) => {
    mutations.forEach(mutation => {
       // send data to google analytics
       console.log(mutation.type)
    });
});

observer.observe(targetElement, {
  childList: true,              // Watch for addition or removal of child nodes
});

// Stop observing if needed:
// observer.disconnect();
```

7. other options to watch are
   
| Option                  | Description & Example Code                                                                 |
|-------------------------|--------------------------------------------------------------------------------------------|
| `childList`             | Observe when direct children are added or removed. <br>**Code:** `{ childList: true }` <br>**Example:** Detect when a new product card is added to a list. |
| `attributes`            | Observe changes to attributes like `class`, `id`, or `src`. <br>**Code:** `{ attributes: true }` <br>**Example:** Detect when an image's `src` or a button's class changes. |
| `characterData`         | Observe changes to text nodes (e.g., inside `<p>` or `<span>`). <br>**Code:** `{ characterData: true }` <br>**Example:** Detect when a user edits a review or description text. |
| `subtree`               | Extends observation to all descendant elements. <br>**Code:** `{ childList: true, subtree: true }` <br>**Example:** Track product cards added within deeply nested UI components. <br>⚠️ **Note:** `subtree` alone does nothing — use it with `childList`, `attributes`, or `characterData`. |
| `attributeFilter`       | Observe only specific attributes (requires `attributes: true`). <br>**Code:** `{ attributes: true, attributeFilter: ['class', 'style'] }` <br>**Example:** Detect when only `class` or `style` attributes change on buttons or items. |
| `attributeOldValue`     | Include the old value of a changed attribute (requires `attributes: true`). <br>**Code:** `{ attributes: true, attributeOldValue: true }` <br>**Example:** Log the previous value of an image's `src` before it changes. |
| `characterDataOldValue` | Include the old value of changed text (requires `characterData: true`). <br>**Code:** `{ characterData: true, characterDataOldValue: true }` <br>**Example:** Capture the original content of a paragraph before the user edits it. |


## Micro fronend
Micro Frontend is an **architectural approach** where a large frontend application is decomposed into smaller, independent pieces—called micro frontends—each owned by different teams. These pieces work together to form a complete user experience, but are developed, tested, and deployed independently.

1. Enables teams to work independently and choose different tech stacks(react, vue, angular).
2. Improves scalability(maintaining easy) and speeds up development.
3. A shell/container app loads and integrates all micro frontends.
4. Communication between micro frontends is usually via events or shared state.
5. Common implementation methods: **iframes, JavaScript bundles, Web Components, module federation(module 5).**
6. Challenges: shared state management, ensuring consistent UI, routing, and performance.
7. Used in large apps like **e-commerce sites, where product, checkout, and user profile** are separate micro frontends.

## splice vs slice

## shallow vs deep copy
Copy - To create a new reference of an array/object. When copying an object or array in JavaScript, you’re either:

**Shallow Copy**
1. Copies only the **first level** of the object or array.
2. **Nested objects are still referenced**, not copied.
3. Modifying nested data in the copy will also affect the original.
```javascript
const original = { a: 1, b: { c: 2 } };
const shallowCopy = { ...original };

shallowCopy.b.c = 42;

console.log(original.b.c); // Output: 42 (affected)
```
**Other ways to do**
1. Object: `{ ...obj }, Object.assign({}, obj)`
2. Array: `[...arr], arr.slice()`

**Deep Copy**
1. Copies all levels recursively, creating completely independent objects.
2. Modifications in the copy do not affect the original, even for nested objects.
3. Usually more complex and expensive to perform.
```JS
const original = { a: 1, b: { c: 2 } };
const deepCopy = JSON.parse(JSON.stringify(original));

deepCopy.b.c = 42;

console.log(original.b.c); // Output: 2 (unchanged)
```
**Other ways to do**
1. JSON.parse(JSON.stringify(obj)) -> has limitations(only works if no functions, undefined, or special objects like Date)
2. structuredClone - built-in JS function for deep clone - has limitation(only works if no functions)
3. _.cloneDeep(obj) from Lodash
4. Custom recursive function

### Limitations on JSON.parse(JSON.stringify(obj))
1. Functions, undefined, and symbols are lost
```JS
const id = Symbol("id of user");

const obj = {
name: "Alice",
greet: () => "Hi",
age: undefined,
[id]: 123 
};

const deepClone = JSON.parse(JSON.stringify(obj));
console.log(deepClone); // { name: 'Alice' }
```
2. Dates become strings
```JS
const obj = { today: new Date() };
const deepClone = JSON.parse(JSON.stringify(obj));

console.log(deepClone.today);              // "2025-05-26T12:00:00.000Z" ❌
console.log(deepClone.today instanceof Date); // false ❌
```   
3. Special objects like Map, Set, and RegExp become {}
```JS
const obj = {
  regex: /abc/,
  map: new Map(),
  set: new Set(),
};

const copy = JSON.parse(JSON.stringify(obj));
console.log(copy);
```
4. Infinity, NaN become null
```JS
const obj = { val1: Infinity, val2: NaN };
const deepClone = JSON.parse(JSON.stringify(obj));
console.log(deepClone); // { val1: null, val2: null } ❌
```
5. Prototype is lost
```JS
function Person(name) {
  this.name = name;
}
const alice = new Person("Alice");

const clone = JSON.parse(JSON.stringify(alice));
console.log(clone instanceof Person); // false
```

### Structured clone

### Mutation VS Reassigning
1. Mutation (e.g. push, change property) =>	Shared — both reflect change
2. Reassignment (e.g. arr[2] = ..., obj.key = {...}) =>	Not shared — breaks reference, changes are separate

```JS
const arr = [1,2,[3,4,5]]
const arrCopy = [...arr]
arr[2].push(6) // mutating the existing one
// arr[2] = [3,4,5,6] // reassigning a new arr
console.log(arrCopy);

// Output:
// for mutation, changes: [ 1, 2, [ 3, 4, 5, 6 ] ]
// for reassigning, no change: [ 1, 2, [ 3, 4, 5 ] ]

const obj = {name : "Mounika", address : {city: "Hyd"}}
const objCopy = {...obj}
obj.address.city = "Chennai"  // mutating the existing one
// obj.address = {city : "Chennai"} // reassigning a new arr
console.log(objCopy);

// Output:
// for mutation, changes: {name : "Mounika", address : {city: "Chennai"}}
// for reassigning, no change: {name : "Mounika", address : {city: "Hyd"}}
```

## Promises
1. A Promise is an object which represents the eventual completion (or failure) of an asynchronous operation and its resulting value.
2. States of promise:
   1. Pending – initial state, neither fulfilled nor rejected. -> when we log immediately we will get this
   2. Fulfilled – operation completed successfully.
   3. Rejected – operation failed.
3. We can also create a promise other than getting from async operation. Syntax explanation
   1. new Promise -> creating with new keyword using Promise -> a JS built-in class or constructor function
   2. this accepts a executor function with params(resolve & reject) -> these aren't keywords we can use any other names
      3. resolve: a function to call if the async operation succeeds.
      4. reject: a function to call if the async operation fails.
```JS
   let promise = new Promise((resolve, reject) => {
      // Asynchronous operation
      if (/* success */) {
       resolve("Success!");
      } else {
       reject("Error occurred.");
      }
   });
```
4. Now that we created a promise, we have to consume it using then & catch block(takes callbacks).
   1. .then() gets called if the promise resolves.
   2. .catch() gets called if the promise rejects.
   3. .finally() runs regardless of success or failure.
      
```JS
   promise
     .then(value => {
     console.log("Success:", value)
     }).catch(error => {
     console.error("Failed:", error)
     }).finally(() => {
     console.log("Done")
     });
```
5. Promise chaining - we can chain then blocks when previous then block does another async op & so on... This chaning is also known as **callback hell**
   ```JS
   doTask1()
     .then(result1 => doTask2(result1))
     .then(result2 => console.log(result2))
     .catch(err => console.error(err));
   ```

   ```JS
   const giveNumber = new Promise((success, failure) => {
      setTimeout(() => {
           if (true) {
               success(5);  // resolves with 5 after 2 seconds
           }
       }, 2000);
   });

   console.log(myPromise); // Logs the Promise object (initially pending)

   const multiplyBy2 = (num) => {
       return new Promise((resolve, reject) => {
           setTimeout(() => {
               reject(num * 2);  // rejects with num*2 after 2 seconds
           }, 2000);
       });
   };

   giveNumber
       .then((res) => {
           return multiplyBy2(res);  // after myPromise resolves, calls multiplyBy2
       })
       .then((res2) => {
           console.log(res2);  // this will not run because multiplyBy2 rejects
       })
       .catch((err) => {
           console.log("I'm an err", err);  // catches rejection from multiplyBy2
       });
   ```
6. async/await(intro in **ES8**) - helps to avoid callback hell & write asynchronous code that looks like regular, easy-to-read code.
   1. **await** waits for a promise to fulfill or reject inside an **async** function. **A FUNCTION ALWAYS NEEDED**
   2. error handling(catch block) will be done using **try/catch**. - **ALWAYS NEEDED**
   3. chaining - Each await waits for the previous async operation to finish before moving on, we can write it like normal synchronous code.

```JS
  const runTasks = async () => {
     try {
       // const result1 = await promise; // when it's a direct promise
       const result1 = await doSomething();
       const result2 = await doSomethingElse(result1);
       const result3 = await doThirdThing(result2);
       console.log('Final result:', result3);
     } catch (err) {
       console.error('Error:', err);
     }
  }

   runTasks();
```
7. Static methods -> iterable -> array of promises [p1, p2, p3]
   1. **`Promise.resolve(value)`** — creates a **resolved** promise with the given `value`.  
   2. **`Promise.reject(error)`** — creates a **rejected** promise with the given `error`.  
   3. **`Promise.all(iterable)`** — waits for **all promises** to resolve & returns an array of resolved values.; if any promise rejects, it rejects immediately with that reason
   4. **`Promise.allSettled(iterable)`** — waits for **all promises to settle** (resolve or reject) and returns an array of their outcome.  
   5. **`Promise.race(iterable)`** — waits for **the first promise to settle** (resolve or reject) and returns that promise’s outcome.  
   6. **`Promise.any(iterable)`** — waits for **the first promise to resolve** and returns its value; if all promises reject, it rejects with an **AggregateError**(catch block if exists).  

**NOTE: STOP & READ** If two (or more) promises settle at exactly the same time (e.g., both resolve or reject after 2 seconds), Promise.race will settle with whichever promise’s resolution or rejection the JavaScript engine processes first — practically, that means:
1. It picks the result of the promise that the event loop happens to handle first.
2. This is usually nondeterministic (can vary run-to-run or environment-to-environment).
3. So, it’s not guaranteed which one “wins” if they settle simultaneously.

## bugnub
