### HTTP – the Standard Library
```javascript
const https = require('https');

https.get('https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY', (res) => {
  let data = '';

  // A chunk of data has been recieved.
  res.on('data', (chunk) => {
    data += chunk;
  });

  // The whole response has been received. Print out the result.
  res.on('end', () => {
    console.log(JSON.parse(data).explanation);
  });

}).on("error", (err) => {
  console.log("Error: " + err.message);
});
```
### Request 
```javascript
const request = require('request');
 
request('https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY', { json: true }, (err, res, body) => {
  if (err) { return console.log(err); }
  console.log(body.url);
  console.log(body.explanation);
});
```

### Axios
```javascript
const axios = require('axios');
 
axios.all([
  axios.get('https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY&date=2017-08-03'),
  axios.get('https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY&date=2017-08-02')
]).then(axios.spread((response1, response2) => {
  console.log(response1.data.url);
  console.log(response2.data.url);
})).catch(error => {
  console.log(error);
});
 ```
