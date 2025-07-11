## Virtual DOM
- The **virtual DOM** is a lightweight, in-memory version of the real DOM, created by React using the reactDOM library.
- When a **user interaction** happens, there are two patterns
  - It might triggers a **internal state change** in child or
  - It triggers a **callback to the parent** -> **state change in a parent** -> parent passes new props down to children.
- Now, all these **affected components** will re-render along with it's **children**.
- These re-renders doesn't change real DOM, instead creates a new VDOM with changes
- So, if multiple state changes are happening, it will **batch** them to optimize performance instead if re-rendring each time for 1 state update.
- Then React performs a deep comparision btw the **new virtual DOM** & the **previous virtual DOM**.
- This comparision is done by a **diffing** algorithm, this actually checks for changes in:
    - Element types (`div`, `span`, etc.)
    - Keys (especially in lists)
    - Props (e.g., `className`, `style`, `onClick`)
- React **does not compare virtual DOM with real DOM** directly (real DOM is slow to access & read).
- This comparision gives actually changed values(minimal) & creates a effect list. 
- Only the **actually changed parts** will be added, for example, if only `className` changes, only the class attribute change is added to effect list, the entire element
- This whole process where React **render & generates the new virtual DOM**, **compares it with the old one**, and **figures out what needs to change**. **It prepares a list of changes**, is called **Reconciliation**.
- **Remember** - but re-render happens for whole component including children while creating VDOM.
- Now, all these changes in list will be applied to **DOM in commit phase** in one go
- After DOM updates, the **browser repaints the UI in one go**.

# 🔄 React Fiber 
- The problem with the existing reconciliation process is that it **compares all components at once, synchronously.**
- If the user interacts with the browser during this comparison, they may notice a **lag**, and the page might become **unresponsive** especially in **larger apps.**
- **Fiber is a re-written version of this process to solve that problem.**
- **What Fiber does is divide the comparison work into small units.**
- **Each unit of work is called a fiber, and one fiber is responsible for comparing one component**
- After completing the work for one fiber, React checks:
  > "Do I have time left in this frame for the browser to handle user inputs?"
- If time is left, React continues with the next fiber's work.
- If not, it pauses and yields control to the browser.
- Later, it resumes the remaining fiber work from where it left off.
- This way, the React app stays responsive to user inputs even during rendering.
- Once all the comparison work is done, React gives out a list of changes.
- After that, the process is the same:
  - DOM updates happen in the **commit phase**
  - Then the **browser repaints** the UI

## built-in hooks
1. Hooks are built in functions which allow us to do as follows
   - `useState`: manage state
   - `useEffect`: perform side effects
   - `useContext`: share data between components(avoid prop drilling)
   - `useReducer`: predictive way to manage complex state
   - `useCallback`, `useMemo`: optimize performance
   - `useRef`: work with DOM elements(when no need re-rendering on change of values) 
   - and many more.
2. All built-in book have `use` prefix. It's react way of indenfying hooks & apply hook rules

## useState:
1. useState is a React Hook that adds state to functional components. 
2. If you want your component to "remember" a value across renders—such as user input, a counter, or a toggle—you can use this hook.

```
javascript
const [state, setState] = useState(initialValue);
```
  1. state: The current state value.
  2. setState: A function used to update the state.
  3. initialValue: The initial value of the state.

3. When you update the state using the setState function, React identifies it & rerender the component. Which eventually updates the UI with the new state values.

### Why we shouldn't update state directly(`count = 5;`)?
- Directly **mutating the state** will only update the value in that moment, but it won't change the react internal state value, so no rerender will trigger to reflect the state changes in UI.
- React can only recognise state changes done by setState function.
- Note: Updating state directly **won't not throw an error**, unless it's a const value(which often is)
- To **properly update the state** and ensure the UI reflects the changes, you should always use
- The **state update function** (`setCount`, `setIsloaded` etc.) in **functional components**.
 
### useState - setState - prevstate when multiple state updates
  1. `set[State](updater);` -> convention to use like `setCount(newValue);`
  2. **updater:** can either be an object or a function.
     - **value/expression:** Directly update state.
     - **Function:** (prevState) => prevState + 1
  3. It's recommended to use prevState when we update the same state multiple times in a row like below
     ```JS
     const [count, setCount] = useState(0);
     
     setCount(count + 1);
     if(true){ //some condition
       setCount(count + 1);   
     }
     ```
  4. Now the value will be 1 only not 2.
  5. Because React batches state updates, it groups multiple state changes together into a single re-render, instead of doing one render for each update. This is to improve performance.
  6. This means React doesn't wait for one state update to finish before starting the next one.
  7. So when you do setCount(count + 1) multiple times, each one uses the same old count value — not the updated one.
  8. But `setCount(prev => prev + 1)` uses the latest value each time.

P.S: React batches all state updates inside event handlers or inside useEffect together with 18+ it's batching everywhere(study later)

