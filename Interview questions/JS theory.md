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

**tricky**: **STOP & SEE**
   <img width="158" alt="Screenshot 2025-06-26 at 2 46 51 AM" src="https://github.com/user-attachments/assets/b41ee758-0a28-4ea2-bc2c-b67d70b6a723" />

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
      1. example: to create metadata of library, private keys like a user wants to hide phone number, balance application - hide bank balance - this basically supports encasulation.
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
4. * No return in function	`const doNothing = () => {}; console.log(doNothing()); // undefined`
5. * Empty slot in array `const arr = [1, , 3]; console.log(arr[1]); // undefined`

## == VS ===
1. **`==` (loose equality)**: compares two values for equality **after type coercion**.
2. **`===` (strict equality)**: compares both **value and type**, **no type coercion**.

**Implicit coercion in loose equality**
1. With == before comparing, JavaScript tries to convert operands((one or both sometimes) to the same type,
2. JS understands user intensions & does coercion, but sometimes the output can be a bit strange
3. There are specific rule defined - Abstract Equality Comparison rules in JS

| A           | B                  | Rule - Coercion Behavior & Flow                                                    | Example Result                                            |
| ----------- | ------------------ | --------------------------------------------------------------------------- | --------------------------------------------------------- |
| `null`      | `undefined`        | No coercion; special hardcoded rule                                                   | `null == undefined` → ✅ true                              |
| `null`      | anything else      | No coercion                                                                 | `null == 0` → ❌ false                                     |
| `undefined` | anything else      | No coercion                                                                 | `undefined == false` → ❌ false                            |
| `boolean`   | anything else      | `boolean → number` then compare                         | `false == "0"` → "0" → 0, false → 0 → ✅ `0 == 0`          |
| `string`    | `number`           | `string → number`, then compare                                             | `"42" == 42` → `42 == 42` → ✅ true                        |
| `string`    | `boolean`          | `boolean → number`, `string → number`, then compare                         | `"1" == true` → "1" → 1, true → 1 → ✅ `1 == 1`            |
| `object`    | `string`           | `object → primitive (toString)`, then string compared                       | `[5] == "5"` → `[5]` → `"5"` → ✅ `"5" == "5"`             |
| `object`    | `number`           | `object → string`, then `string → number`, then compare                     | `[1] == 1` → `[1]` → `"1"` → 1 → ✅ `1 == 1`               |
| `object`   | `boolean`           | `object → string` then `string → number`, then compare | `[] == false` → `[]` → `""` → 0, `false → 0` → ✅ `0 == 0` |
| `NaN`       | anything           | Always false; `NaN` never equals anything, including itself                 | `NaN == NaN` → ❌ false                |

   **Why NaN == anything or even itself is false** ?
   1. `NaN` means "invalid number", `NaN` can result from `0/0` or `"hello" * 5`.
   2. Are two `NaN`s the same? We can't say. Think of `NaN` as an “unknown value. Two unknowns can't be confirmed as equal.

3. JavaScript tries hard to make things "work", but sometimes it guesses wrong & output seems wierd. So:
   1. == use this, when you expect implicit coercion to happen but be careful as it's risky
   2. === try to use this as this is safe and predictable

## Implicit coercion with arthmetic operations
1. JavaScript is dynamically typed and tries to be helpful by converting types where it can.
2. The -, *, / operators trigger numeric coercion.
3. But + is special: if either operand is a string, it performs string concatenation instead.

<img width="657" alt="Screenshot 2025-06-28 at 12 00 32 PM" src="https://github.com/user-attachments/assets/ee5fc88f-2d10-4ced-afb4-30a48ca4c8db" />

## var, let, const
1. In JavaScript, var, let, and const are used to declare variables, but they have some differences in terms of scope and mutability:
2. var: It is the oldest way to declare a variable in JavaScript. var is function scoped, meaning that it can be accessed from anywhere within the function it is declared in. However, var does not have block scope, which means that it can also be accessed outside the block it was declared in. They are scoped to nearest function if available. var can be **re-declared(only using another var)** and re-assigned. Can be initilized later
3. let intro in ES6 but it is block-scoped. It can only be accessed within the block it was declared in, including nested blocks. let variables can be reassigned, but not redeclared. Can be initilized later
4. const intro in ES6 It is also block-scoped and cannot be reassigned or redeclared. However, if the constant is an object or an array, its properties can still be modified. Should be initilized while declaration.
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

**Lexical scope = Lexical Scope means that what variables a function/block can access is determined by where it is physically written in the source code**
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
 8. **function/block define/creates scope, variables get scope**

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
## scope chaining
Scope chaining is the process JavaScript uses to resolve variable names. When a variable is referenced, the JS engine looks in the current scope, and if not found, it "climbs up" to parent scopes until it either:

1. ✅ Finds the variable
2. ❌ Reaches the global scope and throws a ReferenceError if not found

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
        console.log(x,y,z) // 10, 20, 30
    }
    inner();
}

