## fibonacci series with cache
```JS
function fibonacci(n) {
  if (n <= 1) return n;

  // Use the function's own property as a cache
  if (fibonacci.cache[n] != null) return fibonacci.cache[n];

  fibonacci.cache[n] = fibonacci(n - 1) + fibonacci(n - 2);
  return fibonacci.cache[n];
}
fibonacci.cache = {};

console.log(fibonacci(10)); // 55
```

## currying for n elements
```JS
function add(a) {
  let sum = a; // start with the first number

  function inner(b) {
    sum += b; // keep adding
    return inner; // return itself to allow chaining
  }

  inner.end = () => sum; // a custom method to stop chaining and get the total

  return inner;
}
```
## Group anagrams from given array 
```javascript
const input = ["eat", "ate", "army", "mary", "hello", "tea"]
//expected output = [ [ 'eat', 'ate', 'tea' ], [ 'army', 'mary' ], [ 'hello' ] ];

const groupAnagrams = (input) => {
    const res = {};
    input.forEach((el, idx) =>{
        let sortedKey = el.split("").sort().join("")
        if(res[sortedKey]){
            res[sortedKey] = [...res[sortedKey], el]
        }else{
            res[sortedKey] = [el]
        }
    })
    return Object.values(res);
}

console.log(groupAnagrams(input))
```

## flatten object with prefix
```javascript
const Input = {
   test:123,
   test1:{
      test2:456,
      test3:{
        test4:678
      }
   },
   test5:[1,2,3,5]
}

/*Output = {
  test: 123,
  test1_test2: 456,
  test1_test2_test3_test4: 678,
  test5: [1, 2, 3, 5],
}; */


const flatten = (obj, prefix = []) => {
  let res = {};
  for (let k in obj) {
    if (typeof obj[k] === "object" && obj !== null && !Array.isArray(obj[k])) {
      const nestedFlat = flatten(obj[k], [...prefix, k]);
      res = { ...res, ...nestedFlat };
    } else {
      if (prefix.length) {
        res[[...prefix, k].join("_")] = obj[k];
        prefix.push(k);
      } else {
        res[k] = obj[k];
      }
    }
  }
  return res;
};

console.log(flatten(input));
```

How to group by and sum an array of objects?
ARRAY Input : [
  {
    "points": 100,
    "expiryDate": "2023-11-30T00:00:00.000+00:00",
    "ledgerId": 3714
  },
  {
    "points": 10000,
    "expiryDate": "2023-12-25T00:00:00.000+00:00",
    "ledgerId": 373
  },
  {
    "points": 100,
    "expiryDate": "2023-12-31T00:00:00.000+00:00",
    "ledgerId": 3892
  },
  {
    "points": 100,
    "expiryDate": "2023-12-31T00:00:00.000+00:00",
    "ledgerId": 3894
  },
  {
    "points": 5603.22,
    "expiryDate": "2023-12-31T00:00:00.000+00:00",
    "ledgerId": 1932
  }
]
-------SOLUTION-------
using forEach lengthy method(only works if condition date is line by line)
        let preFormattedDate = '';
        let pointsSum = 0;
        let res = []
        filterdArr.forEach((item: any, key: any) => {            
            if(item.expiryDate){
                let orginalDate = new Date(item.expiryDate.split('T')[0]);
                let getMonth = orginalDate.toLocaleString('default', { month: 'long' });
                let getDay = orginalDate.getDate();
                let getYear = orginalDate.getFullYear();
                let formattedDate = getDay + suffixDate(getDay) + " " + getMonth + " " + getYear;                
                if(filterdArr.length === 1){
                    res.push([item.points, formattedDate])
                }else{
                    if(preFormattedDate === '' || preFormattedDate === formattedDate){                       
                        pointsSum += item.points;                       
                    }else{
                        res.push([roundingOffValuesAFterDecimals(pointsSum), preFormattedDate]);  
                        pointsSum = item.points;                      
                    }
                
                    if(key === (filterdArr.length - 1)){
                        res.push([roundingOffValuesAFterDecimals(pointsSum), preFormattedDate]);  
                    }
                } 
                preFormattedDate = formattedDate;                   
            }          
        })
        return res;

------------------------------------------
Using Reduce(preferable)(this will work if dates are not in order)
        let result = [];
        filterdArr.reduce(function(res: any, item: any) {
            if(item.expiryDate){
                let orginalDate = new Date(item.expiryDate.split('T')[0]);
                let getMonth = orginalDate.toLocaleString('default', { month: 'long' });
                let getDay = orginalDate.getDate();
                let getYear = orginalDate.getFullYear();
                let formattedDate = getDay + suffixDate(getDay) + " " + getMonth + " " + getYear; 
                if (!res[formattedDate]) {
                    res[formattedDate] = {points: 0, date: formattedDate};
                    result.push(res[formattedDate])
                }
                res[formattedDate].points += item.points;
                return res;
            }
        }, {});
        return result;
========================
var maxSpeed = {
    car: 300, 
    bike: 60, 
    motorbike: 200, 
    airplane: 1000,
    helicopter: 400, 
    rocket: 8 * 60 * 60
};
var sortable = [];
for (var vehicle in maxSpeed) {
    sortable.push([vehicle, maxSpeed[vehicle]]);
}

sortable.sort(function(a, b) {
    return a[1] - b[1];
});

//[["bike", 60], ["motorbike", 200], ["car", 300],
//["helicopter", 400], ["airplane", 1000], ["rocket", 28800]]

var objSorted = {}
sortable.forEach(function(item){
    objSorted[item[0]]=item[1]
})

---
const maxSpeed = {
    car: 300, 
    bike: 60, 
    motorbike: 200, 
    airplane: 1000,
    helicopter: 400, 
    rocket: 8 * 60 * 60
};

