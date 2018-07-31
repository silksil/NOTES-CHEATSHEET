### Extract the path
```js
const { URL } = require('url');
const url = 'http://localhost:3000/api/surveys/5b60183955a627115d4a39e6/yes';
const pathname = new URL(url).pathname;

// pathname = /api/surveys/5b60183955a627115d4a39e6/thanks
```

### Extract URL parameters
```js
const Path = require('path-parser').default;
const { URL } = require('url');
const url = 'http://localhost:3000/api/surveys/5b60183955a627115d4a39e6/yes';
const pathname = new URL(url).pathname;
const match = path.test(pathname)
if (match) {
  return { email, surveyId: match.surveyId, choice: match.choice }
}

// returns { surveyId: '5b60183955a627115d4a39e6', choice: 'yes' }
```
