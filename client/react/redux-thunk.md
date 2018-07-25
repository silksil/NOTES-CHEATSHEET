## Configuration
`npm install --save redux-thunk`

```jsx
import ReduxThunk from 'redux-thunk'

const store = createStore(reducers, {}, applyMiddleware(ReduxThunk));
````

## Example
Instead of return an action, it produces a action and passes it to the dispatch function and sends it to all action creators, causing them to instantly recalculate the app state. So rather returning it, you can dispatch it anyware throughout the action creator.

```js
export const fetchUser = () => async dispatch => {  // whenever fetchUser is called it will instantly return a function
    const res = await axios.get('/api/current_user'); // if ReduxThunk sees we return a function, instead of a normal function, it will automaticall call this function and pass the dispatch function as an argument

    dispatch({ type: FETCH_USER, payload: res.data });
};
```


## Dispatch and 
Well, this is exactly what redux-thunk does: it is a middleware that looks at every action that passes through the system, and if it’s a function, it calls that function. That’s all it does.

The only thing I left out of that little code snippet is that Redux will pass two arguments to thunk functions: dispatch, so that they can dispatch new actions if they need to; and getState, so they can access the current state. So you can do things like this:
```
export const logOutUser() => async (dispatch, getState) => {
    return axios.post('/logout').then(function() {
      // pretend we declared an action creator
      // called 'userLoggedOut', and now we can dispatch it
      dispatch(userLoggedOut());
    });
}
```
