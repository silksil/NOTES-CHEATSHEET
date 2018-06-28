# General
Whereas React displays the views, Redux collects all the data of the application. 
- With Redux we centralize all the data in a single object - we refer to this as 'state'. 
- The state can change through events, e.g. a user clicks on something, or new data coming in from a server. 
- If the state changes, the containers will instantly re-render.

# Containers
A container is a component that has direct access to the Redux store, it:
1. Receives state updates.
2. Dispatches actions.

- To make the connection between react-redux you hav to `import { connect } from 'react-redux'`.
- The function `mapStateToProps(state){}` takes in the application state as an argument, and object is returned that can be extracted as props in the container. 
- The function `function mapDispatchToProps(dispatch) {}` is a method that retuns what should be included in the container's props. See also the section below about actions. 
- To produce the container and glue React with Redux we have to connect the function(s) with the component, e.g. `export default connect(mapStateToProps, mapDispatchToProps)(BookList);`. 

```jsx
import React, { Component } from 'react';
import { connect } from 'react-redux';
import { selectBook } from '../actions/index';
import { bindActionCreators } from 'redux';

class BookList extends Component {
  renderList() {
    return this.props.books.map((book) => {
      return (
        <li
          key={book.title}
          onClick={() => this.props.selectBook(book)}
          // we call the action creator everytime we click
          className="list-group-item">
          {book.title}
        </li>
      );
    });
  }
  render() {
    return (
      <ul className="list-group col-sm-4">
        {this.renderList()}
      </ul>
    );
  }
}

//anything returned from this function will end up as props on the booklist container.
function mapDispatchToProps(dispatch) {
  return bindActionCreators({ selectBook }, dispatch);
  // dispatch receives actions results and spits it out to all reducers everytime selectBook is called.

}

// whenever our application state changes, it will re-render.
function mapStateToProps(state) {
  return {
    books: state.books
  };
}

// connect takes a function(s) and component and produces a container.
export default connect(mapStateToProps, mapDispatchToProps)(BookList);
```

# Action 
- Actions creaters are functions that return a specific action. Examples of action creators: 
  - Direct: clicking on a button, hovering, add something to the basket. 
  - Indirect: ajax or webpage finish loading. 
- To bind an action to an component we should import action the action, e.g. `import { selectBook } from '../actions/index';`
- Then we import a function to make sure that the action created from the action creator, flows through all reducers: `import { bindActionCreators } from 'redux';`
- To bind an action happening to all reducers we can use `function mapDispatchToProps(dispatch}`:
```jsx 
// Anything returned from this function will end up as props on the container
function mapDispatchToProps(dispatch) {
  return bindActionCreators({ selectBook: selectBook }, dispatch); 
  //dispatch receives actions and spits it out to all reducers
}
```
- Action creator returns an object (which is called the action) that describes the action. It can return:
  - A 'type' property - a value that describe how / what state should change.
  - A 'payload' (optional) - includes specific data.
  
```jsx
export function selectBook(book) {
 return {
  type: 'BOOK_SELECTED',
  payload: book
 };
}
```

# Reducer
Reducers returns the new state.

## childrenReducer
Is a function that returns a piece of the application state.  Because an application can have many different pieces of state, it can have many different reducers. 

- Takes in two arguments: state and action.
- Has a switch statement to check the action type. The type property value is used to calculate the next state. When no action type matches, it should return at least an initial state.
- Assure that the state is updated in an immutable way => copy current state plus new data:
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

# Action => Reducer => Container 
The only way to change the state is by sending a signal to the state: an *action* initiates this signal and a *reducer* returns the new state
- Action creator is trigger through an certain event (e.g. product added to the basket)
- Action creator returns an object (which is called the action) that describes the action. It can return:
  - A 'type' property - a value that describe how / what state should change.
  - A 'payload' (optional) - includes specific data.
- Action automatically send to all reducers.
- Through the switch statement the reducer checks whether the action is related to the reducer.
```jsx
// state argument is not the app state, it is only refering to the state this reducer is responsible for
export default function(state = null, action) {
 switch(action.type) {
  case 'BOOK_SELECTED':
   return action.payload;
  }
  return state;
}
```
- Depending on the action, a reducer can return a new piece of state and that can be piped in the application state. If all reducers have been processed net state has been assembled.
- Then, containers will be notified of the changes to the state, causing the containers to re-render wit the new props.


#### Sources
- https://www.valentinog.com/blog/react-redux-tutorial-beginners/#React_Redux_tutorial_getting_to_know_Redux_actions


