Create Action Creator
```js
const request = axios.get(`${ROOT_URL}/posts/${API_KEY}`);
return {
  type: FETCH_POSTS,
  payload: request,
};
}

```
