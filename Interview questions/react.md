## Server-Side Rendering & Search Engine Optimization
**What is SSR (Server-Side Rendering)**
SSR is a technique where web pages are rendered on the server and sent as fully rendered HTML to the browser, improving initial page load and SEO.

**What is SEO?:** SEO (Search Engine Optimization) refers to the practice of improving the visibility of a website on search engines like Google, making it easier for users to find your site based on relevant keywords.

- **How SSR improves SEO:** SSR improves SEO by delivering fully rendered HTML to search engine crawlers, which ensures that search engines can easily index the content of the page without waiting for JavaScript to load. SSR improves SEO by delivering fully rendered HTML to search engine crawlers, which ensures that search engines can easily index the content of the page without waiting for JavaScript to load.

- **Does React have SSR?:** React by itself doesn't have built-in SSR, but it can be implemented using ReactDOMServer. Frameworks like Next.js provide SSR support out of the box for React.

## How Shallow comparision sufficient in VDOM
- **Diffing** is an alogirithn in the Virtual DOM typically involves shallow comparison(i.e comparing only top level elements) of components or elements in React
- **How Shallow Comparison Works:**
  - For DOM elements: React will compare the type (e.g., div, span) and props (e.g., className, style). If the type or props have changed, React will update the DOM.

  - For components: React compares the props and state of the component. If the shallow comparison detects a change, React will re-render the component and apply the changes.
- If no top-level properties have changed, there is no need for deep level comparision
- Also, deep level comparision is expensive especially for complex websites.

## Why React doesn't allow multiple elements in return?
**1. Virtual DOM Efficiency**
React uses the **Virtual DOM** to compare changes and efficiently update the UI. When a component returns a single root element, React can quickly determine what changed and apply the update. If there were multiple root elements, React would have to deal with more complexity during the reconciliation process, slowing down the rendering.

**2. HTML Structure Consistency**:
- In HTML, every valid structure must have a single parent element. React follows this rule to ensure the structure is valid.
- React still ultimately uses React.createElement() behind the scenes to create the Virtual DOM representation of those elements.
  ```React.createElement(tag, props, ...children)```
- If you try to return multiple elements without a single parent, react won't be able to create html element & will throw an error like:
  ```Adjacent JSX elements must be wrapped in an enclosing tag.```
- **Solution (React Fragments)**: React Fragments allow you to return multiple elements without introducing an extra wrapper element(hence light weight) in the DOM. This avoids unnecessary extra nodes like `<div>` and keeps the DOM clean.
- **Shorthand Syntax**: You can use the shorthand `<>` and `</>` to wrap multiple elements without adding extra tags, making the code more concise:

  ```jsx
  <>
    <h1>Hello</h1>
    <p>This is a paragraph</p>
  </>
  ```
  
## Unidirectional Data Flow in React

**Unidirectional data flow** is a core principle of React. It means that data flows in one direction in a React application: from **parent components** to **child components**. This predictable flow of data makes it easier to understand and manage the state of an application, especially as it grows larger.

**1. Data Flow from Parent to Child:**
- In React, a parent component passes data to its child components via props.
- A child component can only receive data from its parent and cannot directly modify it. Instead, it can only display it or send back events to the parent to handle any changes.

**2. Children Communicate Back to Parent:**
- If a child component needs to update data, it can't directly modify the parent's state. Instead, the child calls functions that the parent passes down as props, and the parent updates its state based on the child’s request.
- This ensures that the parent maintains control over the data and that the application’s state remains predictable.

## Class Component vs Functional Component

| Feature                      | **Class Component**                                    | **Functional Component**                               |
|------------------------------|--------------------------------------------------------|-------------------------------------------------------|
| **Definition**                | A component that extends `React.Component` and uses a class-based syntax. | A component defined as a JavaScript function.          |
| **State Management**          | Can hold and manage local state using `this.state` and `this.setState()` | Can manage state using `useState` hook in React.       |
| **Lifecycle Methods**         | Can use lifecycle methods like `componentDidMount()`, `componentDidUpdate()`, `componentWillUnmount()`, etc. | Can use hooks like `useEffect` for side effects, which can replicate lifecycle methods. |
| **`this` keyword**            | Requires the use of `this` to access component properties and methods. | No need for `this`. All properties are directly available in the function scope. |
| **Performance**               | Slightly slower compared to functional components because of the additional overhead of `this` and class inheritance. | More lightweight and generally faster due to the absence of class-based features. |
| **Syntax**                    | More verbose with `render()` method and class syntax. | Simpler and more concise function-based syntax.        |
| **Hooks Support**             | Does not support hooks (prior to React 16.8). Hooks cannot be used in class components. | Supports hooks (introduced in React 16.8), like `useState`, `useEffect`, `useContext`, etc. |
| **Readability**               | Can be harder to read due to the class structure and `this` context. | Generally easier to read and write due to simpler syntax. |
| **Binding Methods**           | Methods must be explicitly bound to `this` to maintain the correct context in event handlers and callbacks. | No need for binding since there is no `this` keyword. |
| **Default Use Case**          | Often used in older React codebases or when complex lifecycle methods and state management are required. | Preferred for new components in modern React development, especially for simpler components. |

