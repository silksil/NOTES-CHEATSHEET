## Configuration
`npm install --save redux-thunk`

```jsx
import ReduxThunk from 'redux-thunk'

const store = createStore(reducers, {}, applyMiddleware(ReduxThunk));
````

##

Instead of return an action, it produces a action and passes it to the dispatch function and sends it to all action creators, causing them to instantly recalculate the app state. So rather returning it, you can dispatch it anyware throughout the action creator.
ReduxThunk allows you
