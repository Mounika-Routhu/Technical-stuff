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




