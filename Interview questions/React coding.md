1. Write a component to create a textbox & button on button click should add items to the list. List should be displayed below text box with delete icon. it should be deleted from list if clicked on icon

## guess output

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
