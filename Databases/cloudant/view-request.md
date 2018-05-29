### config
```javascript
const request = require('request');
```

### Basic lay-out function
```javascript
function getInfoDb(req, res) {
  request(baseURL, function(error, response, body) {
    if (!error) {
      res.status(response.statusCode).send()
    } else {  
    }
  }
};
```
  
### Request through a view
  ```javascript
baseUrl = { 
    method: 'GET',
    headers: { Authorization: 'Basic dXNlcm5hbWU6cGFzc3dvcmQ=', 'Content-Type': 'application/json' },
   url: '"https://$ACCOUNT.cloudant.com/$DATABASE/_design/$DDOC/_view/by_ingredient?include_docs=true'
};
```
  
  

### Retrieve list of documents through a view
```javascript
baseUrl = { 
  method: 'POST',
  headers: { Authorization: 'Basic dXNlcm5hbWU6cGFzc3dvcmQ=', 'Content-Type': 'application/json' },
  url: '"https://$ACCOUNT.cloudant.com/$DATABASE/_design/$DDOC/_view/by_ingredient?include_docs=true',
  body: '{"keys":["ingredient1","ingredient2","ingredient"]}' 
 };
```