outer();

// ANOTHER EXAMPLE
let z = 30;

function grandParent(){
    let x = 10;
    function parent(){
        let y = 20;
        function child(){
            console.log(x, y, z);
        }
        return child;
    }
    return parent;
}

const parent = grandParent();
const child = parent();
child(); // 10, 20, 30
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
   3. **Event Handlers** - In React, **callbacks**(when passed as prop to child) are closures because they "**remember**" the variables (like **state**, **setState**, **props**, etc.) from the component's rendering scope, even after the component function has **finished running**.
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
   5. **Throttling** - study in later notes
   6. **setTimeout** - In setTimeout, **each callback forms a closure** by **remembering variables from its outer scope**, even after the outer function has finished execution.
      ```JS
      for (var i = 0; i < 3; i++) {
        setTimeout(() => console.log(i), 1000);
      }
      ```
   7. once fn - to make a function execute only once - only study if you have time

## Currying/Function Currying
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

## this
1. The this keyword in JavaScript typically refers to the object that invoked or called the function. Its main purpose is to allow methods (functions defined within an object) to be used by another object.
2. But it can have different values based on where we are executing the code.
3. this in global scope => this refers to global object, which is **window** in browser, **global** in node js
   ```JS
   console.log(this);
   let x = 10; // undefined even though x gets created in global scope ,it won't be attached to global obj
   const y = 20; // same
   var z = 30 // attached to global obj
   console.log(this.x) // undefined
   console.log(this.y) // undefined
   console.log(this.z) // 30
   ```
4. in regular func => undefined in strict mode, window in non strict mode
   ```JS
   function test(){
       console.log(this); // global obj in non - strict mode
   }
   
   test();
   ```
5. this is bcz when you don’t explicitly invoke a function, then this will be null, but JavaScript automatically assigns global obj(window in browsers). this is known as this substitution. Think of it global obj is only calling this function in GEC 
6. in method invokation => obj.x() => this referes to the obj that the invoked the method. // here "obj"
   ```JS
   const obj = {
       name: "Mounika",
       printName: function(){
           console.log(this.name);
       }
   }
   
   obj.printName(); // Mounika
   ```
7. Also, we can manually change the context of **this** by using static methods like call, apply, bind - refer to the topic for more info.
8. in arrow functions => Arrow functions do not have their own **this**. Instead, they inherit the **this** value from their parent scope **where they are created**. This means:
   1. If an arrow function is created in the **global scope**, this refers to the global object (window in browsers).
   2. If an arrow function is defined directly as a **method inside an object**, this still refers to the global object, because the arrow function is not created inside the object scope(there is no object scope in JS)— What internally happens is arrow function will be created in the outer/global scope & then assigned to method. **Arrow function is just created as part of obj not inside obj**
      ```JS
      const obj = {
        arrowMethod: () => {
          console.log(this);
        }
      };
      
      obj.arrowMethod();  // Logs: window (in browsers)
      ```
   3. If an arrow function is defined nested, meaning inside a regular method of an object, it inherits this from that method, as arrow function is created in function scope, so **this** points to the object that invoked the regular method.
   in this way we can preserve this->obj, used in complex logic, setTimeout
      ```JS
      const obj = {
        name: "My Object",
        regularMethod: function() {
          const arrowFunc = () => {
            console.log(this.name); 
          };
          arrowFunc();
        }
      };
      
      obj.regularMethod();  // Logs: "My Object"
      ```
9. In nested function, this inside nested function will refer to undefined since we didn't invoked it on any object
    ```JS
      const obj = {
        name: "My Object",
        regularMethod: function() {
          function anotherFunc() {
            console.log(this.name); 
          };
          anotherFunc();
        }
      };
      
      obj.regularMethod();  // undefined in strict, window in non strict
      ```
