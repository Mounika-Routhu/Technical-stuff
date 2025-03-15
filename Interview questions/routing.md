# Routing in React

In React, **routing** refers to the mechanism that enables navigation between different components or views based on the URL in a Single Page Application (SPA). Since SPAs don't reload the entire page for navigation, React uses routing to dynamically display different components when the user navigates to different paths.

To implement routing in React, the most commonly used library is **React Router**.

## Key Concepts in React Router:

1. **Route**: This is used to define a path and the component that should render when the user navigates to that path.
2. **Switch**: This is used to group multiple routes. It ensures that only the first matching route is rendered.
3. **Link**: This is used to create links that navigate to different routes within your app, without refreshing the page.
4. **BrowserRouter**: This is the component that wraps your entire app to enable routing using the HTML5 history API for clean URLs.

## Basic Example:

1. **Install React Router**:
   You need to install `react-router-dom` to add routing functionality in your React app.

   ```bash
   npm install react-router-dom
   ```

2. **Basic Routing Setup in React**

To set up basic routing in React, you'll need to use `react-router-dom`. This example demonstrates how to create a simple navigation system using React Router.

### Steps:

1. **Install React Router**:

   First, you need to install `react-router-dom` in your React project.

   ```bash
   npm install react-router-dom
  ```
