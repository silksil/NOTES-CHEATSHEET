# User Cases
## Array with objects =>  Parent objects with keys (category) and values (array with (child)objects) => Child object assigned to parent's object key based on value in child object

### Example 1
#### Problem
A function will be called with an array of objects as its argument. The objects represent people, with a property `name` and a property `age`. Your function should group each person **over the age of 18** into age ranges. Here are a few examples:

  ```js
  // Example 1
  groupAdultsByAgeRange([{name: "Henry", age: 9}, {name: "John", age: 20}])
  // Should result in:
  const result = { '20 and younger': [ { name: 'John', age: 20 } ] }

  // Example 2
  groupAdultsByAgeRange([{name: "Anna", age: 31}, {name: "John", age: 32}, {name: "Hank", age: 60}])
  // Should result in:
  const result2 = {
    '31-40': [ { name: 'Anna', age: 31 }, { name: 'John', age: 32 } ],
    '51 and older': [ { name: 'Hank', age: 60 } ]
  }
  ```

#### Solution
```js
array = [
      { name: "pete", age: 10 },
      { name: "dove", age: 17 },
      { name: "dave", age: 18 },
      { name: "hall", age: 19 },
      { name: "donn", age: 20 },
      { name: "trey", age: 21 },
      { name: "hann", age: 29 },
      { name: "chew", age: 30 },
      { name: "cloe", age: 31 },
      { name: "dart", age: 39 },
      { name: "this", age: 40 },
      { name: "dame", age: 41 },
      { name: "henk", age: 49 },
      { name: "anna", age: 50 },
      { name: "olga", age: 51 },
      { name: "bert", age: 52 },
      { name: "oldd", age: 120 },
    ]

function getCategory(age){
  let cat = '';
  if(age <= 20) {
    cat = '20 and younger';
  }
  if(age > 20 && age <= 30) {
    cat = '21-30';
  }
  if(age > 30 && age <= 40) {
    cat = '31-40';
  }
  if(age > 40 && age <= 50) {
    cat = '31-40';
  }
  if(age > 50) {
    cat = '51 and older';
  }
  return cat;
}

function groupAdultsByAgeRange(people) {
  return people.reduce((acc, item) => {
    const category = getCategory(item.age);
    let newArray = acc.hasOwnProperty(category) ? acc[category].concat(item): [].concat(item);
    return  { ...acc,[category]: newArray }
  }, {})
};
```

### Example 2
```js
const array = [1, 2, 3, 5, 6, 9];

function splitEvens(acc, item){
  if( item % 2 === 0 ){
    return { even: acc.even.concat([item]), odd: acc.odd}
  }
  else return { even: acc.even, odd: acc.odd.concat([item]) }
}

array.reduce(splitEvens, { even: [], odd: [] }))
```

## Delete a portion of a string starting from a certain character
```javascript
let x = 'silkie.asd'
let y  = x[0].substring(0, x.indexOf('.'));
```

## Calculate total of array with items
```js
arr.reduce((total, amount) => total + amount);
```

## Use sort() and localCompare() to sort a list based on alphabet
```javascript
let items = ['réservé', 'premier', 'baguette', 'cliché', 'communiqué', 'café', 'adieu'];
console.log(items.sort((a, z) => a.localeCompare(z))); // expected output: ['adieu', 'baguette', 'café', 'cliché', 'communiqué', 'premier', 'réservé']
```


## Transfrom an array with objects to an object that has objects with a key of a certain property (e.g. id)
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

## Get get unique values out of array with Set()
```javascript
const values = ['a', 'a', 'b', 'b', 'c', 'd'];

function storeUniqueValues(arr) {
    return Array.from(new Set(arr));
}

console.log(storeUniqueValues(values)); // expected output: [ 'a', 'b', 'c', 'e' ]
```


