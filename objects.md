
### Create and clone object
`newjsonobj = Object.assign({}, jsonobj, {})`

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





