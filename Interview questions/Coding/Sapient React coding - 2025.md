## Producs.js
```JSX
import React, { useState, useEffect } from 'react';

import Product from './Product';

const Products = () => {
  const [productsData, setProductsData] = useState([]);
  const [sortOrder, setSortOrder] = useState('asc');

  const sortHandler = (e) => {
    setSortOrder(e.target.value);
  };

  const sortProductsByPrice = (res = productsData) => {
    console.log(sortOrder);
    const sortedData = [...res];
    if (sortOrder === 'asc') {
      sortedData.sort((a, b) => a.price - b.price);
    } else {
      sortedData.sort((a, b) => b.price - a.price);
    }
    setProductsData(sortedData);
  };

  const fetchProducts = async () => {
    try {
      let res = await fetch('https://fakestoreapi.com/products');
      if (res.ok) {
        res = await res.json();
        console.log(res);
        sortProductsByPrice(res);
      }
    } catch (err) {
      console.log(err);
    }
  };

  useEffect(() => fetchProducts(), []);

  useEffect(() => sortProductsByPrice(), [sortOrder]);

  return (
    <>
      <h2>Products List</h2>
      <select id="sort-list" onChange={sortHandler}>
        <option value="asc">ASC</option>
        <option value="desc">DESC</option>
      </select>
      {productsData.length > 0 ? (
        productsData.map((p) => <Product key={p.id} item={p} />)
      ) : (
        <div>Loading...</div>
      )}
    </>
  );
};

export default Products;
```

## product.js
```JSX
import React from 'react';

import './products.css';

const Product = ({ item }) => {
  return (
    <div className="product">
      <p>{item.title}</p>
      <p>{item.price}</p>
      <img src={item.image} alt={item.description} />
    </div>
  );
};

export default Product;
```

## product.css
```
.product img {
  max-width: 20%;
  height: fit-content;
}
```
