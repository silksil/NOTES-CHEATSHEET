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
In this example the data we get is an array and we transform it in an object with objects, to make it easier to access a specific item. See paragraph `Current route as piece of state` for more info https://github.com/silksil/best-practices-cheatsheets/blob/master/client/react/navigation.md
```jsx
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
### Include Reducer in rootReducer
```js
import { combineReducers } from 'redux';

import PostReducer from './reducer_posts';

const rootReducer = combineReducers({
  posts: PostReducer,
});

export default rootReducer;
```

### Hook-up Action Creator
```jsx
import React, { Component } from 'react';
import { connect } from 'react-redux';;

import { fetchPosts } from '../actions';
```
```jsx
class PostIndex extends Component {
 render() {
  return (
   <div>
   </div>
  );
 }
}
```
```jsx
export default connect(null, { fetchPosts })(PostIndex);
```

