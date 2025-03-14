## Need of Redux?
As the application grows our state & complex async API logic also grows. This rises problems like prop drilling, state updates become unpredictable & hard to debug. 
one solution is to use `useContext` but this has some limitations
1. All components which use context **will re-render even if only use one part of the state**
  - for example I have a context where theme & user details are stored, now my main page component only requires theme & user component only requires user but whenever theme changes both main page & user component re-renders
  - this will cause performance issues
2. In large complex applications, debugging will become difficult & hard to identify which components re-rendered & which component triggerred the state update

Better solution for complex application is **REDUX**

## REDUX
1. Redux is a **state management library** for JavaScript applications, often used with React but can be used with other JavaScript frameworks as well. 
2. It helps manage the application state in a predictable way, making it easier to understand and debug. 
3. It is based on the Flux architecture, but it simplifies many parts of it.

**Flux architecture** : It was developed by Facebook to address some challenges in handling complex data flows within web apps. But it's not widely used as this is **more complex**

**Why Use Redux?**
1. **Predictable state:** The state is centralized and follows clear rules.
2. **Debugging:** With tools like Redux DevTools, you can trace every action and state change & time travlleing capabelities.
3. **Maintainability**: Redux enforces a structured approach to managing state in larger applications.

Redux flow on high level -> Redux follows a **unidirectional data** flow where:
1. **State** is stored in a **single store**.
2. **Actions** are dispatched to signal state changes.
3. **Reducers** handle these actions and update the state accordingly.
4. The state is passed down to components, which re-render when the state changes.

**Core Concepts in Redux & How to implement it**
1. Install **Redux** and **React-Redux**: If you’re using React, you’ll need react-redux as well.
  - **React-Redux**: A library that connects Redux with React applications. This will make integration of redux into react applications easy.
    `npm install redux react-redux`
    
2. Create the **Store** with **Reducer**

  - **Store:**
    - The central repository where your application state is stored.
    - You can only have one store in a Redux application.

  - **Reducer:**
    - A reducer is a **pure function** that determines how the state of the application changes in response to an **action**.
    - **Why Reducer is pure function?** To make the application more predictable & avoid unexpected behaviour
    - **Action**: A plain JS object with type
      - **type**: This key is required in every action object and describes what type of action has occurred. Eg: `INCREMENT`
      - **payload**: This key is optional and holds additional data that is necessary to perform the action, such as values that should update the state.
   
    Note: you can use different names instead of type and payload. However, using type and payload is the common practice and is recommended for consistency.

    ```js
    const store = createStore(reducer, [preloadedState], [enhancer]);

    const store = createStore(rootReducer, initialState, applyMiddleWare(thunk))
    ```
    
    **Store.js**
    ```js
    import { createStore } from 'redux';
    
    // Initial state
    const initialState = {
      counter: 0,
    };
    
    // Reducer to handle the state changes
    const counterReducer = (state = initialState, action) => {
      switch (action.type) {
        case 'INCREMENT':
          return { counter: state.counter + 1 };
        case 'DECREMENT':
          return { counter: state.counter - 1 };
        default:
          return state;
      }
    };

    // Create Redux store
    const store = createStore(counterReducer);
    
    export default store;
    ```
what if I dont pass initial state as second arg, should I send undefined?

3. **Provider Setup**: Wrap your React application in a Provider component to allow Redux to work with React & pass store.
    
    **Index.js**
    ```js
    import { Provider } from 'react-redux';
    import React from 'react';
    import ReactDOM from 'react-dom';
    import store from './store.js';

    ReactDOM.render(
      <Provider store={store}>
        <App />
      </Provider>,
      document.getElementById('root')
    );
    ```
4. **connect Redux to React**: There are two main ways to connect Redux to your components: using `connect` (older method) or using the modern hooks (`useSelector` and `useDispatch`).
  - `useSelector`:
    - allows you to **read data** from the Redux store.
    - This hook takes a function as an argument, which maps the Redux state to the component's props.
    - In this case, `state => state.counter` is extracting the counter value from the state.
  - `useDispatch`
     - allows you to **dispatch actions** to the store.
     - This hook provides the dispatch function that allows you to send actions to the Redux store (like `INCREMENT` and `DECREMENT`).

    **App.js**
    ```js
    import React from 'react';
    import { useSelector, useDispatch } from 'react-redux';
    
    const App = () => {
      // Use useSelector to read from the Redux store
      const counter = useSelector(state => state.counter);
    
      // Use useDispatch to dispatch actions
      const dispatch = useDispatch();
      
      // Action handlers
      const increment = () => dispatch({ type: 'INCREMENT' });
      const decrement = () => dispatch({ type: 'DECREMENT' });
    
      return (
        <div>
          <h1>Counter: {counter}</h1>
          <button onClick={increment}>Increment</button>
          <button onClick={decrement}>Decrement</button>
        </div>
      );
    };
    
    export default App;
    ```

  - To access multiple values using `useSelector`:
    ```js
    // Access multiple values from the Redux store
    const { counter, user, isLoggedIn } = useSelector(state => ({
      counter: state.counter,
      user: state.user,
      isLoggedIn: state.isLoggedIn,
    }));
    ```
  - Earlier we used `connect` a HOC along with `mapStateToProps` & `mapDispatchToProps`. With introduction of hooks **React-Redux 7.1.0**
    we are using hooks approach as it is simpler

    ```js
    connect(mapStateToProps, mapDispatchToProps)(App);
    ```

