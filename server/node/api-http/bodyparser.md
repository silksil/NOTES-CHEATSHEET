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
