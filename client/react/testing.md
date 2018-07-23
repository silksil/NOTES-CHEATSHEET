### Jest and JSDOM
Test are run with Jest - an automated test runner. When we run Jest it is being run in our terminal. If we run React it always expects it is executed in the browser. As the command line and the browser are not the same thing, we use the dependancy JSDOM - this replicates how the browser works. 

### Basic set-up
What you do is that you take a component and put it inside a div. Then, we can test what is in that component by replicating it in the JSDOM. `ReactDOM.unmountComponentAtNode(div)`  this function looks to the div and remote the app component. Every time we start running tests, we start getting concerned about our performance, thus we unmount components.
```js
import React from 'react';
import ReactDOM from 'react-dom';

import App from '../App';

it('shows a comment box', () => {

  const div = document.create('div');

  ReactDOM.render(<App />, div);

  ReactDOM.unmountComponentAtNode(div);
});
```

### expect

```js
expect(div.innerHTML).toContain('Comment Box');
```


