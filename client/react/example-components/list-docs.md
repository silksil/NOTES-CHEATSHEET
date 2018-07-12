This component displays a fetching and displaying a list of blog posts. Every item includes a link to a specific blog post.

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
React always render the component as soon as possible, thus loading beforehand doesn't make a change. Therefore, use componentDidMount. 
```jsx
class PostIndex extends Component {
componentDidMount() {
    this.props.fetchPosts();
  }
  
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
### Test Network Request
If set-up correclty the console in network tab > XHR request will show the request if we reload the page. 

### Link it to the application state
```jsx
function mapStateToProps(state) {
  return { posts: state.posts };
}
```
```jsx
export default connect(mapStateToProps, { fetchPosts })(PostIndex);
```
### Log Props
To check whether we have received our props we can console.log within render(). It should log twice: once when the page is loaded first, secondly after data is being fetched and returned to the state. 

### Map items
Lastly we map-out out all items. In this case, we use lodash's library to map (as we map over an object). Additionaly, an link to every indvidual item is included. For more info see https://github.com/silksil/best-practices-cheatsheets/blob/master/client/react/navigation.md

```jsx
import { Link } from 'react-router-dom';
import _ from 'lodash';
```

```jsx
class PostIndex extends Component {
  componentDidMount() {
    this.props.fetchPosts();
  }

  renderPosts() {
    return _.map(this.props.posts, post => {
      return (
        <li className="list-group-item" key={post.id}>
          <Link to={`/posts/${post.id}`}>
            {post.title}
          </Link>
        </li>
      );
    });
  }

  render() {
    return (
      <div>
        <h3> Posts </h3>
        <ul className="list-group">
          {this.renderPosts()}
        </ul>
      </div>
    );
  }
}
```

