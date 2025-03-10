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

## print 1 to 5 numbers with 1 sec delay
```javascript
for(let x = 1; x < 6; x++){ 
    setTimeout(()=>{
        console.log(x)
    }, 1000)
}
```
if give var i for loop (```for(var x = 1; x < 6; x++)```) here then var creates a gloabl variable & by the time all setTimeout async function run global var x will be come 6 hence 6 will be printed for 5 times
