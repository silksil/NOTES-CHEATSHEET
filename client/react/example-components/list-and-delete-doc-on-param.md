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
Instead of tossing all the previous posts we have fetched, we want to add to the all the other posts that have been fetched before
```jsx
import {FETCH_POST} from '../actions';

export default function (state = {}, action) {
  switch (action.type) {
    case FETCH_POST: // this is the action creator this component is referring too
      return { ...state, [action.payload.data.id]: action.payload.data }
    case FETCH_POSTS:
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
