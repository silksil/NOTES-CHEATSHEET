### Check if the data send to the DB is JSON

```javascript
/**
  * @function isJsonString - receives an JSON object, convert it into a string an try to parse it again into a json object
  * @param {object} jsonObject - JSON object being send
  */
  
function isJsonString(jsonObject) {
    try {
        let objectToTest = JSON.stringify(jsonObject);
        JSON.parse(objectToTest);
    } catch (e) {
        return false;
    }
    return true;
}
```

```javascript
/**
  * @param {object} responseData - An empty object which will be inserted with (informational) error messages
  */
let responseData = {};
```

```javascript
function displayDataError(response) {
  response.send(JSON.stringify(responseData));
  response.end();
}
```

```javascript
insertDoc = function(request, response) {
  const insertedDoc = request.body
  if (IsJsonString(insertedDoc)) {
  } else {
  responseData.error = true;
  responseData.reason = "Wrong input type!";
  responseData.description = "The input provided is not of the type JSON.".concat(JSON.stringify(request.body));
  displayDataError(response);
}
```
