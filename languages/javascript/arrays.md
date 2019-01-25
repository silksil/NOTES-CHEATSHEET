# Learning Goals
- What does includes() do?
- What does filter() do?
- What does map() do?
- What does slice() do?
- What does reduce() do?
- What does join() do?
- What does sort() do?

# Methods
## Includes
```javascript
let status = ["status1", "status2", "status3"]

if (status.includes(checkStatus)) {
  //doSomething
}
```

## Filter
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

## Map
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
## forEach()
Executes a provided function once per array element.The main difference that you need to know is .map() returns a new array while .forEach() doesn't.
```js
let arr = [1, 2, 3, 4, 5];

arr.forEach((num, index) => {
    return arr[index] = num * 2;
});
//result arr = [2, 4, 6, 8, 10]
```
## Slice
`splice()` (not `splice()`) you cut of items of an array based on location. `Splice()` does the same, but instead not copies but mutates the array.
```javascript
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];

console.log(animals.slice(2));
// expected output: Array ["camel", "duck", "elephant"]
console.log(animals.slice(2, 4));
// expected output: Array ["camel", "duck"]
```

## Reduce
`reduce()`reduces the array to a single value, it provides a function for each value of the array (from left-to-right) and the return value of the function is stored in an accumulator (result/total).

## Join
`join()` the elements of an array into a string.
```javascript
const printItems = (arr) =>
  arr.map((currElement, index) => {
   return index + ". " + currElement;
 }).join('\n')

console.log(printItems(list))
```

## Sort
Array.sort takes an optional compare function as an argument. If you pass in this function, you will have to write it yourself. The function receives 2 arguments, a and b, that can be 2 random elements on the array. Your compare function will need to figure out whether the 2 elements are equal, or one should be ranked higher than the other. It has the following form:
```js
function compare(a, b) {
  if (a is less than b by some ordering criterion) {
    return -1; // or any negative number
  }
  if (a is greater than b by the ordering criterion) {
    return 1; // or any positive number
  }
  // a must be equal to b
  return 0;
}
```
The result of sort is a new, sorted array.
```js
const movies = [
  {
    title: 'The Godfather',
    rating: 9.2,
    released: 1972,
    votes: 100
  },
  {
    title: 'The Shawshank Redemption',
    rating: 9.2,
    released: 1994,
    votes: 101
  },
  {
    title: 'The Dark Knight',
    rating: 8.9,
    released: 2008,
    votes: 200
  },
  {
    title: 'Star Wars: Episode IV - A New Hope',
    rating: 8.6,
    released: 1977,
    votes: 999
  }
]

const moviesByYear = movies.sort((a, b) => {
  return b.released - a.released
})

console.log(moviesByYear)

const moviesByRating = moviesByYear.sort((a, b) => {
  return b.rating - a.rating
})

console.log(moviesByRating)
```
