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

    