10. ways to bind obj to this inside nested using .bind method or const self = this;
    ```JS
      const obj = {
        name: "My Object",
        regularMethod: function() {
          function anotherFunc() {
            console.log(this.name);
          };
          const boundAnotherFunc = anotherFunc.bind(this)
          boundAnotherFunc();
        }
      };
      
      obj.regularMethod();  // Logs: "My Object"
      ```

      ```JS
      const obj = {
        name: "My Object",
        regularMethod: function() {
          const self = this;
          function anotherFunc() {
            console.log(self.name);
          };
          anotherFunc();
        }
      };
      
      obj.regularMethod();  // Logs: "My Object"
      ```
11. in DOM -> reference to the HTML element on which event is called
 <img width="857" alt="Screenshot 2025-06-02 at 10 25 50 PM" src="https://github.com/user-attachments/assets/a0298097-422a-47ba-8b1c-63c18fbb01ab" />

## No binding of this in regular methhods - Issue in class based components
1. In regular methods, `this` is evaluated at call time, not when it's defined. That means when you call a method on an object, `this` refers to that object.
2. When assigned to another variable or passed as a callback, the method loses `this` unless explicitly bound.
```js
const obj = {
  name: "Alice",
  greet() {
    console.log(this.name);
  }
};

obj.greet();       // logs "Alice"
const greet = obj.greet; // only reference is passed without binding
greet();           // logs undefined (or global object in non-strict mode)
```
3. In class-based React components, when you pass a class method as a callback, you are passing only the reference to that method:
```jsx
<button onClick={this.handleClick}>Click me!</button>
```
4. This behaves like passing an inline regular function:
```jsx
<button onClick={function () { console.log(this); }}>Click me</button>
```
5. When the user clicks the button, the `handleClick` function is invoked by the DOM element (the button), so `this` inside the function points to the button element, not the component instance.
6. To avoid this loss of `this`, explicitly bind it to the component instance:
```jsx
<button onClick={this.handleClick.bind(this)}>Click me</button>
```
7. Arrow functions do not have their own `this`. Instead, they capture `this` from their lexical (surrounding) scope.
8. In React class components, an instance of the component is created internally by React, and arrow functions defined as class fields automatically refer to that instance:
```js
class MyComponent extends React.Component {
  handleClick = () => {
    console.log(this); // `this` refers to the component instance
  }

  render() {
    return <button onClick={this.handleClick}>Click me</button>;
  }
}
```
9. You can also use inline arrow functions in JSX, which close over the component instance: 
```jsx
<button onClick={() => this.handleClick()}>Click me</button>
```

## Every function in JavaScript has two important things:
| Concept               | What It Refers To                                 | Purpose / Usage                                      | Example                         |
|-----------------------|--------------------------------------------------|----------------------------------------------------|--------------------------------|
| `functionName.prototype` | The prototype object assigned to instances created by the function when called with `new` | Sets the prototype for objects created by `new` (e.g., inheritance for instances) | `sayHi.prototype` is the prototype for `new sayHi()` objects |
| `functionName.__proto__` or internal `[[Prototype]]` | The prototype of the function object itself | Points to `Function.prototype`, giving the function access to methods like `call`, `apply`, `bind` | `sayHi.__proto__ === Function.prototype` |
| `Function.prototype`  | The prototype object for all functions            | Contains methods such as `call`, `apply`, and `bind` that all functions inherit | `Function.prototype.call` is available to all functions |

## Prototype - INHERITANCE
1. In JavaScript, every object has an **hidden internal link to another object** called its **prototype**.
2. A prototype is simply an object from which other objects can **inherit properties and methods**.

**How prototype links are created**
### 1. Using Constructor Functions(implict prototype link)
- When a function(except arrow function) is created JS automatically adds a property to it, called **Prototype** - an object
- constructor function - is a regular JavaScript function that is used to create objects using new keyword.
- When you create an object using the `new` keyword and a constructor function
- A new object is created with properties & methods of that constructor.
- This object is internally linked to the constructor’s **`.prototype`** object.
- This allows all instances created from that constructor to **share methods** defined on the prototype.
  - Example:
    ```js
    function Person(name) {
      this.name = name;
    }
    Person.prototype.greet = function() {
      console.log('Hello', this.name);
    };
    const p = new Person("Mounika");
    console.log(p); // {name : "Mounika"}
    p.greet() // Hello Mounika
    ```

