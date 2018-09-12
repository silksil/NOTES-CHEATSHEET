
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
### Take out the values
```javascript
let personValues = [];
let person = {name: 'Sil', age: 10, favouriteBook: 'Siddhartha'}

for (let key in person) {
  personValues.push(person[key]);
}

console.log(personValues); //expected output: [ 'Sil', 10, 'Siddhartha' ]
```

### Take out the keys
```javascript
let personKeys = [];
let person = {name: 'Sil', age: 10, favouriteBook: 'Siddhartha'}

for (let key in person) {
  personKeys.push(key);
}

console.log(personKeys); //expected output: [ 'name', 'age', 'favouriteBook' ]
```
### Property spread operators
{...this.props} spreads out the properties in props as discrete properties (attributes) on the Modal element you're creating. For instance, if this.props contained a: 1 and b: 2, then
```javascript
<Modal {...this.props}>
```
Would be the same as:
```javascript
<Modal a={this.props.a} b={this.props.b}>
```
But it's dynamic, so whatever properties are in props are included.

Spread notation is handy not only for that use case, but for creating a new object with most (or all) of the properties of an existing object — which comes up a lot when you're updating state, since you can't modify state directly:

```js
const obj = {
  foo: {
    a: 1,
    b: 2,
    c: 3
  }
};
// Creates a NEW object and assigns it to `obj.foo`
obj.foo = {...obj.foo, a: "updated"};
console.log("updated", obj.foo);
/* updated {
  "a": "updated",
  "b": 2,
  "c": 3
}
*/
```









