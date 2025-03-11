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