const sortable = Object.entries(maxSpeed)
    .sort(([,a],[,b]) => a-b) //destructuring
    .reduce((r, [k, v]) => ({ ...r, [k]: v }), {}); //() are used to return an obj({}) if not { return {} } [k] this is take dynamic key values

console.log(sortable);

====================================================react basic structure===============
class App extends React.Component{
  state = {
    name:"Mouni",
    age:  21
  }
  nameClickHandler = (newName, age) => {
    this.setState({name: newName, age:age})
  }
  nameChangeHandler = (e) => {
    this.setState({name: e.currentTarget.value})
  }
  render(){
    return (
      <Person 
        name={this.state.name} 
        age = {this.state.age} 
        click={this.nameClickHandler}
        change = {this.nameChangeHandler}/>
  )}
}

const Person = (props) => {
  let name = props.name[0].toUpperCase() + props.name.slice(1)
  return (
    <div>
      <h1 onClick={() => props.click("Ammu", 23)}>{name}</h1>
      <input type="text" placeHolder="your name" onChange={props.change}/> 
      <h1>{props.age}</h1>
    </div>
  );
} 

ReactDOM.render(<App />, document.getElementById('root'));

=================uncontrolled component============
class App extends React.Component{
  constructor(props){
    super(props);
    this.focusInput = React.createRef();
  }

  componentDidMount(){
    this.focusInput.current.focus();
  }
  
  render(){
    return(
      <form>
        <input ref={this.focusInput} value="this input is focussed" />
        <input value="this input is not focussed" />
      </form>
    )
  }
}

ReactDOM.render(<App />, document.getElementById("root"))
========================================
const revWordSentence = (sentence) => {
  let words = sentence.split(" ");
  const revWords = []
  for (let word of words){
    revWords.push(word.split("").reverse().join(""));
  }
  revWordsSentence = revWords.join(" ");
  return revWordsSentence;
}
=================================
const revNumber = (num) => {
  let numStr = num.toString();
  let revNum = Number.parseInt(numStr.split("").reverse().join(""));
  return revNum;
}
=================================
const printNum = () => {
  for(let i=0; i<=5; i++){
    setTimeout(()=>{
      console.log(i)
    }, i * 1000);
  }  
}

printNum();

another way:

const printNum = () => {
  for(var i=0; i<=5; i++){
    function close(i){
      setTimeout(()=>{
        console.log(i)
      }, i * 1000);
    }
    close(i)
  }  
}

printNum();
=====================
const factorial = (num) => {
  let fact = 1
  for(let i=num; i>1; i--){
    fact *= i;
  }
  return fact;
}
=================================
const isPrime = (num) => {
  let flag = 0;
  if(num > 1){
    for(let i=2; i <= num/2; i++){
      if(num%i === 0){
        flag = 1;
        console.log("not a prime");
        break;
      }
    }
    if(flag === 0){
       console.log("a prime");
    }
  }else{
    console.log("not a prime")
  }
}
=================================
function mostCommonWord(inputString) {
  var wordsCounter = {};
  let mostCommonCounter = 0;
  let mostCommonWord = "";
  inputString.split(" ").forEach((word)=>{
    wordsCounter[word] = ++wordsCounter[word] || 0;
  });
  Object.keys(wordsCounter).forEach((word)=>{
    if (wordsCounter[word] > mostCommonCounter) {
      mostCommonWord = word;
      mostCommonCounter = wordsCounter[word];
    }
  });
  return wordsCounter;
}
=======================================
const encryptEmail = (mailId) => {
  let encrpted = mailId.split("")
  let index = encrpted.indexOf("@");
  encrpted.fill('x', 3, index)
  console.log(encrpted.join(""))
}

encryptEmail("mounika@hello.com")
=========================================
function (a, b) {
  return a.length === b.length && (a + a).indexOf(b) > -1;
};
====================================find all missing el=========================
const findMissing = (arr, n) => {
  let tempArr = [...arr];
  tempArr.sort();
  let missingArr = [];
  for(let i =0; i < tempArr.length-1 ; i++){
    let nextVal = tempArr[i]+1
    if(nextVal != tempArr[i+1]){
      tempArr.splice(i+1, 0, nextVal)
      missingArr.push(nextVal);
    }
    
    if(n && missingArr.length === n){
      break;
    }
  }
  return missingArr + " is(are) missing";
}

console.log(findMissing([1,2,4,6,3,68], 7))
========================tricky=========
var z = function (x, f = () => x){
    var x;
    var y = x;
    x = 2;
    return[x,y,f()]
  }(1)
  
console.log(z) //[2,1,1] it creates x in block and local scope also

----
function* genFunc(){
    yield 1;
    yield 2;
    return 3;
}

for (let n of genFunc()) {
    console.log(n); //1,2
}

------------------
const input=
{
   test:123,
   test1:{
      test2:456,
      test3:{
        test4:678,
        test5:{
            test6: 1678
        }
      }
   },
   test7:[1,2,3,5]
}

Output:
{
    test:123,
    test1_test2:456,
    test1_test2_test3_test4:678,
    test5:[1,2,3,5]
}

const flatten = (obj, prefix = []) => {
    let res = {}
    for(let k in obj) {
        if(typeof obj[k] === 'object' && obj !== null && !Array.isArray(obj[k])){
            const nestedFlat = flatten(obj[k], [...prefix, k])
            res = {...res, ...nestedFlat}
        }else{
            if(prefix.length){
                res[[...prefix, k].join('_')] = obj[k]
                prefix.push(k)
            }else{
                res[k] = obj[k]
            }
            
        }
    }
    return res;
}

console.log(flatten(input));

