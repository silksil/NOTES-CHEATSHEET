# General
Whereas React displays the views, Redux collects all the data of the application. With Redux we centralize all the data in a single object - we refer to this as 'state'. The state can change through events, e.g. a user clicks on somehting, or new data coming in from a server.

# Containers
A container is a component that has direct access to the Redux store. It...
- Receives state updates.
- Dispatches actions

# Reducer
Reducers manage the state.

## childrenReducer
Is a function that reduces a piece of the application state - the data is returned through a function. Because an application can have many different pieces of state, it can have many different reducers. 

- Takes in two arguments: state and action
- Has a switch statement to check the action type. The type property value is used to calculate the next state.
- Calculates the next state depending on the action type. When no action type matches, it should return at least an initial state.
- Assure that the state is updated in an immutable way => copy curren state plus new data:
  - Array's: concat(), slice(), ...spread
  - Object's: object.assign() and ..spread

## rootReducer
The rootReducer/combineReducer extract what is returned from every reducer in a single object => the state. The example below shows how the BooksReducer and ActiveBookReducer are assigned to the different keys/pieces of state.  

```jsx
import { combineReducers } from 'redux';
import BooksReducer from './reducer_books';
import ActiveBook from './reducer_active_book'

const rootReducer = combineReducers({
  books: BooksReducer, 
  activeBook: ActiveBookReducer
});

export default rootReducer;
```



