## scoping - guess the o/p
```javascript
function(x, f = () => x){
 var x;
 var y=x;
 x = 2;
 return [x,y,f()]
} 
function(1); 
```
ans:[2,1,1]
**explanation:**
- x is 2, this is straight, x is initialised with 2
- y is 1, due to hoisting all declarations are moved to top. In this phase, when x will get undefined. so y should also get undefined but no. If a local variable (var x) and a function parameter (x) have the same name, the function will prioritize the function parameter over the hoisted local variable.
- f() is 1, the default value will be calculated if given as an expression/function return value when the function is invoked. Here x is 1 when the function is invoked. If there no x in paramaters then f = () => x checks for global level x if no available throws an err
