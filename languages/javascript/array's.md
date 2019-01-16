## Methods
### Filter
`Filter()` expects a callback function to return a true or false > takes away items thare are false.

```javascript
let books = [
  {name: 'The Alchemist', author: 'Paulo Caulho', language: 'Portuguese'},
  {name: 'Shiddharta',author: 'Herman Hesse', language: 'German'},
  {name: 'The Art of Learning',author: 'Joshua Waitzkin', language: 'English'}
]

const longStretches = (books) => {
  return books.filter(book => book.language === 'English')
}

/** englishBooks returns:
[ { name: 'The Art of Learning',
    author: 'Joshua Waitzkin',
    language: 'English' } ]
**/
```

### Map
`Map()` includes all items, but transform every individual item

#### Example 1
```javascript
let books = [
  {name: 'The Alchemist', author: 'Paulo Caulho', language: 'Portuguese'},
  {name: 'Shiddharta',author: 'Herman Hesse', language: 'German'},
  {name: 'The Art of Learning',author: 'Joshua Waitzkin', language: 'English'}
]

let nameBooks = books.map((book) => book.name + ' is written by ' + book.author);

/** nameBooks returns:
[ 'The Alchemist is written by Paulo Caulho',
  'Shiddharta is written by Herman Hesse',
  'The Art of Learning is written by Joshua Waitzkin' ]
**/

```
#### Example 2
```javascript
const list = [ 'h', 'i'];

const printItems = (arr) =>
  arr.map((currElement, index) => {
   return index + ". " + currElement;
  })

console.log(printItems(list)) //expected output: [ '0. h', '1. i' ]
```

### Slice
`splice()` (not `splice()`) you cut of items of an array based on location. `Splice()` does the same, but instead not copies but mutates the array. 
```javascript
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];

console.log(animals.slice(2));
// expected output: Array ["camel", "duck", "elephant"]
console.log(animals.slice(2, 4));
// expected output: Array ["camel", "duck"]
```

### Reduce
`reduce()`reduces the array to a single value, it provides a function for each value of the array (from left-to-right) and the return value of the function is stored in an accumulator (result/total). Examples: https://medium.freecodecamp.org/reduce-f47a7da511a9

#### Calculate Total
```js
const total = arr => {
  return arr.reduce((total, amount) => total + amount);
}
```

#### Even odds
```
const array = [1, 2, 3, 5, 6, 9];

function splitEvens(acc, item){
  if( item % 2 === 0 ){
    return { even: acc.even.concat([item]), odd: acc.odd}
  }
  else return { even: acc.even, odd: acc.odd.concat([item]) }
}

array.reduce(splitEvens, { even: [], odd: [] })) 
```

### Join
`join()` the elements of an array into a string:
```javascript
const printItems = (arr) =>
  arr.map((currElement, index) => {
   return index + ". " + currElement;
 }).join('\n')

console.log(printItems(list))
/*
expected output:
0. h
1. i
/*
```

## Cases
### Use Set() to get unique values out of array
```javascript
const values = ['a', 'a', 'b', 'b', 'c', 'd'];

function storeUniqueValues(arr) {
    return Array.from(new Set(arr));
}

console.log(storeUniqueValues(values)); // expected output: [ 'a', 'b', 'c', 'e' ]
```
### Use sort() and localCompare() to sort a list
```javascript
let items = ['réservé', 'premier', 'baguette', 'cliché', 'communiqué', 'café', 'adieu'];
console.log(items.sort((a, z) => a.localeCompare(z))); // expected output: ['adieu', 'baguette', 'café', 'cliché', 'communiqué', 'premier', 'réservé']
```

### Loop through nested arrays
```javascript
function addAll(arr) {
    let count = 0;
    for (let i = 0; i < arr.length; i++) {
        for (let j = 0; j < arr[i].length; j++) {
            count += arr[i][j];
        }
    }
    return count;
}

console.log(addAll([[1, 2], [3, 4], [5, 6, 7]])); //expected output: 28
```

### Transfrom an array with objects to an object that has objects with a key of a certain property (e.g. id)
```javascript
let friendList = [
  	{id: '9', name: 'Sil'},
  	{id: '10', name: 'Bert'}
  ]

const arrayToObject = (array, keyField) =>
   array.reduce((obj, item) => {
     obj[item[keyField]] = item
     return obj
   }, {})

arrayToObject(friendList, 'id') // {"9":{"id":9,"name":"Sil"},"10":{"id":10,"name":"Bert"}}
```
