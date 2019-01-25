To check whether data that is inluded in the state of Redux Store is being displayed we have to pass some initial state to the `Root.js` file.

As a reminder, the `createStore` property of redux allows you to create a new store. The first argument includes the reducers and the second argument includes some initial state. To invoke this we add `initialState` as an second argument. If an initial state is not being provided, it will be an empty object. 
```js
import React from 'react';
import { Provider } from 'react-redux';
import { createStore } from 'redux';
import reducers from './reducers';

export default ({ children, initialState = {} }) => {
  return (
    <Provider store={createStore(reducers, initialState)}>
      {children}
    </Provider>
  )
}
```
In the test we can that pass the initial state to the Redux store.
```js
import React from 'react';
import { mount } from 'enzyme';
import CommentList from '../CommentList';
import Root from '../../Root';

let wrapped;

beforeEach(() => {
  const initialState = {
    comments: ['Comment 1', 'Comment 2']
  };

  wrapped = mount (
    <Root initialState={initialState}>
      <CommentList />
    </Root>
  );
});
```
We can then test if the initial state is succesfully rendered to the screen. 
```js
it('it shows the text for each comment', () => {
  expect(wrapped.render().text()).toContain('Comment 1');
  expect(wrapped.render().text()).toContain('Comment 2')
});
```
