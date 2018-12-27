## Configuration
`npm install --save redux-thunk`

```jsx
import ReduxThunk from 'redux-thunk'

const store = createStore(reducers, {}, applyMiddleware(ReduxThunk));
````
## Redux-thunk
- It allows you to return as many actions as you want from a single action creator (instead of Redux-promise, that allow you return only one promise one single action creator). 
- You can do it any time as you wish. 

## Dispatch and getState
Redux will pass two arguments to thunk functions: dispatch, so that they can dispatch new actions if they need to; and getState, so they can access the current state. 

In order to dispatch a new actionm, you return a new function with the dispatch argument.
```js
export const signup = ({ email, password }) => {
  return (dispatch) => {

  }
};
```
This can be rewritten to:
```
export const signup = ({ email, password }) => dispatch => {
};
```