### 2. Using `Object.create()` (explicit prototype link)
- You can also create an object with a specific prototype using `Object.create([object as prototype])`:
- The object passed to `Object.create()` becomes the prototype of the new object.
  - Example:
    ```js
    const base = {
       greet: () => console.log("hi");
    };
    const obj = Object.create(base);
    console.log(obj) // {}
    obj.greet() // hi
    ```

### 3. Manually set prototype
- You can set an object's prototype using:
  - `Object.setPrototypeOf(obj, newPrototype)`

### To access prototype
- You can access an object's prototype using:
  - `Object.getPrototypeOf(obj)`
  - `obj.__proto__` (not recommended but still used)

### Uses
- The main use of prototypes is to **reuse shared properties and methods** across multiple objects.
- Instead of creating a new copy of a method for every object, the method is placed on the prototype.
- All objects linked to that prototype can **access the method** without **duplicating it in memory**.

**Some core concepts:**
1. Object created using object literal {} like const obj = {name: "Mounika"}, it has `Object.prototype` as prototype & this has null as prototype.
2. this is how object get methods like toString(), valueOf(), contructor etc from `Object.prototype`
3. Same for function - which has `Function.prototype` as prototype & this has null as prototype.
4. this is how function get methods like call, apply, bind, arguments, length, name from `Object.prototype`
5. if we try to console.log(Object.prototype) or console.log(Function.prototype) we will get {}. As these are internal, to see these methods, use `Object.getPropertyNames(Object.prototype)`

<img width="1153" alt="Screenshot 2025-06-30 at 6 31 43 PM" src="https://github.com/user-attachments/assets/1648e25d-08d8-4559-a632-8945d7c993a1" />

**NOTE:**
1. normal functions also gets prototype by default, but the .prototype is just unused. It's only meaningful with constructor function.
<img width="1027" alt="Screenshot 2025-05-24 at 8 39 54 PM" src="https://github.com/user-attachments/assets/cd108746-f7eb-437c-8317-0694a31140f1" />
2. arrow function will not get prototype object property

```JS
const printSum = (x,y) => console.log(x+y)
console.log(printSum.prototype); // undefined
```

**prototype vs [[Prototype]]/__proto__**
1. prototype is used to **define** properties or methods, which future objects will inherit.
2. `[[Prototype]]/__proto__` is used represents prototype of current obj from which we can **inherit** properties or methods.

## Prototype chain / prototypal inheritance
Prototypal inheritance in JavaScript means that objects can inherit missing properties and methods from other objects via the **prototype chain** — which is created either through constructor functions or Object.create([prototype])

```JS
function animal(name) {
   this.name = name
   this.walks = true;
   this.printName = function(){
      console.log(this.name)
   } 
}

animal.prototype.speak = false;

const dog = new animal("dog")

console.log(dog.speak); // false
```
flow in order to access speak 
`dog --> animal.prototype --> Object.prototype --> null`

```JS
const cow = {
   name : "Cow"
}

dog.printName.call(cow) // Cow
```
flow in order to access call method
`dog.printName --> Function.prototype(has call method) --> Object.prototype --> null`

## call, apply, bind
1. In React, function is an object it so it gets [[Prototype]] or `__proto__` which links to Function.prototype. From Function.prototype, functions inherit methods like call, apply & bind.
2. These methods help to explicitly change the context (this) when calling a function. Useful when we already have a method in an obj & we want to use the same method on another obj. Or we have a function which can be used for multiple objects
3. **Call:** calls(invokes) a function immediately, with **this** set to the object you pass. accepts & passes **optional** arguments individually.
```JS
function printHobbies(s1, s2){
    console.log(this.name, "has following skills:", s1 + ", " + s2);
}

const employee = {
    name : "Mounika"
}

printHobbies.call(employee, 'react', 'JS') // Mounika has following skills: react, JS
```
4. **Apply:** Like call(), but takes **optional** arguments as an array(array-like objects) & It spreads that array into individual arguments when calling the function. So your function should use rest parameters to gather those args into an array.
   1. Useful when arguments are already in an array or when we don't know no. of arguments expected. 
   2. **array-like objects** -> objects which have length & indices but not an array eg: aruguments for a function doesn't have array methods like push, pop etc & Array.isArray(aruguments) // false

```JS
function printHobbies(...args){
    console.log(this.name, "has following skills");
    args.forEach(skill =>{
        console.log(skill);
    })
}

const hobbiesList = ["react", 'JS', "HTML"]; // employee selected these skills from dropdown

const employee = {
    name : "Mounika"
}

printHobbies.apply(employee, hobbiesList)
// printHobbies.call(employee, ...hobbiesList) // for call spread args & accept using rest op
```

