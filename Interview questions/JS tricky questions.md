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
1. function params get a different scope & they get evaluted even before the function body runs
2. here, when function got invoked, x get value 1, & f forms a closure with x, later return 1
3. inside function body, var x; this is valid, as redeclaration of var is allowed in JS
4. But it will be ignored as function params & local variables(variables inside function) share same scope
5. it's like var x = 1; var x; => var gets hoisted, & assigned a value 1
6. later y = x => x already has a value 1
7. x gets reassigned with 2

```JS
Param Scope:       Function Body Scope:
x = 1              var x;   (ignored, no new var)
f = () => x        var y = x;  y = 1
                   x = 2

f() closure captures param scope x=1
```

```JS
function scope(){
    let x = 10;
    {
        var x = 9;
        console.log(x)
    }
    console.log(x)
}
scope();
```

error bcz var is not block scoped & there is no function, so it will become global variable. but let can't be redeclared, so throws an err saying **syntax err: Identifier 'x' has already been declared**

## print 1 to 5 numbers with 1 sec delay
```javascript
for(let x = 1; x < 6; x++){ 
    setTimeout(()=>{
        console.log(x)
    }, 1000)
}
```
if give var i for loop (```for(var x = 1; x < 6; x++)```) here then var creates a gloabl variable & by the time all setTimeout async function run global var x will be come 6 hence 6 will be printed for 5 times

## guess it
```JS
function implicit(){
    {
        x = 10;
        console.log(x) // 10
    }
    console.log(x) // 10
}

implicit()
console.log(x); // 10
```

tricky guess below code o/p
```JS
function implicit(){
    x = 20;
    {
        var x = 10;
        console.log(x)
    }
    console.log(x)
}

implicit()
console.log(x);
```
o/p
```JS
10
10
reference error: x is not defined
```
explanation: inside function scope hoisting happens **ONLY** bcz **we have a var declaration,  no hoisting without declaration**, so JS treats it as
```JS
function implicit(){
    var x; // hoisting during memery allocation phase of execution context for function
    x = 20;
    {
        x = 10;
        console.log(x)
    }
    console.log(x)
}

implicit()
console.log(x);
```

## increment-decrement, post-pre, primitive types, immutatble
```JS
let x = 0
console.log(x++)
console.log(++x)
console.log(x)
```

```JS
o/p :
0
2
2
```
explanation: 
1. It's true that primitive types are immutable, meaning value can't be changed. 
2. But X++ means => x = x + 1
3. so here we aren't modifying value, but reassigning value to X hence value of x change
4. also X++ means, post increment, use the value & increment
5. ++X means, pre increment, increment & use the value

```JS
let x = 5;
let y = x--;
console.log(x,y);

//o/p: 4, 5
```

## guess o/p
```js
[1, [2], [1,2, [3,4]]].join(' ')
```
o/p: '1 2 1,2,3,4'

//accenture hackerrack test
```JS
var greet = "hi";
if(true){
    var greet = "hellow"
    console.log(greet)
}
console.log(greet)
```
o/p: 
"hellow"
"hellow"

//accenture hackerrack test
```JS
var obj = {
    a : 1, b:2, c:3
}
var obj2 = {
    d : 1, e:2, c:5
}
console.log({...obj, obj2})
```
o/p: ```{a:1, b:2, c:3, d:1, e:2, c:5}```

//accenture hackerrack test
```js
var employee = {
    name : "mounika"
}
var employee2 = employee
employee2.name = "newName"
console.log(employee, employee2 )

o/p: 
{ name : "newName"}
{ name : "newName"}
```

## promises

```JS
Promise.reject("Oops")
  .then(() => console.log("Success"))
  .catch(err => console.log("Error:", err))
  .then(() => console.log("After catch"));

o/p :
Error: Oops
After catch
```

```JS
Promise.any([
  Promise.reject("Fail 1"),
  Promise.reject("Fail 2"),
  Promise.reject("Fail 3")
])
.then(value => console.log("Resolved with:", value))
.catch(err => console.log("All rejected:", err.errors));

o/p: All rejected: [ 'Fail 1', 'Fail 2', 'Fail 3' ]
```
