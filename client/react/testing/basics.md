### Jest and JSDOM
Test are run with Jest - an automated test runner. When we run Jest it is being run in our terminal. If we run React it always expects it is executed in the browser. As the command line and the browser are not the same thing, we use the dependancy JSDOM - this replicates how the browser works. 

### Basic set-up
What you do is that you take a component and put it inside a div. Then, we can test what is in that component by replicating it in the JSDOM. 
- `expect` is global function. After expect we specify **what**  we are inspecting (in the example below that is `div.innerHTML`). Then we include a matcher statement, stating **how** we want to inspect the 'subject' (in this case `toContain`). Lastly, we state the **expected value**, it's what we want our 'subject' to be (in this case `Comment Box`).
- `ReactDOM.unmountComponentAtNode(div)`  this function looks to the div and remote the app component. Every time we start running tests, we start getting concerned about our performance, thus we unmount components.

```js
import React from 'react';
import ReactDOM from 'react-dom';

import App from '../App';

it('shows a comment box', () => {

  const div = document.create('div');

  ReactDOM.render(<App />, div);
  
  expect(div.innerHTML).toContain('Comment Box');

  ReactDOM.unmountComponentAtNode(div);
});
```
### Enzyme
Enzyme is a JavaScript Testing utility for React that makes it easier to assert, manipulate, and traverse your React Components' output.
```js
npm install --save-dev enzyme enzyme-adapter-react-${versionReact}
```
Create file with the name setupTests.js and wire it up to the project
```js
import Enzyme from 'enzyme';
import Adapter from 'enzyme-adapter-react-16'; // 16 refers to the version of React, so could be 17, 18 etc.

Enzyme.configure({ adapter: new Adapter() });
```
Enzyme gives us three additional capabilities. These are all render functions take an instance of our component, tries to render them, and then returns us an object that we can use to write tests around.
- ***Static Render*** : Render the given component and return HTML plain. It does not allow us to interact, e.g. clicking. 
- ***Shallow Render***: Render 'just' the given component and none of its children. 
- ***Full DOM***: Render the component and all of its children + let us modify it afterwards (e.g. by clicking). 

If you use Enzyme the set-up is a bit different:
```js
import React from 'react';
import { shallow } from 'enzyme';

import App from '../App';
import CommentBox from '../CommentBox';

it('shows a comment box', () => {
  const wrapped = shallow(<App />);

  expect(wrapped.find(CommentBox).length).toEqual(1);
});
```
### beforeEach
`beforeEach` can be used to state that before each `it` function it will execute a certain function. In the case below, we mount a  component to the fake DOM everytime we we conduct a test.
```js
let wrapped;
beforeEach(() => {
  wrapped = shallow(<App />);

})
it('shows a comment box', () => {
  expect(wrapped.find(CommentBox).length).toEqual(1);
});

it('shows a comment list', () => {
  expect(wrapped.find(CommentList).length).toEqual(1);
});
```
### Set-up with Redux
When testing components that include Redux functionality, it should be considered that the components that include Redux will be connected through `connect()` with the `Provider` to directly communicate with this component. This is a high-order component, and as a result tests that are connected to Redux will fail if we do not take this in consideration. To solve this, you can split up your index.js or app.js file by seperating the Provider tags from the rest. Alternatively, you could include all Provider tags manually in the tests, but the risks is then that if you will add or remove middleware from the real app, you will conduct tests that will not replicate the real app. 

Thus, we create a new file, called Root.js:
```js
import React from 'react';
import { Provider } from 'react-redux';
import { createStore } from 'redux';
import reducers from './reducers';

export default (props) => {
  return (
    <Provider store={createStore(reducers, {})}>
      {props.children}
    </Provider>
  )
}
// props.children allows us to include the children that are included within the Root tag,
// in this case <App />
```
Then we import the Root tag in the index.js file. 
```js
import React from 'react';
import ReactDOM from 'react-dom';
import Root from 'Root';
import App from './components/App';

ReactDOM.render(
  <Root>
    <App />
  </Root>,
  document.querySelector('#root')
);
```
Lastly, we can include the Root file in the test by wrapping it around the component that includes Redux:
```js
import React from 'react';
import { mount } from 'enzyme';
import CommentBox from '../CommentBox';
import Root from '../../Root';

let wrapped;

beforeEach(() => {
  wrapped = mount(
    <Root>
      <CommentBox />
    </Root>
  );
});
```

### afterEach 
`afterEach` can be used to clean things up after each `it` function -- it will unmount the components from the fake dom. 
```jsx
import React from 'react';
import { mount } from 'enzyme';
import CommentBox from '../CommentBox';

let wrapped;

beforeEach(() => {
  wrapped = mount(<CommentBox />);
});

afterEach(()=> {
  wrapped.unmount();
})

it('it has a text area and a button', () => {
  expect(wrapped.find('textarea').length).toEqual(1);
  expect(wrapped.find('button').length).toEqual(1);
});
```

### simulate 
`Simulate` allows you to simulate an event. The example simulates a change event. The name of the event should be a html event name.  
```js
it('it has a text area that users can type in', () => {
  wrapped.find('textarea').simulate('change', {
    target: { value: 'new comment' }
  });
  // simulate the 'change' event
  // Provide a fake event object => a mock event object that will be merged with the event object passed to the handlers
});
```











