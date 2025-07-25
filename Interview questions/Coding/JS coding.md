## format Date
```JS
const formatDate = (str) => {
  let date = new Date(str); //invalid dates become null
  console.log(date.valueOf(), isNaN(date)); // while doing isNaN(), data is coverted to number using valueOf to timestamp
  if (date instanceof Date && !isNaN(date)) {
    // return date.toISOString().split('T')[0]; //easy way
    return [
      date.getFullYear(),
      String(date.getMonth() + 1).padStart(2, 0), // month starts from 0
      String(date.getDate()).padStart(2, 0),
    ].join('-');
  } else {
    return 'Not a valid date';
  }
};

console.log(formatDate('2025-06-01T10:00:00'));
```
<img width="342" alt="Screenshot 2025-07-04 at 1 44 25 AM" src="https://github.com/user-attachments/assets/0faab711-df37-4014-ba0e-864ef139cbee" />

Note: 
1. new Date("abc") instanceof Date // true
2. string, null, undefined, NaN => still instance of Date but gives isNaN //false
   
## generate lorem ipsum
notes: 
1. ASCII -> A-Z(65-90), a-z(97-122) 
2. Math.random() -> generates a floting number 0 to 1(exclusive).
3. So, if you want a num btw 1 to 10 ->
   - Math.ceil(Math.random() * 10)
   - Math.floor(Math.random() * 10) + 1
```JS
const generateLoremIpsum = (wordCount) => {
  let words = ['mouni', 'ipsum'];
  if (wordCount <= 2) {
    return words.slice(0, [wordCount]).join(' ');
  } else {
    for (let i = 0; i < wordCount - 2; i++) {
      const randomWordLength = Math.floor(Math.random() * 8) + 3; // min 3 letter max 10
      let word = '';
      for (let j = 0; j < randomWordLength; j++) {
        const randomLetter = String.fromCharCode(
          Math.floor(Math.random() * 26) + 97
        );
        word += randomLetter;
      }
      words.push(word);
    }
    let sentence = words.join(' ');
    return sentence;
  }
};

console.log(generateLoremIpsum(6));
```

## sort arr
```JS
const sortArr = (arr) => {
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] > arr[j]) {
        [arr[i], arr[j]] = [arr[j], arr[i]]; // Swap
      }
    }
  }
  return arr;
};

console.log(sortArr([5, 2, 8, 1])); // [1, 2, 5, 8]
```

## sort Obj by value
```JS
const obj = { "mounika": 12, "Rohit": 34, "Krishna": 10}

const sortArr = (arr) => {
    for(let i=0; i<arr.length; i++){
        for(let j=i+1; j<arr.length; j++){
            if(arr[i][1] > arr[j][1]){
                [arr[i], arr[j]] = [arr[j], arr[i]]
            }
        }
    }
    return arr;
}

const sortObj = (obj) => {
    let objArr = Object.entries(obj);
    const sortedArr = sortArr(objArr);
    objArr.sort(a, b => a[1] - b[1]);
    const sortedObj = {};
    sortedArr.forEach(el => {
        sortedObj[el[0]] = el[1];
    });
    console.log(sortedObj);
}

sortObj(obj)
```
NOTE: CHROME IS SORTING KEYS ALPHABETICALLY, SO SHOW AS MAP or STRIGIFY OBJ. FOR LOCALE COMPARE ITS IGNORING CASE SENSITIVE. SO, USE 
```JS
  if (a[0] > b[0]) {
    return 1;
  }
  if (a[0] < b[0]) {
    return -1;
  }
  return 0;
```
<img width="1108" alt="Screenshot 2025-06-27 at 1 40 22 PM" src="https://github.com/user-attachments/assets/e8687dd8-fe30-4de8-a761-57a411550611" />

Swap: `arr[i], arr[j]] = [arr[j], arr[i]]`

### Property Order Rules in Objects
When you iterate over object properties (e.g., using for...in, Object.keys(), etc.), JavaScript follows this order:
1. Integer-like keys (e.g., "0", "1", "2") — ordered numerically, even if we don't add in numerical order
2. String keys (non-integer) — ordered in the order they were added
3. Symbol keys — appear only with Object.getOwnPropertySymbols(), not Object.keys()

