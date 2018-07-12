The following component fetches and deletes an item based on the id that is included as a paramater in the url. To make it more concrete, the component is created as we are fetching a specific blog post. The blog post is an object and is saved within an mother object (that includes all fetched posts) with the id as it's key. See the paragraph **Current Route as Piece of State** for more info on why data is stored this way by going to the following page: https://github.com/silksil/best-practices-cheatsheets/blob/master/client/react/navigation.md.  

### Create Action
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

### Create Action Creator
Instead of tossing away all the previous posts we have fetched, we want to add all the other posts that have been fetched before -- including posts that have been fetched through other action types (the reducer below also includes the case that is responsible for fetching all posts). In the case of 'FETCH_POST', the  `...state` refers to all the previous fetched posts. We want to add the most recenty fetched post by assigning the id to it's key. We do this through `[action.payload.data.id]: action.payload.data `.
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
    case FETCH_POSTS: // refers to an action creator related to a different component
    const arrayToObject = (array, keyField) =>
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
