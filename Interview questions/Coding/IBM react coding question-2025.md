```JS
import React, { useState } from 'react';

const MyComponent = () => {
    const [itemCountObj, setItemCountObj] = useState({
        "Dress" : 0,
        "Tv" : 0,
        "Fridge" : 0
    });

    const incrementHandler = (el) => {
        console.log("in", el)
        setItemCountObj({...itemCountObj,  [el]: itemCountObj[el] + 1});
    }

    const decrementHandler = (el) => {
        setItemCountObj({...itemCountObj,  [el]:itemCountObj[el] - 1});
    }

    const addToCartHandler = () => {
        
    }
  
  return (
    <div>
        <ul>
        {
            ["Dress", "Tv", "Fridge"].map(el => <li key={el}>{el}, 
                <span>{itemCountObj[el]}</span>
                <div>
           <button onClick={() => incrementHandler(el)}>+</button>
          <button onClick={() => decrementHandler(el)}>-</button> 
       </div></li>)
        }
        </ul>
        
      <button onClick={addToCartHandler}>Add to cart</button>  
        <div></div>
    </div>
  );
};

export default MyComponent;

```
