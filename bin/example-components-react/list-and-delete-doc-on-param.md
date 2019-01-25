The following component fetches and deletes an item based on the id that is included as a paramater in the url. To make it more concrete, the component is created as we are fetching a specific blog post. The blog post is an object and is saved within an mother object (that includes all fetched posts) with the id as it's key. See the paragraph **Current Route as Piece of State** for more info on why data is stored this way by going to the following page: https://github.com/silksil/best-practices-cheatsheets/blob/master/client/react/navigation.md.  

# List Post
### Create Action Creator
```jsx
import axios from 'axios';
export const FETCH_POST = 'fetch_post';


export function fetchPost(id) {
  const request = axios.get(`${ROOT_URL}/posts/${id}`);

  return {
    type: FETCH_POST,
    payload: request,
  };
}
```

### Set-up reducr
Instead of tossing away all the previous posts we have fetched, we want to add all the other posts that have been fetched before -- including posts that have been fetched through other action types (the reducer below also includes the case that is responsible for fetching all posts). In the case of `'FETCH_POST`', the  `...state` refers to all the previous fetched posts. We want to add the most recenty fetched post by assigning the id to it's key. We do this through `[action.payload.data.id]: action.payload.data `.
```jsx
import { FETCH_POST, FETCH_POST} from '../actions';

export default function (state = {}, action) {
  switch (action.type) {
    case FETCH_POST: // this is the action creator this component is referring too
      return { ...state, [action.payload.data.id]: action.payload.data }
      /* Alternatively we could have done:
      const post = action.payload.data;
      const newState = { ... state };
      newState[post.id] = post;
      return newState;
      */ 
    /*
    case FETCH_POSTS: // refers to an action creator related to a different component
    const arrayToObject = (array, keyField) =>
      array.reduce((obj, item) => {
        obj[item[keyField]] = item
        return obj
      }, {})
    return arrayToObject(action.payload.data, 'id')
    */ 
    default:
      return state;
  }
}
```
### Import Action Creator
```jsx
import React, { Component } from 'react';
import { connect } from 'react-redux';
import { fetchPost} from '../actions';
```
```jsx
class PostsShow extends Component {
  render() {
    return (
      <div>
      Post Show
      </div>
    );
  }
}
```
```jsx
export default connect(null, { fetchPost })(PostsShow);
````
### Fetch Post
We want to fetch the post as soon the component has been rendered. We retrieve the id from the paramater. Alternative you could do this.props.post.id. Doing this, you would you would assume that the post is in existence, but the component may render without the post being available. Thus, doing it through the `params` is safer. 
```jsx
componentDidMount() {
 const { id } = this.props.match.params;
 this.props.fetchPost(id);
}
```
### Map State to Props
Below we retrieve the post. `ownProp`s refers to the props of the class component. As we are able to retrieve the id from `props.match.params` we can directly get the props of the specific post, instead of all the props and getting the value in the render function. This way of doing (instead of getting the value in the render function) is especially common in large-scale apps that often have `mapStateToProps` located in seperate files. 
```jsx
function mapStateToProps( { posts }, ownProps) {
 return { post: posts[ownProps.match.params.id] };
}
```
### List Post
First the component is rendered, than we fetch the post. Due to this, we don't have any post in our pros, which makes the properties undefined, causing an error. Thus, we include a loading div if the posts haven't been fetched yet.  
```jsx
render() {
  const { post } = this.props;
  if(!post) {
    return <div> Loading...</div>;
  }
  return (
    <div>
      <h3>{post.title}</h3>
      <h6>Categories: {post.categories}</h6>
      <p>{post.content}</p>
    </div>
  );
}
```
# Delete post 
### Include Button and Link to Action Creator
```jsx
import { fetchPost, deletePost } from '../actions';
```
```jsx
onDeleteClick() {
 const { id } = this.props.match.params;
 this.props.deletePost(id)
}
```
```jsx
<div>
 <button onClick={this.onDeleteClick.bind(this)}> Delete Post </button>
 <h3>{post.title}</h3>
 <h6>Categories: {post.categories}</h6>
 <p>{post.content}</p>
</div>
```
```jsx
export default connect(mapStateToProps, { fetchPost, deletePost })(PostsShow);
```
### Create Action Creator
In this case we don't need to have access to the post anymore, we only need to remove it from the application level state. Thus, we only return an id. 
```jsx
export const FETCH_POST = 'fetch_post';

export function deletePost(id, callback) {
  const request = axios.delete(`${ROOT_URL}/posts/${id}`)

  return {
    type: DELETE_POST,
    payload: id,
  };
}
```
### Include Navigation Callback
If we want to navigate the user after he has clicked and the request has been completed,  we can include a callback.
```jsx
export function deletePost(id, callback) {
  const request = axios.delete(`${ROOT_URL}/posts/${id}${API_KEY}`)
    .then(() => callback());

  return {
    type: DELETE_POST,
    payload: id,
  };
}
```
```jsx
onDeleteClick() {
 const { id } = this.props.match.params;
 this.props.deletePost(id, () => {
  this.props.history.push('/');
 });
}
```
### Delete it from local memory
We use the lodash library to delete the post.
```jsx
import _ from 'lodash';
import { FETCH_POSTS, FETCH_POST, DELETE_POST } from '../actions';

export default function (state = {}, action) {
  switch (action.type) {
    case DELETE_POST:
      return _.omit(state, action.payload) 
      // it looks to the state object > if it is has the key of the post id > delete it
    /*
    case FETCH_POST:
      return { ...state, [action.payload.data.id]: action.payload.data };
    case FETCH_POSTS:
      const arrayToObject = (array, keyField) =>
        array.reduce((obj, item) => {
          obj[item[keyField]] = item
          return obj
        }, {})
      return arrayToObject(action.payload.data, 'id')
     */
    default:
      return state;
  }
}
```


