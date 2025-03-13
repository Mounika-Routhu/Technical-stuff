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

Flux architecture : It was developed by Facebook to address some challenges in handling complex data flows within web apps. But it's not widely used as this is more complex

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
    `bash
    npm install redux react-redux
    `
2. Create the **Store**
  - The central repository where your application state is stored.
  - You can only have one store in a Redux application.
  - 

    



