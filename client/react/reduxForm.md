In short, Redux Form does the following: 
1. Identifies different pieces of form state.
2. Makes one field component per piece of state.
3. If the user changes input field, Redux Form automatically handles changes >> no set.state call, no action creator, don't have to set value etc.
4. If the user submits a form, it validates input and handles form submittal.

## Configuration
```npm install --save redux-form```

## Reducer
We have to import a reducer from the Redux Form library and hook it up to the combineReducer call.  We assign a different name to reducer because formReducer is more specific. 
```js
import { combineReducers } from 'redux';
import { reducer as formReducer } from 'redux-form';
```

```js
const rootReducer = combineReducers({
  posts: PostReducer,
  form: formReducer //make sure to assign it to form 
});

export default rootReducer;
```


Sources:
- https://redux-form.com/7.4.2/
