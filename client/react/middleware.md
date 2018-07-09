# Middleware 
Middleware intercepts all actions and can let actions:
- Pass.
- Manipulate.
- Log.
- Stop.

## Redux-Promise
https://www.npmjs.com/package/redux-promise
The main purpose of this library is to clean up the code. It:
- Sees the incoming action and ooks to the payload property.
- If it is an promise it  stops the action.
- After the promise is resolves, creates a new action send it to the reducers.
- In other words: it unwraps the promise.

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
