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
Below we wire up the Redux Form. The form property allows us to include a unique string, which allows to include multiple forms on a screen, e.g. login and sign-up form. Make sure the value assigned to the key `form` is unique (throughout the project).
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
    label="Title"
    name="title"
    component={this.renderField}
   />
   <Field
    label="Categories"
    name="categories"
    component={this.renderField}
   />
   <Field
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
renderField(field) {
 return (
  <div>
   <label>{field.label}</label>
   <input
    type="text"
    {...field.input}
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
There are three different states of a input in a form: pristine(not filled something into the input field), touched (enter text and focus away) and invalid. Now the errors will show up directly, but in this case we want to only show the errors if the user enters the touched state.
```jsx
renderField(field) {
 return (
  <div>
   <label>{field.label}</label>
   <input
    type="text"
    {...field.input}
   />
   {field.meta.touched ? field.meta.error : ''}
  </div>
  */show error only if user touched field, else empty string*/
 );
}
```
### Submitting the form
Redux Form handles the state of the form, but it doesn't communicate it to a server, we still have to do this manually. When we wire up the Redux Form helper, it passes a ton of additional properties. One of them is the `handleSubmit`. handleSubmit checks whether the form is valid. If it is valid, it is passed to our assigned function `onSubmit` (as said before it is not the responsiblity of Redux Form as soon it is validated / submitted). We call `.bind(this)`, because we passing onSubmit as a callback function that will operate in a different context. Thus, with the `.bind(this)` we refer to the component.
```jsx
render() {
 const { handleSubmit } = this.props; // here we pull of handleSubmit from this.props
 return (
  <form onSubmit={handleSubmit(this.onSubmit.bind(this))}> // we call .bind(this) to refer to the component
   <Field
    label="Title"
    name="title"
    component={this.renderField}
   />
   <Field
    label="Categories"
    name="categories"
    component={this.renderField}
   />
   <Field
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




Sources:
- https://redux-form.com/7.4.2/
