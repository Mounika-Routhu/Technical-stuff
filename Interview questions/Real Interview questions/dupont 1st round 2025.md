1. What are the uses of `useRef`?
2. Explain `useSelector` and `useDispatch` in Redux.
3. Explain Error Boundaries.
   - Can’t we do it without class-based components?
   - What are the other use cases where class-based components are preferred over functional ones?
4. Explain about Higher-Order Components (HOC).
5. Explain **Debouncing** and **Throttling**.
6. Provide a **Debouncing** example.
7. What is **lazy loading**?
   - How can we achieve it?
   - How does lazy loading work internally?
8. Scenario: "I have a very time-consuming computation and don’t want to block the main thread. How can I handle this?" -> **Web Workers**?
9. Explain the difference between `await fetch(...)` and `await res.json()`.
10. Why do we need to check `res.ok` in a `try-catch` block?
11. How can we catch errors when a response fails (e.g., no 2xx response)?
12. Array methods:
    - `find`
    - `some`
    - `every`
    - `filter`
13. Explain Accessibility in depth:
    - How to achieve it using **semantic HTML elements**
    - How can we handle modal with keyboard navigation
    - how to handle close & tab navigations -> check key value
    - **Keyboard navigation**
    - **ARIA** attributes
    - **Live regions**
14. What is the difference between `package-lock.json` and `package.json`?
15. Is `package-lock.json` important?
16. Should we push `package-lock.json` to Git?
17. Explain **Localization**.
    - How can we achieve localization only for **specific elements**?
    - How can we **exclude nouns** from localization?
18. Fetch **posts** and **users** from `dummyjson.com` **in parallel**.
    - `https://dummyjson.com/users, https://dummyjson.com/posts`
    - Display each post by title  
    - Under each title: show likes and **user who posted it**  
    - You need to **match user ID** to get the user name
    - show posts in ascending order by no. of likes
    - Implement **caching** for API results  
    - Handle missing data gracefully using optional chaining `?`

