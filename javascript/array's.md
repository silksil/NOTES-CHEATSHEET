### Filter
`Filter()` expects a callback function to return a true or false > takes away items thare are false.

```javascript
let books = [
  {name: 'The Alchemist', author: 'Paulo Caulho', language: 'Portuguese'},
  {name: 'Shiddharta',author: 'Herman Hesse', language: 'German'},
  {name: 'The Art of Learning',author: 'Joshua Waitzkin', language: 'English'}
]

let englishBooks = books.filter((book) => {
  return (book.language === 'English');
});

/** englishBooks returns:
[ { name: 'The Art of Learning',
    author: 'Joshua Waitzkin',
    language: 'English' } ]
**/
```

### Map1
`Map()` includes all items, but transform every individual item
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


### Map2
```javascript
const list = [ 'h', 'i'];

const printItems = (arr) =>
  arr.map((currElement, index) => {
   return index + ". " + currElement;
  })

console.log(printItems(list)) //expected output: [ '0. h', '1. i' ]
```

### Splice
`splice()` changes the contents of an array by removing existing elements and/or adding new elements.
```javascript
let months = ['Jan', 'March', 'April', 'June'];
months.splice(1, 1);
// deletes index 1
console.log(months);
// output: Array ["March", "April", "June"]

months.splice(4, 1, 'May');
// replaces 1 element at 4th index
console.log(months);
// output: Array ['Jan', 'Feb', 'March', 'April', 'May']
```

### Reduce
`reduce()`reduces the array to a single value, it provides a function for each value of the array (from left-to-right) and the return value of the function is stored in an accumulator (result/total). Examples: https://medium.freecodecamp.org/reduce-f47a7da511a9
``` javascript
const euros = [29.76, 41.85, 46.5];

const sum = euros.reduce((total, amount) => total + amount); 

sum // 118.11
```
### Join()
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