5. **Creating Action Types Constants & Action Creators (Optional)**
   - **Action Types Constants:** Action Types Constants are **predefined constants** (e.g., `ADD_TODO`) that **represent action types** in Redux. They **prevent typos** and **centralize action type management** for **easier maintenance**.
   - **Action Creators:** Action Creators are **functions** that return **action objects** to be dispatched to the Redux store. They **simplify the process of creating actions** and ensure **consistency** across the app.
     
     **actionTypes.js**
     ```js
      export const INCREMENT = 'INCREMENT';
      export const DECREMENT = 'DECREMENT';
     ```
     **actions.js**
     ```js
      import { INCREMENT, DECREMENT } from './actionTypes';
      
      // Action creators for increment, decrement, and reset
      export const increment = () => ({ type: INCREMENT });
      export const decrement = () => ({ type: DECREMENT });
      };
     ```
    Then, use the action creators in your component:
    
    **App.js**
    ```js
    import React from 'react';
    import { useSelector, useDispatch } from 'react-redux';
    import { increment, decrement } from './actions'; // Import action creators
    
    const App = () => {
      const counter = useSelector(state => state.counter);
      const dispatch = useDispatch();
    
      return (
        <div>
          <h1>Counter: {counter}</h1>
          <button onClick={() => dispatch(increment())}>Increment</button>
          <button onClick={() => dispatch(decrement())}>Decrement</button>
        </div>
      );
    };
    
    export default App;
    ```
 7. **Advanced: Combine Reducers (Optional)**
    If you have multiple reducers, you can combine them using `combineReducers` from Redux.
    **rootReducer.js**
    ```js
    import { combineReducers } from 'redux';
    import counterReducer from './counterReducer'; // Your counter reducer
    
    const rootReducer = combineReducers({
      counter: counterReducer,
      todo:  todoReducer //another reducer for a different functionality in application
    });
    
    export default rootReducer;
    ```
    Then, update your store creation:
    **store.js**
    ```js
    import { createStore } from 'redux';
    import rootReducer from './rootReducer'; // Import the combined reducer
    
    const store = createStore(rootReducer);
    
    export default store;
    ```

## Redux thunk - need to study
**What is a middleware**
1. In Redux, middleware refers to functions that provide a way to extend Redux's capabilities, enabling you to interact with the store's dispatch process. 
2. Middleware sits between the dispatching of an action and the moment it reaches the reducer, allowing you to perform operations like logging, asynchronous actions (e.g., API calls), or modifying actions before they reach the reducer.

## How to add redux devtools for application?

1. Install Redux DevTools Extension in the browser
2. Install `redux-devtools-extension` in the project
   `npm install redux-devtools-extension`
3. Integrate Redux DevTools into Your Store: 2 ways
   **First approach:**
    ```JS
    import { createStore } from 'redux';
    import rootReducer from './reducers'; // Your root reducer
    
    const store = createStore(
    rootReducer,
    window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__() // This line enables the DevTools
    );
    
    export default store;
    ```
  - `window.__REDUX_DEVTOOLS_EXTENSION__`: It checks if the Redux DevTools Extension is installed in the browser.
  - `window.__REDUX_DEVTOOLS_EXTENSION__()`: If installed, it enables the DevTools by calling its function(returns an enhancer function).
  - This ensures your app works whether or not the DevTools are installed, **preventing crashes** if it's missing.

    **2nd approach:** If you're using `configureStore` from `@reduxjs/toolkit`, it's even easier since it has built-in support for Redux DevTools:

    ```javascript
    Copy
    import { configureStore } from '@reduxjs/toolkit';
    import rootReducer from './reducers'; // Your root reducer
    
    const store = configureStore({
      reducer: rootReducer,
    });
    
    export default store;
    ```
    **`@reduxjs/toolkit`** (also known as RTK)
    1. `npm install @reduxjs/toolkit`
    2. is a package designed to make working with Redux easier, faster, and less error-prone by providing better defaults, simplifying common patterns, and reducing the need for boilerplate code.
    3. `createStore`- gives support for middleware(no need to add manually),  `createSlice()` - for reducing boilerplate for creating actions & reducer etc. 
    
