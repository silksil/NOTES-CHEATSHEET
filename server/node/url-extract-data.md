### Extract the path
```js
const { URL } = require('url');

const url = 'http://localhost:3000/api/surveys/5b60183955a627115d4a39e6/thanks'

const pathname = new URL(url).pathname;

// pathname = /api/surveys/5b60183955a627115d4a39e6/thanks
```
