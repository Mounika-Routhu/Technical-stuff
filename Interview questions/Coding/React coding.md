## Implement UI - IBM question
Write a component to create a textbox & button on button click should add items to the list. List should be displayed below text box with delete icon. it should be deleted from list if clicked on icon
```Jsx
import React, { useState, useEffect, useRef } from 'react';
import './style.css';

export default function App() {
  const [todoData, setTodoData] = useState([]);
  const todoRef = useRef('');

  const addTodo = () => {
    const newTodo = [...todoData, todoRef.current.value];
    todoRef.current.value = '';
    setTodoData(newTodo);
  };

  const deleteTodo = (todo) => {
    const index = todoData.indexOf(todo);
    const newTodoList = [...todoData];
    newTodoList.splice(index, 1);
    setTodoData(newTodoList);
  };

  return (
    <div>
      <input type="text" placeHolder="Enter todo" ref={todoRef} />
      <button onClick={addTodo}>Add</button>
      <ul>
        {todoData.map((todo) => (
          <li key={todo}>
            {todo} <button onClick={() => deleteTodo(todo)}>del</button>
          </li>
        ))}
      </ul>
    </div>
  );
}

```

## guess output - accenture hackerrack test

```js
const component = () => {
    return(
        <div>
          HI
        </div>
        <div>
          HI2
        </div>
    )
}
const mountNode = document.getElementById('root');
ReactDom.render(<component/>, mountNode);
```

**Error**: as only **1 top level element** is accepted

## Guess the o/p - accenture hackerrack test

```js
import React, {useState} from "react"

const myComponent = ({team = ""}) => {
    const [teamMate, setTeamMate] = usestate(team)
    return(<>{teamMate && teamMate}</>)
}

export default function App() {
  return <myComponent team="red bull"><myComponent/>
}
```
**Error:** SyntaxError: /App.js: **Unterminated JSX contents.**
