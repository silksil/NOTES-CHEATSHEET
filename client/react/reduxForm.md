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

### Component

### Validation

Sources:
- https://redux-form.com/7.4.2/