```JS
o/p
Mounika has following skills
react
JS
HTML
```
5. **bind**: instead of immediate invokation, bind returns a new function, accepts & passes **optional** individual aruguments
   1. useful when we expect more arguments later & then invoke function

```JS
function printHobbies(...args){
    console.log(this.name, "has following skills");
    args.forEach(skill => {
        console.log(skill);
    })
}

const hobbiesList = ["react", 'JS', "HTML"]; // employee selected these skills from dropdown

const employee = {
    name : "Mounika"
}

const printHobbiesLater = printHobbies.bind(employee, ...hobbiesList)

printHobbiesLater("CSS");
```

```JS
o/p
Mounika has following skills
react
JS
HTML
CSS
```
**thisArg is optional**
**NOTE:** Even if you don't use or need `this`, you can use call, apply, and bind to manipulate arguments or control function behavior.
when we don't pass thisArg, then this will be `global` in non strict mode & `undefined` in strict mode

1. call ->
2. apply -> 
```JS
console.log(Math.max.apply(null, [1,2,7,3,6])) // 7
console.log(Math.max(...[1,2,7,3,6])) // 7 // modren syntax is to use ...spread instead of apply
```
3. bind -> when you want to generate intermediate function like currying
```JS
function multiply(a, b) {
  return a * b;
}

const double = multiply.bind(null, 2); // `a = 2`, `this` not used
console.log(double(5)); // Output: 10
```

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

##  IIFE (Immediately Invoked Function Expression)
1. syntax -> (function)()
2. function() -> JS will throw err, `Function statements require a function name`

