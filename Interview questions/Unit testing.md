## Unit Testing:
Unit testing in React means testing individual components or logic (like a form, button, or utility function) in isolation, to ensure they behave correctly.

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
2. queryby -> doesn't throw error if not available in screen
3. findBy -> throws error if element not available in screen & for asycn(promise) operations
4. fireEvent.methods -> to trigger user event like click, focus, blur etc
5. unit test coverage -> 
