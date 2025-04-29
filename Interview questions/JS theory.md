## INTRO to JS
1. JavaScript (often abbreviated as JS) is a high-level, interpreted programming language that was created to enable dynamic interaction on websites.
2. Year of Introduction: JavaScript was introduced in **1995**.
3. Creator: It was created by **Brendan Eich**, a programmer at **Netscape Communications Corporation.**
4. Original Name: Initially, the language was called **Mocha**, then it was briefly named **LiveScript**, and finally, it was renamed to **JavaScript** as a marketing strategy to capitalize on the popularity of Java at the time (though the two languages are very different).
5. **high-level:** closer to human languages (like English) than to machine code. Developer friendly - to write & read
6. **interpreted**: not compiled beforehand but rather interpreted(executed) line-by-line at runtime by the JS engine (e.g., V8 in Chrome, SpiderMonkey in Firefox).
8. **Dynamic**: don’t need to declare the type of a variable when you create it. The type is determined at runtime - flexible can be restricted with const
9. A **scripting language** is a programming language designed for automating tasks and controlling software, typically executed by an interpreter rather than being compiled.
   JavaScript (JS) is considered a scripting language, as it is primarily used for automating tasks, manipulating web pages, and controlling web browsers, and it is executed by an interpreter (the browser) rather than being compiled.
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

12. **single threaded**: describes how JavaScript runs code: There is only one thread(single call stack), so only one piece of code runs at a time.
13. **Synchronous**: describes when JavaScript runs code: Each line runs one after another, waiting for the previous to complete

Imagine a cashier:
1. Single-threaded = only one cashier open (one task at a time).
2. Synchronous = each customer waits in line until the previous one is done.

JavaScript is single-threaded by design, and it runs synchronously by default, but you can make it asynchronous with tools like `setTimeout`, `Promise`, or `async/await`.

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

    
