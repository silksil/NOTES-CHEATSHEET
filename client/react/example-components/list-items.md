### Create Action Creator
```js
import axios from 'axios';

export const FETCH_POSTS = 'fetch_posts';

const request = axios.get(`${ROOT_URL}/posts/${API_KEY}`);
export function fetchPosts() {
 const request = axios.get(`${ROOT_URL}/posts/${API_KEY}`);
 return {
  type: FETCH_POSTS,
  payload: request,
 };
}
```
### Create Reducer

### Include reducer in rootReducer
```js
import { combineReducers } from 'redux';

import PostReducer from './reducer_posts';

const rootReducer = combineReducers({
  posts: PostReducer,
});

export default rootReducer;
```

```
import { FETCH_POSTS } from '../actions';

export default function(state = {}, action) { // default you return an object
  switch(action.type) {
  case FETCH_POSTS:
    const arrayToObject = (array, keyField) => // transfrom array, take each property, create object
     array.reduce((obj, item) => {
       obj[item[keyField]] = item
       return obj
     }, {})
    return arrayToObject(action.payload.data, 'id')
  default:
    return state;
  }
}
```