## count no. of vowels in string
```JS
const getVowelCount = (str) => {
    const vowels = ['a', 'e', 'i', 'o', 'u'];
    let vowelCount = 0;
    let strArr = str.split('');
    strArr.forEach(el => {
        if(vowels.includes(el)){
            vowelCount++;
        }
    });
    // for(i=0; i<str.length; i++){
    //     if(vowels.includes(str[i])){
    //         vowelCount++;
    //     }
    // }
    return vowelCount;
};

let vowelCount = getVowelCount("education");
console.log(vowelCount); // 5
```

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
without using properties
```JS
function sum(x) {
  let res = x;

  function inner() {
    if (arguments.length > 0) {
      res += arguments[0];
      return inner; // Keep chaining
    }
    return res; // When called with no args, return result
  }

  return inner;
}

console.log(sum(1)(2)()); // 3
```
without using properties & multiple args
```JS
function sum() {
  let res = 0;

  // Add all initial arguments
  for (let i = 0; i < arguments.length; i++) {
    res += arguments[i];
  }

  function inner() {
    if (arguments.length > 0) {
      for (let i = 0; i < arguments.length; i++) {
        res += arguments[i];
      }
      return inner; // Keep chaining
    }
    return res; // When called with no args, return result
  }

  return inner;
}

console.log(sum(1, 4)(2, 6)()); // 13
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

## flatten arr
```JS
const flattenArr = (arr) => {
    let res = [];
    arr.forEach(el => {
        if(Array.isArray(el)){
            const nestedRes = flattenArr(el)
            res = [...res, ...nestedRes]
        }else{
            res.push(el);
        }
    })
    return res
}

console.log(flattenArr([1,2,3, 9, [8, [5, 6], 2]]))
```

## flatten array with depth 
```JS
const flattenArr = (arr, depth) => {
    let res = [];
    arr.forEach(el => {
        if(Array.isArray(el) && depth > 0){
            const nestedRes = flattenArr(el, depth-1);
            res = [...res, ...nestedRes];
        }else{
            res.push(el);
        }
    })
    return res
}

console.log(flattenArr([1, [2, [3]], [4, [5]]], 2)); //[ 1, 2, 3, 4, 5 ]
console.log([1, [2, [3]], [4, [5]]].flat(1)); // inbuilt method => [ 1, 2, [ 3 ], 4, [ 5 ] ]
```

## find pairs which sums to target
```JS
// Input: nums = [2, 7, 11, 15], target = 9  
// Output: [0, 1] // Because nums[0] + nums[1] == 2 + 7 == 9

const findPair = (arr, target) => {
    let allPairs = [];
    for(let i=0; i<arr.length; i++){
        for(j=i+1; j<arr.length; j++){
            let res = arr[i] + arr[j];
            if(res === target){
                allPairs.push([i, j]);
            }
        }
    }
    return allPairs;
}

const arr = [2,1,3,8,2,7];
console.log(findPair(arr, 4)) [ [ 0, 4 ], [ 1, 2 ] ]
```

## find sum of min & max of array
```JS
const sumMinMax = (arr) => {
    let min = arr[0];
    let max = arr[0];
    // let min = max = arr[0] // might be confusing to avoid using, but it's valid in JS

    for (let i = 0; i < arr.length; i++) {
        if (arr[i] < min) min = arr[i];
        if (arr[i] > max) max = arr[i];
    }

    return min + max;
};

const arr = [2, 1, 4, 62, 7, 9];
console.log(sumMinMax(arr)); // Output: 63 (1 + 62)
```

## reverse string
```JS
const reverseStr = (str) => {
    let strArr = str.split("");
    // return strArr.reverse().join("");
    let res =[];
    for(let i=strArr.length-1; i>=0 ; i--){
        res.push(strArr[i])
    }
    return res.join("");
}

console.log(reverseStr("Hellow!"));
```

## 

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

