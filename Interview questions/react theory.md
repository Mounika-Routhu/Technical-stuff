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

## Virtual DOM
- The virtual DOM is a lightweight virtual representation of the actual DOM (Document Object Model) in memory, created by React using ReactDOM library.
- When you make changes to your webpage in React, instead of immediately updating the actual DOM, React first updates the virtual DOM to reflect these changes.
- As updating the actual DOM can be slow and resource-consuming.
- Once the virtual DOM has been updated, React then performs a **"diffing"** algorithm to **compare the new virtual DOM with the previous virtual DOM**, and identify any differences between them.
- **Explain Diffing here**
- So, react always maintains 2 VDOMs - current DOM, work in progress DOM(user changes)
- Here, React never compares with Real DOM -> reading Real DOM is slow & in previous VDOM we already have JS objects available - easy to compare
- Then react updates real DOM in batches to optimize DOM writes(updating UI)
- This entire process is called as **"Reconciliation"**

### Diffing explained - How Shallow comparision sufficient in VDOM?
- **Diffing** is an algoirithm which does shallow comparison for only top level elements(i.e top node)
- **How Shallow Comparison Works:**
  - For components: React compares the props and state of the component. If any changes detected, React will re-render the component and checks for it's children util it reaches DOM elements
  - For DOM elements: React will compare the type (e.g., div, span) and props (e.g., className, style). If the type or props have changed, React will update the DOM element.
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

```JS
function MyComponent() {
        const [count, setCount] = useState(0);

        // Define a function that increments the count state
        const incrementCount = useCallback(() => {
          setCount(prevCount => prevCount + 1);
        }, []
        // }, [setCount]
)
    useEffect(() => {
        console.log("created")
    }, [incrementCount])

        return (
          <div>
            <p>Count: {count}</p>
            <button onClick={incrementCount}>Increment</button>
          </div>
        );
      }
```

In above example we want the increament to be created only when once so we gave setCount(**bcz on render useState won't get trigger**) as dependency or simple empty array also will do

**Note:**
- **Recomputes every render:** functions, variables, and JSX code inside the component.
- **Does not recompute every render:** state values (useState), props, and memoized values (useMemo, useCallback if [] passed or based on dependencies).

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
- React uses reference equality for re-rendering(which got changed)
- This issue arises when we don't use dependency array properly & pass unnecssary values to it.
- Meaning, when I'm using React.memo & using this calllback function as prop & gave [count] then child component will render everytime on change of count. This will make React.memo **useless**
- here we don't need count to be added as a dependency
- Including count in the dependency array of useCallback can make sense when the function needs to **update its behavior** based on the updated count value. explain in below note

**VV IMP Note**:
  > when we use `setCount(count+1)` then [count] needed  if not this takes count as 0(initial value) always
  > only when you use `setCount(prev => prev+1)` count in [count] is not needed bcz prevState always fetches prev state value

```javascript
// Child component using React.memo to prevent unnecessary re-renders
const ChildComponent = React.memo(({ onClick }) => {
  console.log('Child rendered');
  return <button onClick={onClick}>Click me</button>;
});

const ParentComponent = () => {
  const [count, setCount] = useState(0);

  // Memoized callback with count as a dependency
  // The function is recreated every time `count` changes
  const memoizedIncrement = useCallback(() => {
    setCount(prevCount => prevCount + 1);
  }, [count]); // `memoizedIncrement` is recreated every time `count` changes

  return (
    <>
      <p>Count: {count}</p>
      <ChildComponent onClick={memoizedIncrement} />
    </>
  );
};
```

## React.memo

- **Purpose**: It is a higher-order component (HOC) that wraps a functional component to prevent re-rendering of the component if its props haven't changed.

- **Usage**: `React.memo(Component)` is used to wrap a component to ensure that the component only re-renders when its props change.

```javascript
const MyComponent = React.memo((props) => {
  // Your component logic
  return <div>{props.name}</div>;
});
```
- **When to use:** It's most useful for components that receive props but don't depend on state or context and when you want to avoid unnecessary re-renders when the props haven't changed.
- eg: a component receives a list as a prop to display in a certain way, no user interaction, read-only purpose

## HOC - higher order component
- A HOC is a function that takes a **component as an argument and returns a new component**.
- HOCs **don’t modify the original component** but create a **new enhanced version** of it.
- They are used to **share logic between components** (e.g., adding authentication, loading spinner, logging, data fetching, theming, etc.)

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

## useState - setState - access prev state
  1. ```set[State](updater);```
  2. **updater:** Can either be an object or a function.
       - **value/expression:** Directly update state.
       - **Function:** (prevState) => prevState + 1
  3. It's recommended to use prevState when new state value depends on prevState (eg:increment count)
  4. Detailed explanation abt why to use prevState:
     - State updates are asynchronous, and React may batch multiple setState calls together. When this happens, React doesn't immediately apply each state update, which can cause issues if the new state depends on the previous state.
     - For instance, when you call setCount(count + 1) multiple times, React batches these updates, but it doesn't have the latest state value in each call, so it may apply each setCount with the old state value.
    
  ```JS
const [count, setCount] = useState(0);
setCount(5);  // Sets the state to 5
setCount(prevCount => prevCount + 1);  // Increment state based on previous state
  ```
**Note:** Interesting fact
1. in class based componenets setState also gets an optional second component callback - to excecute some logic after state update
   ```this.setState(updater, [callback]) ```
2. in functional based we can use ```useEffect``` for such requirement

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

## `useContext` practical example - refer useContext.md file

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

**What are hook rules?**
1. **Call Hooks at the Top Level**: Always use hooks at the top level of your component, not inside loops, conditions, or nested functions.
2.  **Only Call Hooks from React Functions:** Hooks should only be used in React function components or custom hooks, not in regular JavaScript functions or class components.

**Why these rules are imp/neccessary?**
1. React needs to track hook calls in a predictable order, so calling hooks in loops or conditions can disrupt this order, leading to bugs.
  - Eg: hooks like `useState` and `useEffect` rely on a specific order to maintain component state and side effects correctly. What if we run `useState` conditionally & `useEffect` is expecting it in dependency array?
2. Hooks should only be used within React function components or custom hooks to align with React's component lifecycle.
  - like using hooks outside of these places (e.g., in regular JavaScript functions or class components) would disconnect them from React’s lifecycle, leading to unpredictable behavior.

## Custom hooks
1. A custom hook is a function that allows you to **extract and reuse stateful logic or side effects** in a way that is **reusable** across multiple components.
2. Custom hooks are a powerful feature in React that help in **organizing complex logic outside of components**, making it **easier to share and manage logic.**
   
**Custom hooks follow a naming convention**: the name of a custom hook should start with the word use (e.g., `useFetch`, `useForm`) to make so that react identify them as hooks & make them follows the rules of hooks.

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
1. When you use a custom hook inside a component, React will track the state or values that the custom hook returns.
2. If the state inside the custom hook changes (for example, via a setState or dispatch call), it will trigger a rerender of the component using that hook.
   
**Why Custom Hooks Instead of Regular Functions?** - To access to React’s State and Effects:

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
   
