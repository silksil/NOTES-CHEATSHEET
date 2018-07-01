## A controlled field component
The value of our input is set by our state, not the other way around. You can this by stating the value to the state, and by refering on the onChange attribute to a function that changes the state.

#### The form
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
If we pass of the event handler **through a callback**, the value of `this` is not referring to the SearchBar component, causing an error. In order to solve this, we need to bind the event handler to the right context: 
```jsx
constructor(props) {
super(props);

 this.state = { term: '' }

 this.onFormSubmit = this.onFormSubmit.bind(this);
  }
```






