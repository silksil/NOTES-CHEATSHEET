## Configuration
`npm install --save redux-thunk`

```jsx
import ReduxThunk from 'redux-thunk'

const store = createStore(reducers, {}, applyMiddleware(ReduxThunk));
````


## Dispatch and getState
If you wanted an action to do something, that code would need to live inside a function. Something like this:
```js
function getUser() {
  return function() {
    return axios.get('/current_user');
  };
}
```

Redux-thunk is middleware that looks at every action that passes through the system, and if it’s a function, it calls that function. That’s all it does.

The only thing I left out of that little code snippet is that Redux will pass two arguments to thunk functions: dispatch, so that they can dispatch new actions if they need to; and getState, so they can access the current state. So you can do things like this:
```js
export const logOutUser() => async (dispatch, getState) => {
    return axios.post('/logout').then(function() {
      // pretend we declared an action creator
      // called 'userLoggedOut', and now we can dispatch it
      dispatch(userLoggedOut());
    });
}
```
