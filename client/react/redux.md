# General
Whereas React displays the views, Redux collects all the data of the application. 
- With Redux we centralize all the data in a single object - we refer to this as 'state'. 
- The state can change through events, e.g. a user clicks on something, or new data coming in from a server. 
- If the state changes, the containers will instantly re-render.

# Containers
A container is a component that has direct access to the Redux storem, it:
1. Receives state updates.
2. Dispatches actions.

- To make the connection between react-redux you hav to `import { connect } from 'react-redux'`.
- The function `mapStateToProps(state){}` takes in the application state as an argument, and object is returned that can be extracted as props in the container. 
- The function `function mapDispatchToProps(dispatch) {}`.
- To produce the container and glue React with Redux we have to connect the function(s) with the component, e.g. `export default connect(mapStateToProps, mapDispatchToProps)(BookList);`. 

# Reducer
Reducers returns the new state.

## childrenReducer
Is a function that reduces a piece of the application state - the data is returned through a function. Because an application can have many different pieces of state, it can have many different reducers. 

- Takes in two arguments: state and action.
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
import ActiveBook from './reducer_active_book';

const rootReducer = combineReducers({
  books: BooksReducer, 
  activeBook: ActiveBookReducer
});

export default rootReducer;
```

## Reducer
The only way to change the state is by sending a signal to the state: an *action* initiates this signal and a *reducer* returns the new state. 

The reducer can return:
1. A 'type' property - a value that describe how / what state should change.
2. A 'payload' (optional) - includes specific data.

#### Sources
- https://www.valentinog.com/blog/react-redux-tutorial-beginners/#React_Redux_tutorial_getting_to_know_Redux_actions