## useEffect - mount, update, unmount
1. useEffect is a React Hook that lets you perform **side effects(actions outside component)** in function components. Side effects include tasks like:
    - Fetching data from an API
    - Setting up subscriptions (adding event listeners)
    - Updating the DOM manually (changing title on routing)
    - Running timers (setTimeout, setIntervals
2. It take 2 parameters -> a callback, dependency array
3. It replaces lifecycle methods like **componentDidMount**, **componentDidUpdate**, and **componentWillUnmount** from class components.
4. componentDidMount - empty dependency array - runs once per component lifecycle
```JS
useEffect(() => {
  console.log('Component mounted');
}, []);
```
5. componentDidUpdate - Run when state/props change - can be given in dependency array
```JS
useEffect(() => {
  console.log('Count changed:', count);
}, [count]);
```
6. componentWillUnmount - Cleanup - clean any event listeners or timers to avoid memory leak
ex: to add event listener when it user clicks escape to close a modal

```JS
useEffect(() => {
  const handleKeyDown = (e) => {
    if (e.key === 'Escape') onClose();
  };
  window.addEventListener('keydown', handleKeyDown);

  return () => window.removeEventListener('keydown', handleKeyDown);
}, []);

```
Note: if dependency array not provided, component will render everytime prop or state changes.

### what is memory leak, garbage collection, why is it neccessary to cleanup?
1. A memory leak is unreleased memory that is no longer in use. This means RAM already reserved some memory but not released, even though it's not being used anywhere. Over time, this unused memory accumulates, which can slow down or crash your app.
2. Garbage collection (GC) is an asynchronous automatic process in JavaScript (and many other languages) that:
    - removes objects from memory when they are no longer reachable and Frees up memory used by those objects
    - "Reachable" means: there is some reference to the object from the current call stack, closure, or global scope.
    - it runs outside of the main JS call stack, in the engine's background system.

    ```JS
    function createUser() {
      let user = {
        name: 'Alice'
      };
    
      console.log(user.name); // 'Alice'
    
      // After this line, `user` is no longer needed
    }
    ```
    - there are no more references to that object once the function execution is completed.
    - The garbage collector will clean it up automatically in the background.
3. But, GC only works if there are no references to the object.
4. If you create a timer, or add event listener that will get attached to window(gloabl obj).
5. So, even if component unmounts, window still holds a reference to that obj, hence, can't be garbage collected & leads to memory leak.
6. **We can also handle this in finally in both try..catch..finally or then()...catch()...finally**
7. **Advanced: Technically speacking, GC cleans heap objects (non-primitive things). Primitives are removed automatically and immediately as soon as EC for functions & GEC for entire code is removed from call stack after execution**

## useRef
1. useRef is a react hook which return a mutable object: {current: initialValue}
2. syntax : `inputRef = useRef(initialValue);`
   
Uses:
1. to access DOM elements directly - scroll to section, focus an input box on mount
```JS
import React, { useRef } from 'react';

const ScrollExample = () => {
  const sectionRef = useRef();
  const inputRef = useRef();

  const scrollToSection = () => {
    sectionRef.current.scrollIntoView({ behavior: 'smooth' });
  };

  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return (
    <>
      <input ref={inputRef} placeholder="Autofocus input" />;
      <button onClick={scrollToSection}>Scroll to Section</button>
      <div style={{ height: '100vh' }}>some long text in between</div>
      <div ref={sectionRef}>🎯 Target Section</div>
    </>
  );
}
```
3. to store form data without causing re-renders - uncontrolled components
```JS
import React, { useRef } from 'react';

function UncontrolledForm() {
  const nameRef = useRef();
  const emailRef = useRef();

  const handleSubmit = () => {
    alert(`Name: ${nameRef.current.value}, Email: ${emailRef.current.value}`);
  };

  return (
    <>
      <input ref={nameRef} type="text" placeholder="Enter name" />
      <input ref={emailRef} type="email" placeholder="Enter email" />
      <button onClick={handleSubmit}>Submit</button>
    </>
  );
}
```
5. to preserve values over re-renders - counter - perfect for internal tracking.
```Js
import React, { useEffect, useRef } from 'react';

function TimerCounter() {
  const countRef = useRef(0);

  useEffect(() => {
    const id = setInterval(() => {
      countRef.current++;
      console.log('Count:', countRef.current);
    }, 1000);

    return () => clearInterval(id);
  }, []);

  return <div>Open the console to see the counter ⏱️</div>;
}
```

### controlled components - uncontrolled components
1. Controlled Component: Components in which DOM elements are controlled by React state via state, setState(event handlers). So that, every change of DOM element will cause a re-render useful for realtime tracking.
   
```JS
const [name, setName] = useState('');
<input value={name} onChange={(e) => setName(e.target.value)} />
```
2. Uncontrolled Component: Components in which DOM elements are not controlled by react state instead they are controlled by DOM itself, react can access DOM values & change it (if needed) via useRef without causing re-rendering - used for storing temp data of a form before sumbission.
   
```
const nameRef = useRef();
<input ref={nameRef} />
```

## What will be recomputed on re-render?
- **Recomputed on Every Render:**
  - Functions, variables, and everthing inside return - all child components & JSX code(`<div>Hellow!</div>`)
- **Not Recomputed on Every Render:**
  - `useState`: State values persist between renders and are not recalculated unless explicitly updated.
  - `useRef`: Value persists between renders without causing re-renders
  - `useMemo`: Memoized values are only recomputed when their dependencies change.
  - `useCallback`: Memoized functions are re-created only when their dependencies change.
  - `React.memo`: Prevents re-rendering of child components unless their props have changed.
  - `useContext`: accessing context don't re-render, but the component only re-renders when the context value changes.

## React.memo
- It is a higher-order component (HOC)(explained later) that wraps a functional component to prevent re-rendering of the component if its props haven't changed.
- Usecase: In react, by default when a component re-render due to state update, all child components also re-render even if no props/state of these doesn't change.
- let's say You have a component that re-renders often, but one of its child components doesn't need to update unless a specific prop changes. When we can wrap it with React.Memo, child components will only re-render when it's props changes
- **POINT to NOTE:** memo only does reference equality (only checks the reference, not the internal structure). Let's say we passed an obj as prop, if we don't recreate obj but updated a key value, then reference stays same, then our component won't re-render

```javascript
const MyComponent = React.memo((props) => {
  // Your component logic
  return <div>{props.name}</div>;
});
```
- **Practical use case:** score card of a player, everytime score updated(state change in Game), re-renders all children(Player profile Component) & score display(JSX) but we don't what this to happen unless player info(like name) also changes.
```sql
App
│
└── Game
    ├── Player   ← re-renders every time
    └── ScoreCard  ← updates score state
```  

```JS
import React, { useState, memo, useCallback, useMemo } from "react";

// ✅ This Player won't re-render unless `name` changes by using `memo`
const Player = memo(({ name }) => {
  console.log("Player rendered");
  return <p>Hi, {name}</p>;
});

// ✅ This ScoreCard won't re-render unless `score` and/or `increaseScore` changes by using `memo`
const ScoreCard = memo(({score, increaseScore}) => {
  console.log("ScoreCard rendered");
  return (
    <div>
      <span> Player Score: {score} </span>
      <button onClick={increseScore}>
        Increase Score
      </button>
    </div>
  );
});

const Game = () => {
  const [playerName, setPlayerName] = useState("");
  const [score, setScore] = useState(0);

  const playerInfo = useMemo(() => {
    return {
      country: "India",
      state: "Telangana"
    }
  }, []);

  const increaseScore = useCallback(() => {
    setScore(prev => prev + 1);
  }, []);

  return (
    <div>
      <input
        type="text"
        placeholder="Enter Name"
        value={playerName}
        onChange={(e) => setPlayerName(e.target.value)} />
      <Player name={playerName} info={playerInfo}/>
      <ScoreCard score={score} increaseScore={increaseScore} />
    </div>
  );
}
```

## What is callBack?
- A callback is a function passed as an argument to another function, which is called (or "called back") later — usually after some operation or event happens.
- In react, to update parent state by child, child invokes a prop, a function passed by parent to child component

## useCallback
- A React hook that returns a memoized version of a function — the function is only re-created when dependencies change.
- In React on every render - functions, variables & JSX will be recomputed
- But we often doesn't need functions to be recreated(unless any particular value changes)
- To avoid this & improve performance, we can use useCallback.

- **practical use:**
  1. Passing a memoized function(using callback) to a memoized child (React.memo) or useEffect, useMemo(as dependencies) -	Prevents child from re-rendering unnecessarily when the parent re-renders
  2. Let's say our component has 2 states & even 1 state updates, all function will be recomputed.
  3. Space will take up -> not a big issue, v8 does garbage collection. But this is something to avoid as app grows.
 
Code: scorecard example  

## useMemo
- A React hook that returns a memoized value of a function — the function is only re-computed when dependencies change.
- In react on every render - functions, variables, JSX will be recomputed
- What if the varibale is calculated using some expensive(time taking) logic
- we don't what it to be recomputed on every prop or state change unless some particular value(can be props or state or [] for 1 time calculation)
- To achieve this we have to use useMemo
  
**Practical use**
1. Derived data from props or state by doing expensive computations (e.g., sorting, filtering, math) - Avoids recalculating unless necessary
2. Memoize **static** props (objects/arrays) when passing to memoized components - Dynamic object should be created as new reference if not memoised component won't detect(as React.memo only compare references(reference equality)

Code: scorecard example  

**Note:** when useMemo not used then expensiveValue will become a fuction then we have to invoke function(add ()) in line 
Also, when hook not used, expensiveValue will get computed on every render
```
<p>Expensive Value: {expensiveValue()}</p>
```

## Behavior of Hooks with and without Dependency Array
- **No Dependency Array (`[]`)**:
  - **`useEffect`** runs after **every render**.
  - **`useMemo`** and **`useCallback`** trigger **recomputations** or **re-creations** on **every render** without optimizations.
  - **Note**: React **warns** in development mode if the dependency array is omitted.

- **Empty Dependency Array (`[]`)**:
  - **`useEffect`** runs only **once**, after the **first render** (similar to `componentDidMount`).
  - **`useMemo`** and **`useCallback`** **memoize** the value or function and **do not recompute** or **recreate** unless the component is remounted.
 
### Is it good to use `useMemo` and `useCallback` everywhere - NO
- `useMemo` and `useCallback` can increase **memory consumption** because they **store memoized values or functions**.
- Even though **memory usage** is generally low, but it can grow if you are using these hooks on **large datasets** or many memoized functions.
- **Best practice** is to use these hooks **only when necessary** (e.g., for expensive calculations or functions passed down to child components) and
- **Avoid using them for every small value or function** for more redability, keep the code clean & to avaoid unnecessary code complexity.
- Another issue is **Reference equality issue** - Reference equality refers to the concept of comparing the memory references of objects, arrays, functions, or other non-primitive data types, rather than comparing their contents.
- React uses reference equality for re-rendering
- This issue arises when we don't use dependency array properly & pass unnecssary values to it.
- Meaning, when I'm using React.memo & using this calllback function as prop & gave [count](or any other unrelated dependency) then child component will render everytime on change of count. This will make React.memo **useless**
- here we don't need count to be added as a dependency we can directly use prevCount from updater function
- Including count in the dependency array of useCallback can make sense when the function needs to **update its behavior** based on the updated count value.

Code: Score card example when score is in Game only => & increScore got score as dependency then score scard will render every time.

## `useContext`\ Context API  
1. useContext(Context API) is a React hook that lets you share data across components without passing props manually at every level.
2. Uses:
    - Helps to avoid prop drilling.
    - Good for global/shared state (like theme, user info, auth status, i18 etc.)
    - light weight alternative for Redux when use along with useReducer

**What is prop drilling?**
- Prop drilling is passing data from a parent to deeply nested child components through props.
- Even if the middle components don’t need the data, they still have to pass it down.
- This can make the code messy and harder to maintain as the component tree grows.
- Uneccessarily middle level components get access to it.

**practical example for prop drilling:**
```pgsql
App **(fetches User info)**
│
└── Layout **(receives user)**
    ├── Sidebar
    └── MainContent **(receives user)**
        ├── Header **(uses user)**
        └── Dashboard
```

**How it's created?**
1. Create a context using createContext from React.
2. Wrap the children in the return of the app with a Context Provider & pass data in value prop.
3. Inside any child component, use useContext(MyContext) to access the data directly.
```JSX
import React, { createContext } from 'react';
export const MyContext = React.createContext();

const App = () => {
  return (
    <MyContext.Provider value={{ greet: "Hello", name: "Mounika"}}>
      <Child />
    </MyContext.Provider>
  );
}

const Child = () => {
  const {name, greet} = useContext(MyContext);
  return <p>{greet + " " + name}</p>; // "Hello"
}
```
- Note: **createContext is not a hook**. It is a standard function to create context obj hence it can be invoked(called) outside functional component unlike hooks.
- Practical example - refer useContext.md file or check stackblitz App & child file

## Lazy loading - code splitting - done with `React.lazy()`
1. **Lazy loading is a technique** where you **defer the loading** of resources (like components, images, or data) until they are actually needed. Instead of loading everything upfront, you load parts of your application **on demand**.
2. **Code splitting is splitling/breaking** the code into separate files/chunks.
3. Lazy loading uses code splitting under the hood to break & fetch those chunks dynamically.
4. uses:
   - Improves performance: **Reduces the initial load time** by splitting the app into smaller chunks.
   - Saves bandwidth: Only downloads code when the user actually needs.
   - Enhances user experience: **Faster page load**.
5. `React.lazy()` is a **built-in function** that enables lazy loading
6. Combine it with **`Suspense`**, an inbuilt component to show a fallback UI (like a spinner) while the component loads.
7. NOTE: **Suspence only shown in UI when the component loads not always**
8. **REMEMBER** to give a **capitilized** name for **`LazyComponent`** as it is a React component
9. NOTE: Lazy loading happens **once per component per session (or page load)**

```JS
import React from 'react';
const LazyComponent = React.lazy(() => import('./HeavyComponent'));

function App() {
  const [show, setShow] = React.useState(false);

  return (
    <div>
      <button onClick={() => setShow(true)}>Load Heavy Component</button>

      <Suspense fallback={<div>Loading...</div>}>
        {show && <LazyComponent />}
      </Suspense>
    </div>
  );
}
```

## Manually add delay to see Suspense loader
1. `React.lazy()` requires a function returning a Promise, if we directly use `setTimeout` schedules a callback but immediately returns a number (timer ID), not a Promise.
```JS
const LazyScoreCard = React.lazy(() =>
  new Promise(resolve => {
    setTimeout(() => {
      resolve(import('./ScoreCard'));
    }, 200);
  })
);
```

## Why React doesn't allow multiple elements in return?
**1. Virtual DOM Efficiency**
React uses the **Virtual DOM** to compare changes and efficiently update the UI. When a component returns a **single root element**, React can **quickly determine** what changed and apply the update. If there were multiple root elements, React would have to deal with **more complexity** during the **reconciliation** process, slowing down the rendering.

**2. HTML Structure Consistency**:
- In HTML, every valid structure must have a single parent element. React follows this rule to ensure the structure is valid.
- React still ultimately uses React.createElement() behind the scenes to create the Virtual DOM representation of those elements.
  ```React.createElement(tag, props, ...children)```
- As JSX is still JS, functional components as still function which expect only single elememt to return.
- If you try to return multiple elements without a single parent, react won't be able to create html element & will throw an error like:
  ```Adjacent JSX elements must be wrapped in an enclosing tag.```
- **Solution (React Fragments in react 16)**: React Fragments allow you to return multiple elements without introducing an extra wrapper element(hence light weight) in the DOM. This avoids unnecessary extra nodes like `<div>` and keeps the DOM clean.

<img width="655" alt="Screenshot 2025-06-28 at 6 03 10 PM" src="https://github.com/user-attachments/assets/bb8345c6-2b3a-4cb6-9e18-3751f6bfaa1c" />

**multiple elements in children**

![image](https://github.com/user-attachments/assets/83e1bd6a-4d06-43dc-a52c-292e4b0b5b5d)

```JSX
import React, { Fragment } from 'react';

const MyComponent = () => {
  return (
    <Fragment>
      <h1>Title</h1>
      <p>Description</p>
    </Fragment>
  );
}
```
- **Shorthand Syntax for Fragment**: You can use the shorthand `<>` and `</>` to wrap multiple elements without adding extra tags, making the code more concise:

  ```jsx
  <>
    <h1>Hello</h1>
    <p>This is a paragraph</p>
  </>
  ```
  
## Unidirectional Data Flow in React
**Unidirectional data flow** is a core principle of React. It means that data flows in one direction in a React application: from **parent components** to **child components**. This **predictable flow** of data makes it **easier to understand and manage the state** of an application, especially as it grows larger.

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

## HOC - higher order component
1. A Higher-Order Component (HOC) is a function that takes a component as an argument and returns an enhanced version of that component.
2. HOCs are useful when we want to reuse common UI behavior across multiple components — like logging, authentication, error handling, or theming. Instead of duplicating this behavior in every component, we can extract it into a reusable function.
3. For example, let's say we want to log multiple times inside a component. We can create a withLogger function that accepts a component and returns a new component that logs a message whenever needed.
4. Internally, this function returns another function that takes props and renders the original component with the original props & additional behavior injected.
5. So inner function forms a closure with access to component

```JS
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```
example:

```JS
import React from 'react';

function withAuth(Component) {
  return function AuthHOC(props) {
    const isAuthenticated = false; // Simulating authentication logic

    if (!isAuthenticated) {
      return <div>Please log in to view this content.</div>;
    }

    return <Component {...props} />;
  };
}

function Dashboard() {
  return <div>Welcome to the dashboard!</div>;
}

const DashboardWithAuth = withAuth(Dashboard);

// Usage
function App() {
  return <DashboardWithAuth />;
}

export default App;
```
Note: In above example, withAuth is used like a function that's why withAuth(lower case 'w') is valid.

## useReducer
1. useReducer is a hook introduced in React 16.8 & used as an alternative to ```useState```
2. It allows more sophisticated state management(managing state in a more predictable and structured way), especially when the state has multiple sub-values or complex transitions(state update logic).
3. It allows you to manage state in a way similar to Redux, where state transitions are handled by dispatching actions to a reducer function. A reducer function takes the current state and an action, and returns a new state.

```JS
const [state, dispatch] = useReducer(reducer, initialState);
```
  - **reducer**: A function that takes the current state and an action as arguments. It defines how state should change based on different action types and returns the new state.
    - just like in redux, An action is an object used to describe what to do and to pass data necessary for state updates. Standard format is
      - type: A constant to identify which logic to run
      - payload: optional arguments used for state updation logic. Obj & primitive Eg: {name: ,age:} / 5
        ```js
        const action = {
          type: 'SET_USER',  // The action type
          payload: { name: 'John', age: 30 }  // The data to be used by the reducer
          // name : 'John'
          // age: 5
        };
        ```
  - **initialState**: The initial state value, which can be any type (e.g., object, array, etc.).
  - **state**: The current state.
  - **dispatch**: A function that you call to dispatch actions that will be handled by the reducer.


```JS
import React, { useReducer } from 'react';

// Reducer function
import React, { useReducer } from 'react';

// Initial state
const initialState = {
  count: 0,
  user: { name: '', age: 0 }
};

// Reducer function
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { ...state, count: state.count + 1 };
    case 'decrement':
      return { ...state, count: state.count - 1 };
    case 'setName':
      return { ...state, user: { ...state.user, name: action.name } };
    case 'setAge':
      return { ...state, user: { ...state.user, age: action.age } };
    default:
      return state;
  }
}

// Component
function UserProfile() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <p>Name: {state.user.name}</p>
      <p>Age: {state.user.age}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
      <button onClick={() => dispatch({ type: 'setName', name: 'John' })}>Set Name to John</button>
      <button onClick={() => dispatch({ type: 'setAge', age: 30 })}>Set Age to 30</button>
    </div>
  );
}

export default UserProfile;
```

**When to Use ```useReducer``` vs ```useState```:**
Use ```useState``` when:
- You have simple state logic that doesn’t require multiple state updates based on previous states.
- The state is simple (e.g., a boolean, string, number) and doesn’t need complex transitions.
  
Use ```useReducer``` when:
- Your state logic is more complex, and you need to handle multiple different actions to update state.
- You need to perform state updates based on the previous state.
- You have complex interactions or need to manage a state object with multiple properties.
- You want a more predictable, centralized way of managing state transitions (similar to how state is managed in Redux).

## Is `type` required in `useReducer` and `Redux`?

1. **In `useReducer`** : **`type` is not strictly required** by the API, but it is a **common convention** and **recommended** for predictable state management.
2. **In `Redux`** : **`type` is required** in every action. `type` is a core part of Redux and tells the reducer what action is being performed and how to update the state.

## Why React Component Names Should Be Capitalized
In React, component names must be capitalized to distinguish them from regular HTML elements. Here's why:

1. **React differentiates between HTML elements and components**:  
   React treats lowercase names (e.g., `<button>`, `<div>`) as standard HTML tags. If you use a lowercase name for a component (e.g., `<useCallbackExample />`), React will assume it's a built-in HTML element, not a custom component.

2. **What happens if not capitalized**:  
   If you don’t capitalize a component name (e.g., `<useCallbackExample />`), React will treat it as a string (or HTML element) and won't render it as a React component. This results in an error or unexpected behavior as life-cycle methods won't apply.

**What are hook rules?**
1. **Call Hooks at the Top Level**: Always use hooks at the top level of your component, not inside loops, conditions, or nested functions.
2.  **Only Call Hooks from React Functions:** Hooks should only be used in React function components or custom hooks, not in regular JavaScript functions or class components.

**Why these rules are imp/neccessary?**
1. React needs to track hook calls in a predictable order, so calling hooks in loops or conditions can disrupt this order, leading to bugs.
  - Eg: hooks like `useState` and `useEffect` rely on a specific order to maintain component state and side effects correctly. What if we run `useState` conditionally & `useEffect` is expecting it in dependency array?
2. Hooks should only be used within React function components or custom hooks to align with React's component lifecycle.
  - like using hooks outside of these places (e.g., in regular JavaScript functions or class components) would disconnect them from React’s lifecycle, leading to unpredictable behavior.

## Custom hooks
1. A custom hook is a user defined function that allows you to **extract stateful logic or side effects** so that it is **reusable** across multiple components.
2. It is useful in **managing complex logic outside of components**, so that we can concentrate on UI in functional components.
3. **Custom hooks follow a naming convention**:
   1. the name of a custom hook should start with `use` (e.g., `useFetch`, `useForm`) so that react identify them as hooks & make them follows the rules of hooks
   2. Like only use hooks inside functional components(applies state & useffect)
   3. And only call at top level(not conditional - so it will be predictable by react)

example: useFetch - extract fetching logic seperately & reuse in multiple files

**useFetch.js (Custom Hook)**
```js
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch(url);
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return { data, loading, error };
}

export default useFetch;
```
**Users.js** - used fetch API result here
```js
import React from 'react';
import useFetch from './useFetch';

const Users = () => {
  const { data, loading, error } = useFetch('https://jsonplaceholder.typicode.com/users');

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;

  return (
    <ul>
      {data.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
};

export default Users;
```

**How re-render trigger with custom hook?**
1. Even though the state is declared inside the custom hook, it's tracked as part of the calling component’s state.
2. So when the state updates, the component re-renders just like it would if useState were used directly inside the component.
   
**Why Custom Hooks Instead of Regular Functions?** - To access react hooks like useState & useEffect

**Custom Hooks vs Higher-Order Components (HOCs)**

| Feature              | **Custom Hooks**                         | **Higher-Order Components (HOCs)**                             |
|----------------------|------------------------------------------|---------------------------------------------------------------|
| **Wrapper Component** | No additional wrapper component required | Requires a wrapper component around the original component, additional layer in UI    |
| **Integration**       | Simple, can be used directly in multiple components | Requires passing additional props down the component tree    |
| **Complexity & usage**        | Less complex, focused on logic           | More abstract, logic + UI            |
| **Prop Drilling & composition**     | No need for prop drilling                | May result in prop drilling across layers when more than 1 HOC used                   |
| **Testability**       | Easier to test in isolation   | Testing can be more complex due to component wrapping        |

## internationalization (i18n) 
Internationalization, or i18n, is the process of designing software that can be adapted to different languages and cultures. Using external libraries we can easily translate and adapted for global markets.

**using react-i18next library**
```js
import { useTranslation } from 'react-i18next';
const App = () => {
  const { t } = useTranslation();
  return (
    <div>
      <h1>{t('helloWorld')}</h1>
      <p>{t('welcomeMessage', { name: 'John' })}</p>
    </div>
  );
};
```

this packages uses combination of factors to determine which language to select like 
**Browser Language:** The language set in the browser's settings.
**Cookie**: A cookie named i18nextLng that stores the user's preferred language.
**Query Parameter**: A query parameter named lng in the URL.
**Navigator Language:** The language set in the navigator object (e.g., navigator.language).
**Fall back lang**

How to handle language change through code
```js
import { useTranslation } from 'react-i18next';

function MyComponent() {
  const { t, i18n } = useTranslation();

  // Change the language to Spanish
  const handleLanguageChange = () => {
    i18n.changeLanguage('es');
  };

  return (
    <div>
      <button onClick={handleLanguageChange}>Change to Spanish</button>
      <p>{t('hello')}</p>
    </div>
  );
}
```

## Localization (L10n)
Localization, or L10n, is the process of adapting software to a specific language, culture, or region. This involves translating software into a local language, adapting it to local customs, and modifying it to comply with local laws, regulations, and standards. The goal of localization is to make software feel natural and relevant to users in a particular region.

same **react-i18next** library can be used to localize

## How React supports accessibiliy
Accessibility in digital applications refers to the design and development of products that can be used by people with disabilities, like visual. So it's imp to follow some standard to make screen readers work efficiently

**How React Achieves Accessibility**
1. **Semantic HTML:** Use semantic HTML elements to provide meaning to the structure of your content.
2. **ARIA attributes:** Add ARIA(**Accessible Rich Internet Applications**) to dynamic content to make it accessible to screen readers like `aria-label, aria-role, aria-pressed, aria-selected` etc.
3. **Focus management:** Manage focus states to ensure keyboard navigation works correctly.
4. **Accessibility props:** Use props like `alt, role` to provide accessibility information.
5. **Good contrast ratio of colors**

**To test accessibility of an app**
1. Go to developer tool & go to lighthouse
2. check fot accessibility audit & get report
3. check screen readers to play

## React.createRoot
ReactDOM.createRoot is part of the new API in React 18, designed to enable features like Concurrent Rendering, which allows React to work on multiple tasks simultaneously without blocking the main thread.

```js
import React from 'react';
import ReactDOM from 'react-dom/client'; // Use `react-dom/client` instead of `react-dom`

const App = () => {
  return <h1>Hello, World!</h1>;
};

// Create a root using the new API
const root = ReactDOM.createRoot(document.getElementById('root'));

// Render the application
root.render(<App />);
```

**Need to write more on this** like what is need? what is concurrent mode etc

## Server-Side Rendering & Search Engine Optimization
**What is SSR (Server-Side Rendering)**
SSR is a technique where web pages are rendered on the server and sent as fully rendered HTML to the browser, improving initial page load and SEO.

**What is SEO?:** SEO (Search Engine Optimization) refers to the practice of improving the visibility of a website on search engines like Google, making it easier for users to find your site based on relevant keywords.

- **How SSR improves SEO:** SSR involves rendering the initial HTML content of a web page on the server, rather than on the client-side. This allows search engines to crawl and index the content more easily, which can improve a website's visibility in search results without waiting for JavaScript to load.

- **Does React have SSR?:** React by itself doesn't have built-in SSR, but it can be implemented using ReactDOMServer. Frameworks like Next.js provide SSR support out of the box for React.

## What is JSX?
1. JSX means JavaScript XML – it's a syntax extension of JS
2. It lets you write HTML-like code in JavaScript, so we combine markup & logic into 1 file.
3. So, easy to write & maintain
4. Used in React to describe the UI in return statement.
5. Even though it looks like HTML, but it's actually JavaScript syntax.
6. JSX will internally converted into React.createElemet(tag, props, ...children);
7. JSX code must return a single parent element as JSX is still JS & react functional component is a fuction which expect only 1 statement in return. So wrap it in `<div>{...}</div>` or `<Fragment>{...}</Fragment>` / `<>{...}</>`
8. some diffs are btw HTML & JSX are:
   - class => className. As class is reserved keyword in React <br>
     `<div className="box"></div>`
   - for -> htmlFor. As for is reversed keyword for `for` loop in react. <br>
     `<label htmlFor="name">Name</label>`
   - Self closing tag must end with /, HTML is forgiving it will ignore & auto correct even if we forget but error in react
     ```JSx
       <img src="logo.png" />
       <input type="text" />
     ```
9. We can use JS in JSX for dynamic code using {}. <br> eg: `<h1>{username}</h1>`  
   
## React vs Vue vs Angular

| 🔹 Feature | ✅ **React** | 🔶 **Vue** | 🔴 **Angular** |
|-----------|---------------|-------------|----------------|
| **Ease of Learning** | Uses plain JS + JSX — very beginner-friendly. | Easy to start, but has its own template syntax. | Complex — requires TypeScript, decorators, and extra structure. |
| **Flexibility** | Just a library — choose your own routing, state, etc. | Framework-lite — some structure, some freedom. | Full framework — opinionated and rigid. |
| **Logic Reuse** | Powerful **Hooks** (`useState`, `useEffect`) for clean and modular logic sharing. | Uses Composition API (good, but newer and less intuitive). | Uses services, DI, and boilerplate-heavy structure. |
| **UI + Logic Structure** | JSX = UI and logic in one place — easier maintenance. | Logic and templates are split — more separation. | UI in HTML templates, logic via TypeScript classes + decorators. |
| **Community & Jobs** | Huge ecosystem, tons of jobs, backed by Meta, used by top companies (Netflix, Instagram). | Strong OSS community, mostly used in smaller teams/projects. | Popular in enterprise, but less startup adoption, fewer jobs. |
| **Performance** | Virtual DOM + fine-tuned control (`React.memo`, `useCallback`) for fast updates. | Also uses Virtual DOM, good speed, less customization. | Heavier due to **Zone.js** and complex change detection system. |
| **Cross-Platform** | Write mobile apps using **React Native** — same code style. | Vue Native exists but not widely used. | Uses **Ionic** (separate library, bulkier setup). |

## Error Boundary:
1. Error Boundary is wrapper component, which handles JavaScript runtime errors during rendering and in lifecycle methods of child components.
2. For example, when props are null or undefined(not passed) & we are doing some action on it in render phase undefined.toUpperCase() in return(**render phase**)
3. Or some went wrong in lifecycleMethods(in class based components). But in devlopement **useEffect** & we throw an error this is working but in production it might not work it seems
4. They don't catch async errors - use try...catch
    - Syntax/compile-time error
    - Errors in event handlers
    - Asynchronous errors (e.g., setTimeout, fetch)
    - Errors outside React tree (like service workers)
5. Currently, error boundaries must be class components. React might support functional boundaries with hooks like useErrorBoundary in the future (experimental).
6. NOTE: Always wrap the component for which you want handle runtime error not inside component

```JS
import React from 'react';

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Update state so the next render shows fallback UI
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    // Log error details to an error reporting service
    console.error("Caught an error:", error, info);
  }

  render() {
    if (this.state.hasError) {
      return <h2>Something went wrong.</h2>;
    }

    return this.props.children;
  }
}

export default ErrorBoundary;
```
```JS
//wrap for components
import ErrorBoundary from './ErrorBoundary';
import MyComponent from './MyComponent';

function App() {
  return (
    <ErrorBoundary>
      <MyComponent />
    </ErrorBoundary>
  );
}
```

## React 18+ benefits
1. Automatic batching: Before 18+, react didn't batched state updates outside react control, now it's batching everywhere increase performance by reducing no. of re-renders
   
| Environment                            | Example                                                            | Batching Before React 18 | Batching in React 18+ |
| -------------------------------------- | ------------------------------------------------------------------ | ------------------------ | --------------------- |
| React event handlers                   | `<button onClick={...} />`                                         | ✅ Yes                    | ✅ Yes                 |
| `useEffect`                            | `useEffect(() => { setA(1); setB(2); }, [])`                       | ✅ Yes                    | ✅ Yes                 |
| `setTimeout`, Promises -> .then(), async/await    | `await fetch(...); setA(1); setB(2);`                              | ❌ No                     | ✅ Yes                 |
| Native DOM events (`addEventListener`) | `document.addEventListener("click", () => { setA(1); setB(2); });` | ❌ No                     | ✅ Yes                 |

## Why immutability is important?
1. Immutability means not modifying the original object/array, but instead creating a new copy with the desired changes. This is imp bcz of following reasons:
2. React re-renders a component only if the state or props change — and it does this reference quality check incase of objects & arrays
3. In React.memo, it uses reference equality check to determine whether props changed or not.
4. So, immutability is imp to make react predicatable, for debugging & to get expected results
5. Always copy the obj into another var & use it, consider deep copy for nested objects
   ```JS
   setItems([...items, newItem]); // for arrays
   setUserData({ ...data, name: 'Routhu' }); // for objects
   ```

## Keys in react
1. A key is a **special prop** in React that must be provided for each element in a **dynamically generated list**.
2. It helps React identify which items have been added, removed, or updated during re-renders.
3. If you don’t provide a key explicitly, React uses the array index as the default key (i.e., 0, 1, 2…). This can lead to bugs when the list is reordered or changed.
4. For example, consider a list of student names rendered as <Student /> components, each containing a contentEditable <div>. If you type inside one of the editable areas and then reverse the list, React—relying on index-based keys—will reuse the DOM at that same index, even though the content has logically moved. As a result, your typed content may appear under the wrong student.
5. `["Mounika", "Rohith", "Krishna"]` => In dom 0 1 2 -> even when we reverse it `["Krishna", "Rohith", "Mounika"]` keys remain same 0, 1, 2. So, react think like no change happened in lit items, so won't change the content editable div.
6. To prevent this, you should always provide a **stable and unique key** (like an ID or unique name) for each list item. This ensures React can track elements accurately and preserve their state correctly during updates.

```JS
const Student = ({ name }) => {
  return (
    <>
      <p>Name: {name}</p>
      <div contentEditable></div>
    </>
  );
};

const App = () => {
  const [studentList, setStudentList] = useState([
    'Mounika',
    'Rohith',
    'Krishna',
  ]);

  return (
    <>
      <button onClick={(e) => setStudentList([...studentList.reverse()])}>
        Reverse student order
      </button>
      {studentList.map((name) => (
        <Student key={name} name={name} /> // key is imp
      ))}
    </>
  );
}
```
