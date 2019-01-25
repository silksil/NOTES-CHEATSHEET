## Communicating through the Query vs. Body vs. URL
A REST API can have arguments in several places:
- In the request body - There are a bunch of different ways to format the data you POST to the server:
    1. application/x-www-form-urlencoded
    2. multipart/form-data
    3. application/json
    4. application/xml
    6. maybe some other
- In the query string - e.g. /api/resource?p1=v1&p2=v2
- As part of the URL-path - e.g. /api/resource/v1/v2

#### Query
##### URL
```javascript
url = "localhost:8080/api/test?name=" + name + "&id=" + id + "&key=" + key;
```
##### Route
```javascript
app.get('/api/test', controller.handleQuery)
```
##### Controller
``` javascript
module.exports.handleQuery = function(req, res) {
  let name = req.query.name;
  let id = req.query.id;
  let key = req.query.key;
}
```
##### Sources:
- https://stackoverflow.com/questions/4024271/rest-api-best-practices-where-to-put-parameters
- https://stackoverflow.com/questions/25385559/rest-api-best-practices-args-in-query-string-vs-in-request-body?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa


## BodyParser
### Install
Parse incoming request bodies in a middleware before your handlers, available under the req.body property.
`npm install --save body-parser`

### Assign Middleware
Express middlewares are always assigned to the app.use call.
```js
const bodyParser = require('body-parser');
app.use(bodyParser.json());
```
Now everythime there is a request (post, put etc.), the middleware will parse the body and assign it to req.body.


## Extract the path
```js
const { URL } = require('url');
const url = 'http://localhost:3000/api/surveys/5b60183955a627115d4a39e6/yes';
const pathname = new URL(url).pathname;

// pathname = /api/surveys/5b60183955a627115d4a39e6/thanks
```

## Extract URL parameters
```js
const Path = require('path-parser').default;
const { URL } = require('url');
const url = 'http://localhost:3000/api/surveys/5b60183955a627115d4a39e6/yes';

const path = new Path('/api/surveys/:surveyId/:choice');
const pathname = new URL(url).pathname;
const match = path.test(pathname);
if (match) {
  return { surveyId: match.surveyId, choice: match.choice };
}

// returns { surveyId: '5b60183955a627115d4a39e6', choice: 'yes' }
```
