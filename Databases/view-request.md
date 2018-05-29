###module 
```javascript
const var request = require('request');
```

### Basic lay-out function
```javasript
function getInfoDb(req, res) {
  request(baseURL, function(error, response, body) {
    if (!error) {
      res.status(response.statusCode).send()
    } else {  
    }
  }
}
```


### Retrieve list of documents through a view
```javascript
baseUrl = { 
  method: 'POST',
  headers:
   { Authorization: 'Basic dXNlcm5hbWU6cGFzc3dvcmQ=',
     'Content-Type': 'application/json' },
  strictSSL: false,
  url: '"https://$ACCOUNT.cloudant.com/$DATABASE/_design/$DDOC/_view/by_ingredient?',
  body: '{"keys":["ingredient1","ingredient2","ingredient"]}' 
 }
```
