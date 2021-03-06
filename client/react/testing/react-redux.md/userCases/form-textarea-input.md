The following page describes three tests. 
1.) To check whether a component has certain elements.
2.) To check whether it has a element where users can type in.
3.) To check whether an element is emptied after a text area is submitted.

### Component
```jsx
import React, { Component } from 'react';

class CommentBox extends Component {
  state = { comment: '' };

  handleChange = (event) => {
    this.setState({ comment: event.target.value});
  };

  handleSubmit = (event) => {
    event.preventDefault();

    this.setState({ comment: '' });
  };

  render() {
    return(
      <form onSubmit={this.handleSubmit}>
        <h4> Add a comment </h4>
        <textarea onChange={this.handleChange} value={this.state.comment} />
        <div>
          <button> Submit Comment </button>
        </div>
      </form>
    )
  }
}

export default CommentBox;
```
### Test 1: To check whether a component has certain elements.
```js
import React from 'react';
import { mount } from 'enzyme';
import CommentBox from '../CommentBox';

let wrapped;

beforeEach(() => {
  wrapped = mount(<CommentBox />);
});

afterEach(()=> {
  wrapped.unmount();
});

it('it has a text area and a button', () => {
  expect(wrapped.find('textarea').length).toEqual(1);
  expect(wrapped.find('button').length).toEqual(1);
});
```

### Test 2: To check whether it has a element where users can type in
The test below tests a textarea/input field.  It:
1. Find the textarea element
2. Simulate the 'change' event
3. Provide a fake event object => a mock event object that will be merged with the event object passed to the handlers
4. Force the component to update => setState is asynchronous. Thus, you have to force it to re-render.
5. Assert that the textareas value has changed
```js
it('it has a text area that users can type in', () => {
  wrapped.find('textarea').simulate('change', { // 1. find the textarea element 2. simulate the 'change' event
    target: { value: 'new comment' } // 3. Provide a fake event object
  });
  wrapped.update(); // 4. Force the component to update => setState is asynchronous. Thus, you have to force it to re-render.
  expect(wrapped.find('textarea').prop('value')).toEqual('new comment'); // 5. Assert that the textareas value has changed
  // the prop method allows you to retrieve the prop that you assigned to an element.
  // In this case you check whether the prop `value` is equal to the string 'new comment'
});
```

### Test 3: To check whether an element is emptied after a text area is submitted.
```js
// to trick that it is submitted we have to simulate a submit event, rather than a click on the button
it('it has a text area that empties after it is submitted', () => {
  wrapped.find('textarea').simulate('change', {
    target: { value: 'new comment' }
  });
  wrapped.update();
  expect(wrapped.find('textarea').prop('value')).toEqual('new comment');
  wrapped.find('form').simulate('submit');
  wrapped.update();
  expect(wrapped.find('textarea').prop('value')).toEqual('');
});
```
