The following component fetches and deletes an item based on the id that is included as a paramater in the url. To make it more concrete, the component is created as we are fetching a specific blog post. The blog post is an object and is saved within an mother object (that includes all fetched posts) with the id as it's key. See the paragraph **Current Route as Piecce of State** for more info on https://github.com/silksil/best-practices-cheatsheets/blob/master/client/react/navigation.md.  

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
```jsx
import {FETCH_POST} from '../actions';

export default function (state = {}, action) {
  switch (action.type) {
    case FETCH_POST:
      return { ...state, [action.payload.data.id]: action.payload.data }
    default:
      return state;
  }
}
```
