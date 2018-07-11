# Middleware 
Middleware intercepts all actions and can let actions:
- Pass.
- Manipulate.
- Log.
- Stop.

## Redux-Promise
https://www.npmjs.com/package/redux-promise </br>
The main purpose of this library is to clean up the code. It:
- Sees the incoming action and ooks to the payload property.
- If it is an promise it  stops the action.
- After the promise is resolves, creates a new action send it to the reducers.
- In other words: it unwraps the promise.

### Configuration 
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { createStore, applyMiddleware } from 'redux';
import ReduxPromise from 'redux-promise';

import App from './components/app';
import reducers from './reducers';

const createStoreWithMiddleware = applyMiddleware(ReduxPromise)(createStore);

ReactDOM.render(
 <Provider store={createStoreWithMiddleware(reducers)}>
  <App />
 </Provider>
 , document.querySelector('.container'));
```
#### Example
One way the middleware is helpfull is when we use axios to fetch some data. Axios returns a promise, and instead of unwrapping the promise we can include the promise in the payload. 
```jsx
export function fetchWeather(city) {

  const url = `${ROOT_URL}&q=${city},us`;
  const request = axios.get(url);

  return {
    type: FETCH_WEATHER,
    payload: request
  };
}
```
The promise will be unwrapped by the middleware and can be accessed in one of the reducers. 
```jsx
export default function(state = [], action) {
  switch(action.type) {
  case FETCH_WEATHER:
    return [ action.payload.data, ...state ];
  }
  return state;
}
```
