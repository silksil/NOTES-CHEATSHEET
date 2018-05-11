
### Create and clone object
`newjsonobj = Object.assign({}, jsonobj, {})`


### Loop trough nested objects
```javascript
function loopNestedObject(obj) {
  for (let key in obj ) {
    console.log(key)
    for (let key2 in obj[key]) {
      console.log(obj[key][key2])
    }
  }
}

const books = {
   book1: {
     bookTitle: 'Forty laws of love',
     language: 'Arabic',
     author: 'Elif Şafak'
   },
   book2: {
     bookTitle: 'Alchemist',
     language: 'Arabic',
     author: 'Paulo Coelhho'
   }
}

loopNestedObject(books)

/*
  returns:
  book1
  Forty laws of love
  Arabic
  Elif Şafak
  book2
  Alchemist
  Arabic
  Paulo Coelhho
*/
```




