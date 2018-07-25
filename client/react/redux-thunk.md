## Configuration
`npm install --save redux-thunk`

```jsx
import ReduxThunk from 'redux-thunk'

const store = createStore(reducers, {}, applyMiddleware(ReduxThunk));
````

## Example
Instead of return an action, it produces a action and passes it to the dispatch function and sends it to all action creators, causing them to instantly recalculate the app state. So rather returning it, you can dispatch it anyware throughout the action creator.

```
import axios from 'axios';
import { FETCH_USER } from './types';

export const fetchUser = () => async dispatch => {  // whenever fetchUser is called it will instantly return a function
    const res = await axios.get('/api/current_user'); // if ReduxThunk sees we return a function, instead of a normal function, it will automaticall call this function and pass the dispatch function as an argument

    dispatch({ type: FETCH_USER, payload: res.data });
};
```z
