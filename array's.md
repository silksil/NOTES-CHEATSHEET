### Filter
`Filter` expects a callback function to return a true or false > takes away items thare are false.

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

### Map
`Map` includes all items, but transform every individual item
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


