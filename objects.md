
### Create and clone object
`newjsonobj = Object.assign({}, jsonobj, {})`


### Loop trough nested objects (1)
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


### Loop trough nested objects (2) with Object.keys and Object.values
```javascript
function loopNestedObjec(parentObject) {
  for (const childObject in parentObject) {
    for (let i = 0; i < Object.keys(parentObject[childObject]).length; i++) {
      return Object.keys(parentObject[childObject])[i] + ' :\t' + Object.values(parentObject[childObject])[i]
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

loopNestedObjec(books)
  
  /*
returns:
  bookTitle :     Forty laws of love
  language :      Arabic
  author :        Elif Şafak
  bookTitle :     Alchemist
  language :      Arabic
  author :        Paulo Coelhho
*/
  ```


### Access nested objects
```javascript
//scenario1
const user = {
    id: 101,
    email: 'jack@dev.com',
    personalInfo: {
        name: 'Jack',
        address: {
            line1: 'westwish st',
            line2: 'washmasher',
            city: 'wallas',
            state: 'WX'
        }
    }
}

const name = user.personalInfo.name;

//scenario2
const user = {
    id: 101,
    email: 'jack@dev.com'
}

const name = user.personalInfo.name; // not possible - cannot read property 'name' of undefined
const name = user && user.personalInfo ? user.personalInfo.name : null; //option1 - if data nested 5 or 6 levels deep, then your code will look really messy like this
const name = ((user || {}).personalInfo || {}).name; // option2 - You basically check if user exists, if not, you create an empty object on the fly

//source: https://hackernoon.com/accessing-nested-objects-in-javascript-f02f1bd6387f

```





