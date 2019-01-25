To test reducers you call the reducer, pass a fake action and make an expectation of what it returns.

### The reducer file
```js
import { SAVE_COMMENT } from '../actions/types';

export default function(state = [], action) {
  switch (action.type) {
    case SAVE_COMMENT:
      return [...state, action.payload];
    default:
      return state;
  }
}
```
### The test
```js
import commentsReducer from '../comments';
import { SAVE_COMMENT } from '../../actions/types';

it('handles actions of type SAVE_COMMENT', () => {
  const action = {
    type: SAVE_COMMENT,
    payload: 'New Comment'
  };

  const newState = commentsReducer([], action);
  expect(newState).toEqual(['New Comment']);
})
```
