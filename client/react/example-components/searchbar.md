You create a `controlled component`: the value of our input is set by our state, not the other way around. To get the state you can do this by relating the value to the state, and to update it you can refer trough the onChange attribute to a function that changes the state.
```jsx
constructor(props) {
 super(props);

 this.state = { term: '' }

}
```
```jsx
render() {
 return (
  <form onSubmit={this.onFormSubmit} className="input-group">
   <input
    placeholder="Get a five-day forecast in your favourite cities"
    className="form-control"
    value={this.state.term} // making it a controlled component
    onChange={this.onInputChange}
   />
    <span className="input-group-btn">
      <button type="submit" className="btn btn-secondary"> Submit </button>
    </span>
  </form>
 )
}
```
All event handlers, e.g. `onChange`, come along with the event object. To get the typed input, we can refer to even.target.value. If we now type, it would say `'cannot read property setState of undefined'`.
```jsx
onInputChange(event) {
 this.setState({ term: event.target.value });
}
``` 
If we pass of the event handler **through a callback**, the value of `this` is not referring to the component. In order to solve this, we need to bind the event handler to the right context: 
```jsx
constructor(props) {
 super(props);

 this.state = { term: '' }

 this.onInputChange = this.onInputChange.bind(this); // here we set the right context
}
```
