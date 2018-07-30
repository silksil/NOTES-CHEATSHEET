## Introduction
In short, Redux Form does the following: 
1. Identifies different pieces of form state.
2. Makes one 'Field' component per piece of state (created by Redux Form).
3. If the user changes input field, Redux Form automatically handles changes (This means no set.state call, no action creator, no need to set the value).
4. If the user submits a form, it validates input and handles form submittal.

## Configuration
```npm install --save redux-form```

## Reducer
We have to import a reducer from the Redux Form library and hook it up to the combineReducer call. Due to this we don't have to include manual created action creators. We assign a different name to reducer because formReducer is more specific. 
```js
import { combineReducers } from 'redux';
import { reducer as formReducer } from 'redux-form'; // rename to formReducer
```
formReducer has to be assigned to form. 
```js
const rootReducer = combineReducers({
  form: formReducer, // make sure to assign it to form 
});

export default rootReducer;
```

### Import
Field is a react-form component that helps wire up event handlers. ReduxForm is a function similar to the connect helper; it allows our component to directly talk to the formReducer in the Redux Store.
```jsx
import React, { Component } from 'React';
import { Field, reduxForm } from 'redux-form';
```

### Export
Below we wire up the Redux Form. The form property allows us to include a unique string, which allows to include multiple forms on a screen, e.g. login and sign-up form. Make sure that the key is `form` and that the assigned value is unique (throughout the project).
```jsx
export default reduxForm({ // wire up Redux Form
 form: 'PostsNewForm' // have unique value assigned to the key form
 })(PostNew);
```

### Form Component
### Single field
The field component represents a distinct input. The name property refers to the piece of state it produces. The field component doesn't know how to show itself on the screen. Therefore we have to include a component in 'component' to show something on the screen.
```jsx
render() {
 const { handleSubmit } = this.props;
 return (
  <form onSubmit={handleSubmit(this.onSubmit.bind(this))}>
   <Field
    name="title"
    component={this.renderTitleField}
   />
   <button type="submit"> Submit </button>
  </form>
 );
}

```
To include the component we refer to `this.renderTitleField`  and we include in this function the argument `field`. This field object includes event handlers that we need to wire up. In this case, the renderTitleField is being linked to the component through the field argument. `Field.input` is an object with a bunch of different event handlers (e.g. onFocus, onChange, onBlur) and props (e.g. value input). Through `{...field.input}` we say that the different properties of that object should be communicated as props to the input tag.
```jsx
renderTitleField(field) {
 return (
  <div>
   <label>Title</label>
   <input
    type="text"
    {...field.input}
   />
  </div>
 );
}
```
  
