#### A controlled field component
The value of our input is set by our state, not the other way around. To get the state you can do this by relating the value to the state, and to update it you can refer the onChange attribute to a function that changes the state.
```jsx
<form onSubmit={this.onFormSubmit} className="input-group">
  <input
    placeholder="type here"
    className="form-control"
    value={this.state.term}
    onChange={this.onInputChange}
  />
</form>
```

#### The change handler
All event handlers, e.g. onChange, come along with the event object. To get the typed input, we can refer to even.target.value.
```jsx
onInputChange(event) {
 this.setState({ term: event.target.value });
}
```

#### Binding
If we pass of the event handler **through a callback**, the value of `this` is not referring to the component, causing an error. In order to solve this, we need to bind the event handler to the right context: 
```jsx
constructor(props) {
 super(props);

 this.state = { term: '' }

 this.onInputChange = this.onInputChange.bind(this);
}
```
The statement above states: the existing function = take existing function and bind it to this. 

#### More on binding
The `bind` method is avaiable on every function (although it has no effect on fat arrow functions). To set the value of `this` you call the `bind` method with a single parameter, passing the value to be assigned to `this`. The `bind` method returns a new function for which the `this` value is fixed to the value specified in the `bind` parameter, as shown below.
```js
const myObj = {
  myData: 'Hello world!'
};

function printMyData() {
  console.log(this.myData);
}

const newFunction = printMyData.bind(myObj);
newFunction(); // --> Hello world!
```
Using the `printMyData` function as a basis, the `bind` method fixes the `this` value to `myObj` and returns a new function assigned here to the variable `newFunction`. When we call `newFunction` the `this` value will be `myObj`, and therefore we can console.log `myData` through the `this` keyword.



Sources:
- https://github.com/HackYourFuture/fundamentals/blob/master/fundamentals/this.md]
- https://medium.freecodecamp.org/react-binding-patterns-5-approaches-for-handling-this-92c651b5af56
- https://reactjs.org/docs/handling-events.html