```JS
(function(y){
     return y;
 })(4)
```
3. syntax for arrow func
```JS
(() => console.log("hey"))()
```
4. uses => execute code immediately without polluting global namespace, encapsulation, private variables, to alter the scope

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
1. structuredClone() is a built-in JavaScript method used to deep clone complex JavaScript objects, including those with nested structures, special types like Map, Set, Date etc
2. syntax: `const clone = structuredClone(value);`
3. limitation: **can't clone functions, throws error**

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
      1. resolve: a function to call if the async operation succeeds.
      2. reject: a function to call if the async operation fails.
      3. NOTE: resolve & reject only take single arugument any more will be ignored(do string concatination if required(not like resolve("My promise is", val) instead do resolve("My promise is " + val)
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


## Optimization techniques: Debouncing & throttling
1. Both debouncing and throttling limit the number of executions for a specific time.
2. Which help prevent excessive function calls, which can improve performance and reduce resource usage

So, in a sense, both techniques control the frequency of executions, but with different strategies:

### Debouncing
1. Debouncing waits for a quiet period before executing
2. like waits for 500ms before executing a function
3. Debouncing is better suited for situations where you want to wait for a user to finish interacting with an element before executing a function.
4. Real time usage:
   1. Search input: Wait for the user to finish typing before executing a search query.
   2. Autocomplete: Wait for the user to finish typing before showing suggestions.
5. In general, debouncing means filtering out rapid, unwanted inputs or signals that occur within a short period
  
```JS
const debounceFunc = (func, delay) => {
    let timer;
    return function(...args){ // rest op
        // clearTimeout(undefined); for first time, no error
        clearTimeout(timer); 
        timer = setTimeout(() => {
            func(...args) //spread op
        }, delay)
    }
}


const searchWithKeyword = (keyword) => {
    console.log("searching with keyword " + keyword)
}

const debouncedSearchWithKeyword = debounceFunc(searchWithKeyword, 200)

debouncedSearchWithKeyword("J"); // user typed 1 letter
debouncedSearchWithKeyword("JS"); // user typed 2 letters
setTimeout(() => debouncedSearchWithKeyword("JSX"), 200) // simulating user typed after 200
```

with object method
```JS
const apiManager = {
    attempts: 0,
    fetchAutocomplete(keyword){
        console.log("fetching autocomplete for '" + keyword + "' on attempt", this.attempts);
    }
}

const deboucedFunction = (func, context, delay) => {
    let timer;
    return function(){
        clearTimeout(timer);
        context.attempts++
        let args = arguments;
        timer = setTimeout(() => func.apply(context, args), delay)
    }
}

const debouncedFetchAutoComplete = deboucedFunction(apiManager.fetchAutocomplete, apiManager, 200);

debouncedFetchAutoComplete("J"); // user typed 1 letter
debouncedFetchAutoComplete("JS"); // user typed 2 letters - executes now
```
### Throttling:
1. Throttling executes at a fixed rate, regardless of the input frequency(number of triggers)
2. like for every 500ms executes a function only once
3. Throttling is better suited for situations where you want to execute a function at a fixed rate, regardless of the input frequency.
4. Real time usage:
   1. autosave a file : every 5 seconds save file, debouncing not suitable here, bcz user might be continuosly typing for a long time
   2. gaming : when key pressed down for a long time, we want to trigger action at regular intervals 
   3. Scrolling: Update a UI component every 200ms while the user is scrolling.
   4. API requests: Limit the number of API requests per second to prevent overwhelming the server.
5. Throttle is typically twist grip on the handlebar that the rider uses to accelerate or decelerate the bike. Throttling means controlling the engine speed using throttle

```JS
const fetchAPI = (keyword) => {
    console.log("Fetching API results for", keyword);
}

const throttlingFunc = (func, interval) => {
    let isTrottle = false;
    return function(...args){ // rest op
        if(isTrottle){
            return;
        }else{
            func(...args); // spread op
            isTrottle = true;
            setTimeout(() => {
                isTrottle = false
            }, interval);
        }
    }
}

const throttledFetchAPI = throttlingFunc(fetchAPI, 200);

throttledFetchAPI("phone"); // first API fetched
throttledFetchAPI("laptop"); // second attempt ignored bcz of interval
setTimeout(() => throttledFetchAPI("speaker"), 200); // simulating user delay
```

## shift, unshift, pop, push
<img width="928" alt="Screenshot 2025-06-11 at 12 51 21 AM" src="https://github.com/user-attachments/assets/be94b1bf-d894-40f9-beae-ff2d7d51a9f9" />

for pop & shift => **no args**, even if we pass the methods effectively **do nothing except return the removed elememet**.
for push & unshift => we can pass 1 or more args but **optional**. If omitted, the methods effectively **do nothing except return the array’s length.**

## slice vs splice
**slice:**
1. purpose is to extract a shallow copy of a portion of an array (or string).
2. Does **not mutate** the original array.
3. Returns a new array containing the extracted elements.
4. Syntax:  `arr.slice([start], [end])`
   1. start: index at which extraction begin, if omitted defaults to 0(start of arr)
   2. end: index before which extraction end(non-inclusive), if omitted defaults to array length(last element of arr)
5. Hence, easy way to do shallow copy if gave `arr.slice();` same as `arr.splice(0)`
6. Useful for shallow copying arrays or extracting subarrays

```JS
let arr = ['a', 'b', 'c', 'd', 'e'];

arr.slice(1, 3);      // ['b', 'c']   (indexes 1 to 2)
arr.slice(-3, -1);    // ['c', 'd']   (indexes counted from end)
arr.slice(2);         // ['c', 'd', 'e'] (from index 2 to end)
arr.slice();          // ['a', 'b', 'c', 'd', 'e'] (shallow copy)
arr.slice(5);         // []           (start > length)
arr.slice(3, 3);      // []           (start = end)
arr.slice(3, 5);      // ['d', 'e']   (end > length)
```
**Note**: 
1. if start >= end, return empty array
2. if start >= length, start is set to array length, returns empty array
3. if end > length, end is set to array length
4. negative indices(counted from end, -1 is last element) are supported

**splice**
1. Purpose is to change the contents of an array by removing, replacing, or adding elements in-place.
2. **Mutates** the original array.
3. return an array containing the removed elements or Empty array if no elements were removed.
4. Syntax: `arr.splice(start[, deleteCount[, item1[, item2[, ...]]]])`
   1. start (required): Index at which to start changing the array. If start > array length, start is set to array length (inserts at end).
      if omitted, no error, returns empty array, does nothing.
   3. deleteCount (optional): Number of elements to remove from start.
   4. Defaults to arr.length - start if omitted (i.e., removes everything from start). If 0, no elements are removed.
   5. item1, item2, ... (optional): Elements to add at start position.

```JS
let arr = ['a', 'b', 'c', 'd'];

arr.splice(1, 2, 'x', 'y'); 
// arr is now ['a', 'x', 'y', 'd']
// returns ['b', 'c']

arr.splice(2);
// arr is ['a', 'x']
// returns ['y', 'd']

arr.splice(1, 0, 'z'); 
// arr is ['a', 'z', 'x']
// returns []

arr.splice(1, 0); 
// arr unchanged, returns []

arr.splice(3)
// arr unchanged, returns []

arr.splice(3, 0, 1)
// arr is ['a', 'z', 'x', 1], returns [] 
```

## local storage vs session storage vs cookies
1. Local Storage (LS) and Session Storage (SS) are browser-based client storage options that allow storing key-value pairs without sending data to the server.
2. They are better than cookies for most client-side use because they don't require user consent, have much larger storage capacity, and aren’t automatically sent to server with every HTTP request, making them faster and more efficient.

| Feature               | Local Storage                     | Session Storage                  | Cookies                          |
|------------------------|----------------------------------|----------------------------------|----------------------------------|
| **Storage Limit**      | ~5–10 MB                         | ~5 MB                            | ~4 KB                            |
| **Persistence**        | Until manually cleared           | Until tab/window is closed       | Until expiration time or manually cleared |
| **Page Reload**        | ✅ Persists                      | ✅ Persists (same tab)            | ✅ Persists (until expired)       |
| **Scope**              | All tabs/windows of same origin  | Current tab/window only          | All tabs, also sent to server     |
| **Accessible By**      | JavaScript only                 | JavaScript only                  | JavaScript & server              |
| **Auto Sent to Server**| ❌ No                            | ❌ No                             | ✅ Yes (with every request)       |
| **Expiration Control** | ❌ No                            | ✅ Auto on tab close              | ✅ Yes (set via expiry)           |
| **Typical Use**        | Persistent app state, preferences | Temporary state, form data       | Auth tokens, tracking IDs        |
| **Requires user consent**| ❌ No                            | ❌ No                             | ✅ Sometimes (for tracking/3rd-party cookies) |

**Note**: when we **duplicate** tab, **session storage is shared with that tab**.

## for..of, for...in
-for...of → Iterates over values of an iterable (like arrays, strings, etc.). technically
👉 "Use when you want the actual values."
   - iterable means any obj that we can loop through, technically speacking, it should have Symbol.iterator method.
   - plain object({k:v}) is not an iterable
- for...in → Iterates over keys (property names) of an object or array.
👉 "Use when you want the keys or indexes."

## HTTP - HTTP Methods
- An HTTP(Hyper Text Transfer Protocal) request is like a message your web browser(client) sends to a website to ask for something—like a page, image, or data.
- We have methods to represent the desired action to be performed on a resource in a web server.

**GET**  
1. Used to request data.
2. Example: Visiting `www.news.com` sends a GET request.

**POST**  
1. Used to send new data to the server., multiple calls with same data creates duplicate resources
2. Example: Logging into a website sends a POST request with your login info.

**PUT**  
1. Used to update existing data, completely replace existing resource, if resources doesn't exist creates a new one
2. Example: Editing your profile sends a PUT request with updated info.

**DELETE**  
1. Used to delete data from the server. Deleting non existing resource won't cause any error
2. Example: Clicking "Delete account" sends a DELETE request.

**PATCH**  
1. Used to partially update data. if resource not found -> 404 error not found.
2. Example: Updating only your email address sends a PATCH request.

## fetch API
1. A built-in JavaScript function used to make HTTP requests (like GET, POST).
2. It returns a Promise.

syntax: fetch(url, {options}

```JS
const fetchAPI = async () => {
  try {
    const res = await fetch("https://jsonplaceholder.typicode.com/todos");
    let data = await res.json();
    data = data.slice(0, 5);
    console.log(data);
  } catch (err) {
    console.log(err);
  }
};

fetchAPI();
```

## Event Propagation - capturing, bubbling, event delegation technique, stop propagation(capture/bubble)
Event propagation is how events travel through the DOM in three phases: capturing → target → bubbling.
Default: Event listeners run in the bubbling phase unless specified.

```HTML
<body>
  <div id="outer">
    <div id="inner">
      <button id="btn">Click me</button>
    </div>
  </div>
</body>
```

It has 3 phases:
1. **Capturing Phase:** Event travels top-down:
   `window → document → <html> → <body> → #outer → #inner → #btn(target)`
2. **Target Phase:** Event reaches the element that triggered it (e.g. the clicked button).
3. **Bubbling Phase(default):** Event bubbles up from target back through its ancestors:
   `#btn(target) → #inner → #outer → body → html → document → window`

**To listen in Capture Phase** => Use the { capture: true } option in addEventListener:
```JS
element.addEventListener('click', handler, { capture: true });
```

**To stop, bubbling/capturing:**
During bubbling/capture phase, if we add event listners to all, all will be triggered. To stop this, use `event.stopPropagation();`
```JS
document.getElementById('btn').addEventListener('click', (e) => {
  console.log('Button clicked');
  e.stopPropagation();
});
```

**Event Delegation**
A technique, where a parent element handles events for its child elements using bubbling and event.target.

```HTML
<ul id="list">
  <li>Item 1</li>
  <li>Item 2</li>
</ul>
```
```JS
document.getElementById('list').addEventListener('click', (e) => {
  if (e.target.tagName === 'LI') {
    console.log('Clicked on:', e.target.textContent);
  }
});
```

✅ Benefits of Event Delegation
1. Fewer event listeners → better performance
2. Works with dynamically added elements
3. Cleaner, scalable code structure

## filter, map, forEach, reduce, some, every
| Method   | Description                                 | Return Value                       | Example Code & Output                                      | Gotchas / Tips                                                     |
|----------|---------------------------------------------|----------------------------------|------------------------------------------------------------|-------------------------------------------------------------------|
| filter   | Creates a new array with elements that pass a test | New filtered array               | ```js const evens = [1,2,3,4].filter(n => n % 2 === 0); console.log(evens); // [2, 4] ``` | Callback must return true/false. If using `{}`, include `return` or it returns undefined and filters wrong. |
| map      | Creates a new array by transforming elements | New transformed array            | ```js const doubled = [1,2,3,4].map(n => n * 2); console.log(doubled); // [2, 4, 6, 8] ``` | Must return new value inside callback. Use `map` in React for rendering lists instead of `forEach`. |
| forEach  | Runs a function on each element (no return) | undefined                       | ```js [1,2,3,4].forEach(n => console.log(n)); // logs 1 2 3 4 ``` | Does not return a new array; good for side effects only on each element (e.g., logging, updating DOM), not for transformation. |
| reduce   | Reduces array to a single value using a callback | Single accumulated value         | ```js const sum = [1,2,3,4].reduce((acc, curr) => acc + curr, 0); console.log(sum); // 10 ``` | Must return accumulator inside callback. Always provide initial value to avoid bugs. |
| every    | Checks if all elements satisfy a condition   | Boolean (true/false)             | ```js const allPositive = [1,2,3,4].every(n => n > 0); console.log(allPositive); // true ``` | Checks all elements; don’t confuse with `some` which checks any element. |
| some     | Checks if any element satisfies a condition   | Boolean (true/false)             | ```js const hasEven = [1,2,3,4].some(n => n % 2 === 0); console.log(hasEven); // true ``` | Returns true on first passing element; may not check all elements. |

## map vs object
| Feature                      | `Object`                                                      | `Map`                                                          |
|------------------------------|---------------------------------------------------------------|----------------------------------------------------------------|
| Creation Syntax               | `const obj = { key1: value1, key2: value2 }`                  | `const map = new Map([[key1, value1], [key2, value2]])`         |
| `new` Keyword Required        | Optional (`const obj = {}` or `new Object()`)                 | Required (`new Map()`)                                          |
| Key Types                    | Only strings & symbols                                        | Any value (strings, numbers, objects, functions)                |
| Key Order                    | Numeric keys sorted, others in insertion order                | Fully preserves insertion order                                 |
| Iteration                    | Use `Object.keys/values/entries` or `for...in`                | Directly iterable via `.keys()`, `.values()`, `.entries()`     |
| Size                         | No built-in way (`Object.keys(obj).length`)                   | `.size` property                                                |
| Add / Update Value           | `obj[key] = value`                                            | `map.set(key, value)`                                           |
| Get Value                    | `obj[key]`                                                    | `map.get(key)`                                                  |
| Check Key Exists             | `'key' in obj` or `obj.hasOwnProperty('key')`                 | `map.has(key)`                                                  |
| Delete Key                   | `delete obj[key]`                                             | `map.delete(key)`                                               |
| Clear All                    | Manual loop or reassign to `{}`                               | `map.clear()`                                                  |
| JSON Support                 | ✅ Yes                                                        | ❌ No (must convert manually)                                  |
| Prototype Inheritance        | Has prototype chain (`__proto__`)                            | No prototype (clean by default)                                 |
| Use Case                     | Static structures, JSON-like data                            | Dynamic key-value store, frequent key changes                   |

