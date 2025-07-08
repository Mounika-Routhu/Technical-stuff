## Unit Testing:
Unit testing in JavaScript involves writing test cases to verify the behavior of individual units of code—typically functions. In my work, I used **Jest**, a popular testing framework developed by Meta. Jest serves as both a **test runner** and an **assertion library**, making it easy to write and organize tests.

To implement unit tests in Jest, we use the `test` (or `it`) function. This function takes two arguments:
1. A **string** that describes what the test is supposed to verify.
2. A **callback function** that contains the actual test logic.

Inside the callback, we:
1. **Invoke** the function under test using specific input values.
2. **Assert** that the returned output matches the expected result using Jest's `expect()` API.

For example:

```javascript
function add(a, b) {
  return a + b;
}

test('adds two numbers correctly', () => {
  const result = add(2, 3);
  expect(result).toBe(5); // Assertion
});
```

## Unit Tetsing with React
### Tools Used: (default installed with Create React App(CRP))
1. **Jest** → Test runner and assertion library 
2. **React Testing Library (RTL)** → To render and test components like a user would

**example:**
1. Write **test** function from Jest take description & arrow function as params
2. inside arrow function, use **render** from RTL to render componenent VDOM,
3. use **screen.getBy*** methods from RTL to fetch data from VDOM
4. use expect from JEST to do **assertion** -> **expect(target).to*()**;
   
```JS
// Greeting.js
export default function Greeting() {
  return <h1>Hello, User!</h1>;
}
```
```Js
// Greeting.test.js
import { render, screen } from '@testing-library/react';
import Greeting from './Greeting';

test('displays greeting message', () => {
  render(<Greeting />);
  const heading = screen.getByText(/hello, user!/i);
  expect(heading).toBeInTheDocument();
});
```

**Key concepts**:
1. getBy -> throws error if element not available in screen
2. queryby -> doesn't throw error if not available in screen - better for optional content
3. findBy -> throws error if element not available in screen & for asycn(promise) operations
4. fireEvent.methods -> to trigger user event like click, focus, blur etc
5. unit test coverage -> Unit test coverage refers to how much of your code is tested by unit tests — usually shown as a percentage.
6. our project is 70%
7. to test test file -> `npm test FileName`
