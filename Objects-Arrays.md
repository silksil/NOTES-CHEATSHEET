
### Create and Clone JSON Object
`newjsonobj = Object.assign({}, jsonobj, {})`


### 
```function loopNestedObject(obj) {
  for (let books in obj ) {
    console.log(books)
    for (let info in obj[books]) {
      console.log(obj[books][info])
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
// book1
// Forty laws of love
// Arabic
// Elif Şafak
// book2
// Alchemist
// Arabic
// Paulo Coelhho
*/
```