### Multiple fields
We can pass arbiturary arguments (with any given name, doesn't have to be label) we can access it through the field property.
```jsx
render() {
 const { handleSubmit } = this.props;
 return (
  <form onSubmit={handleSubmit(this.onSubmit.bind(this))}>
   <Field
    type-"text"
    label="Title"
    name="title"
    component={this.renderField}
   />
   <Field
    type="text"
    label="Categories"
    name="categories"
    component={this.renderField}
   />
   <Field
    type="text"
    label="Post Content"
    name="content"
    component={this.renderField}
   />
   <button type="submit"> Submit </button>
  </form>
 );
}
```
```jsx
renderField({ input, label }) {
 return (
  <div>
   <label>{label}</label>
   <input
    type="text"
    {...input}
   />
  </div>
 );
}
```
### Validation
We use the Redux Form validate function. Firstly, hook up the functionality:
```jsx
export default reduxForm({
  validate, // include the validate functionality of Redux Form
  form: 'PostsNewForm',
})(PostNew);
```
To include validation we should create a function of Redux Form, called `validate`. It will be called automatically throughout the form lifecycle; e.g. when submitting or when a user leaves an input field. We pass the argument `values`. This contains an object with all properties of the form, e.g. `{ title: 'asdf', categories: 'asdf' etc.}` We include an error variable that contains an error, and return this object. If error is empty, the form is fine to submit. If not, Redux Form assumes the form is invalid. To validate if there are errors, we include if-statements.
```jsx
function validate(values) {
 const errors = {};

 if (!values.title || values.title.length < 3) {
  errors.title = 'Enter a title that is at least 3 characters!';
 }
 if (!values.categories) {
  errors.categories = 'Enter some categories';
 }
 if (!values.content) {
  errors.content = 'Enter some content please';
 }
 return errors; // if errors is empty, the Redux Form is ok with submitting
}
```
The property `metta.error` is automatically added to the `field argument` in a `field component` through our `validate` function. If the error object has a property that is equal to the name of a `field element` (in the form), it will automaticaly link the error to `metta.error` with a string containing the error.
```jsx
renderField(field) {
 return (
  <div>
   <label>{field.label}</label>
   <input
    type="text"
    {...field.input}
   />
   {field.meta.error}
  </div>
 );
}
```
There are three different states of a input in a form: pristine(not filled something into the input field), touched (enter text and focus away) and invalid. Now the errors will show up directly, but in this case we want to only show the errors if the user enters the touched state. `{field.meta.touched ? field.meta.error : ''}` states: show error only if user touched field, else empty string.
```jsx
renderField(field) {
 return (
  <div>
   <label>{field.label}</label>
   <input
    type="text"
    {...field.input}
   />
   <div className="text-help>
    {field.meta.touched ? field.meta.error : ''}
   </div>
  </div>
 );
}
```
To apply css when there is an error, we can conditionall include a className.
```jsx
renderField(field) {
 const className = `form-group ${field.meta.touched && field.meta.error ? 'has-danger' : ''}`; 
 // form-group is always assigned. 
 // has-danger is the className to make it red
 return (
  <div className={className}>
   <label>{field.label}</label>
   <input
    type="text"
    {...field.input}
   />
   <div className="text-help>
    {field.meta.touched ? field.meta.error : ''}
   </div>
  </div>
 );
}
```
We can use deconstructuring to pull of `touched` and `error` of the meta property to make the code a bit more tidy. 
```jsx
renderField(field) {
 const { meta: { touched, error } } = field;
 const className = `form-group ${touched && error ? 'has-danger' : ''}`; 
 return (
  <div className={className}>
   <label>{field.label}</label>
   <input
    type="text"
    {...field.input}
   />
   <div className="text-help>
    {touched ? error : ''}
   </div>
  </div>
 );
}
```

### Submitting the form
Redux Form handles the state of the form, but it doesn't communicate it to a server, we still have to do this manually. When we wire up the Redux Form helper, it passes a ton of additional properties. One of them is the `handleSubmit`. handleSubmit checks whether the form is valid. If it is valid, it is passed to our assigned function `onSubmit` (as said before it is not the responsiblity of Redux Form as soon it is validated / submitted). We call `.bind(this)`, because we passing onSubmit as a callback function that will operate in a different context. Thus, with the `.bind(this)` we refer to the component.
```jsx
render() {
 const { handleSubmit } = this.props; // here we pull of handleSubmit from this.props
 return (
  <form onSubmit={handleSubmit(this.onSubmit.bind(this))}>
   <Field
    type="text"
    label="Title"
    name="title"
    component={this.renderField}
   />
   <Field
    type="text"
    label="Categories"
    name="categories"
    component={this.renderField}
   />
   <Field
    type="text"
    label="Post Content"
    name="content"
    component={this.renderField}
   />
   <button type="submit"> Submit </button>
  </form>
 );
}
```
If the form is validated, it will end up at `onSubmit`. It will be called with an object including the values of the form that by convention is called `values`.
```jsx
onSubmit(values) {
  console.log(values)
}
```
Whenever we have to save data or make a API request in Redux, we have to call an action creator.
```jsx
import { connect } from 'react-redux';
import { createPost } from '../actions/index.js'; // the action creator
```
```jsx
onSubmit(values) {
 this.props.createPost(values, () => {
 });
}
```
```jsx
export default reduxForm({
  validate,
  form: 'PostsNewForm',
})(
  connect(null, { createPost })(PostNew)
);
```
### Navigation
The code below shows the function that sends the data to an API. In this case, it includes a callback, because after the post we navigate to a page to shows the post, including that specific post. Thus, through this we assure that it is included in the back-end, and only then the screen can be loaded that includes the specific post.
```jsx
export function createPost(values, callback) {
  const request = axios.post(`${ROOT_URL}/posts/${API_KEY}`, values)
    .then(() => callback());

  return {
    type: CREATE_POSTS,
    payload: request,
  };
}
```
After the callback is being called, we can navigate to the next page through `history.push`.
```jsx
onSubmit(values) {
 this.props.createPost(values, () => {
  this.props.history.push('/')
 });
}
```
#### Sources:
- https://redux-form.com/7.4.2/
