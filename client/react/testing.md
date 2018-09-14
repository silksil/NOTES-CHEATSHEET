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
`beforeEach` can be used to state that before each `it` function it will execute a certain function.
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







