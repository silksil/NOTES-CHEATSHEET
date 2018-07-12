This component includes an input field in which a user can search. If the user clicks enter an ajax request will be fired that fetches data and update the state.

## Create Controlled Component
You create a **controlled component**: the value of our input is set by our state, not the other way around. To get the state you can do this by relating the value to the state, and to update it you can refer trough the onChange attribute to a function that changes the state.
```jsx
constructor(props) {
 super(props);

 this.state = { term: '' }

}
```
```jsx
render() {
 return (
  <form className="input-group">
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
## Prevent Page Reload
We created a seachbar out of a form element - it has specific handlers build in (e.g. onEnter onSubmit) that `input` doesn't have. If you click or hit enter, the browser automatically thinks you want to submit a form, causing the page to be reloaded. For a single page application, this is not what you want. Thus we refer to a function that handles the submittal.
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
```jsx
onFormSubmit(event) {
 event.preventDefault();
}
```
## Set-Up Action Creator
Before submitting we set up the action creator that fetches the data.
```javascript
export function fetchWeather(city) {

  const url = `${ROOT_URL}&q=${city},us`;
  const request = axios.get(url);

  return {
    type: FETCH_WEATHER,
    payload: request
  };
}
```

## Connect Component to Action
```jsx
import { connect } from 'react-redux';
import { fetchWeather } from '../actions/index';
```
The first argument is `null`, because we do not include `mapStateToProps`. `mapDispatchToProps` always comes as the second argument.
```jsx
export default connect(null, { fetchWeather })(SearchBar);
```
If we an user clicks on submit or hits enter, it will fetch the data and then we will clear the search input. As we are working with a callback in the form, we also have to assure we set the right context.
```jsx
onFormSubmit(event) {
 event.preventDefault();
 this.props.fetchWeather(this.state.term);
 this.setState({ term: '' }) // clear the search input
}
```
```jsx
constructor(props) {
 super(props);

 this.state = { term: '' }

 this.onInputChange = this.onInputChange.bind(this);
 this.onFormSubmit = this.onFormSubmit.bind(this); // set the right context
}
```
Now in the terminal(Network > XHR) the request and results should display.
## Set-Up The Reducer
The last step includes setting up the reducer(in this case Redux-Promise is used, so no need to unwrap). In this example, we add every search item to the list.
```jsx
import { FETCH_WEATHER } from '../actions/index';

export default function(state = [], action) {
  switch(action.type) {
  case FETCH_WEATHER:
    return [ action.payload.data, ...state ]; // alternatively: return state.concat([action.payload.data])
    // never push() the data into the state > we don't manipulate, but return a new instance of state
  }
  return state;
}
```
Alternatively we could return only the latests searched item: `return action.payload.data`.