**Key Differences:**
- **Class Components** require `this` for state management and lifecycle methods, whereas **Functional Components** use hooks to manage state and side effects in a more concise manner.
- **Functional Components** are generally preferred in modern React because of their simpler syntax and performance benefits, especially with the introduction of hooks.

## Why we shouldn't update state directly?
- Directly **mutating the state** in React **will not throw an error**, but it will **prevent React from detecting the change**, which leads to **UI inconsistencies**.
- To **properly update the state** and ensure the UI reflects the changes, you should always use:
  - `setState` in **class components**.
  - The **state update function** (`setCount`, `setIsloaded` etc.) in **functional components**.

## Why React Component Names Should Be Capitalized

In React, component names must be capitalized to distinguish them from regular HTML elements. Here's why:

1. **React differentiates between HTML elements and components**:  
   React treats lowercase names (e.g., `<button>`, `<div>`) as standard HTML tags. If you use a lowercase name for a component (e.g., `<useCallbackExample />`), React will assume it's a built-in HTML element, not a custom component.

2. **What happens if not capitalized**:  
   If you don’t capitalize a component name (e.g., `<useCallbackExample />`), React will treat it as a string (or HTML element) and won't render it as a React component. This results in an error or unexpected behavior.


## Example for useCallback

```javascript
import React, { useState, useRef, useEffect, useCallback } from 'react';

const UseCallbackExample = () => {
    const [count, setCount] = useState(0);
    const [count2, setCount2] = useState(0);
    
    // useCallback hook to memoize the increment function
    const increment = useCallback(() => {
        console.log("called");
        setCount(count + 1);
    }, [count]); // depend on count to recreate function when count changes

    // Regular increment function without useCallback
    // Since we don't used callback here this fucntion will get created even if count2 changes
    const increment2 = () => {
        console.log("called 2");
        setCount2(count2 + 1);
    }

    // useEffect hook to log when 'increment' function is created/recreated
    useEffect(() => {
        console.log("created again increment");
    }, [increment]);

    // useEffect hook to log when 'increment2' function is created/recreated
    useEffect(() => {
        console.log("created again increment2");
    }, [increment2]);

    return (
        <>
            <p>Count: {count}</p>
            <p>Count2: {count2}</p>
            <button onClick={increment}>Click</button>
            <button onClick={increment2}>Click2</button>
        </>
    );
  }
```
## Example for useMemo
```JS
import React, { useState, useMemo } from 'react';

    function MyComponent() {
      const [count, setCount] = useState(0);

      // Define a function that returns a computed value
      const expensiveValue = useMemo(() => {
        console.log('Computing expensive value...');
        let result = 0;
        for (let i = 0; i < 1000000000; i++) {
          result += i;
        }
        return result;
      }, []);

      return (
        <div>
          <p>Count: {count}</p>
          <p>Expensive Value: {expensiveValue}</p>
          <button onClick={() => setCount(count + 1)}>Increment</button>
        </div>
      );
    }
```
**Note:** when useMemo not used then expensiveValue will become a fuction when we have to invoke function(add ()) in line 
```
<p>Expensive Value: {expensiveValue**()**}</p>
```

## Behavior of Hooks with and without Dependency Array

- **No Dependency Array (`[]`)**:
  - **`useEffect`** runs after **every render**.
  - **`useMemo`** and **`useCallback`** trigger **recomputations** or **re-creations** on **every render** without optimizations.
  - **Note**: React **warns** in development mode if the dependency array is omitted.

- **Empty Dependency Array (`[]`)**:
  - **`useEffect`** runs only **once**, after the **first render** (similar to `componentDidMount`).
  - **`useMemo`** and **`useCallback`** **memoize** the value or function and **do not recompute** or **recreate** unless the component is remounted.





