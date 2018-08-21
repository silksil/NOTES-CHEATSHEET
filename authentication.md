## Cookies vs Token

#### Cookie
- It brings state in a stateles protocoll (namely the http protocol)
- Unique to each domain. Cannot be send to different domains. 
- Are included by default in all requests, in the Header `cookie: {}`


#### Token
- Have to manually wire up > e.g. manually include it in the header
- Can be sent to any domain. So if you go to a different website, you are logged in. So if you have many different servers on many different domains, it is usefull to use tokens.
- Token is more trending, mainly because it is more scalable. 
- If you have a mobile app and a web app, it is  likely to be more usefull to use a single API server. This is therefore on a seperate server than the client side of a the web app, thus a token is likely to be more interesting.

## Token
#### Install
```npm install--save jwt-simple```

#### Function to generate token
```js
const jwt = require('jwt-simple');
const config = require('../config/keys')

function tokenForUser(user) {
  const timestamp = new Date().getTime();
  return jwt.encode({ sub: user.id, iat: timestamp}, config.tokenKey);
  // sub refers to the subject it belongs to, iat refers to issued at time
}
```

#### Send Token
```js
res.json({ token: tokenForUser(user) }
```












