### Check if the data send to the DB is a jsonObject

```javascript
/**
  * @function isJsonString Receive JSON object, convert it into a string an try to parse it into a json object again
  * @param {object} JSON - Test if object is a JSON object
  */
function issJsonString(jsonObject) {
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
insertRecord = function(request, response) {
  const insertedRecord = request.body
  if (IsJsonString(insertedRecord)) {
  } else {
  responseData.error = true;
  responseData.reason = "Wrong input type!";
  responseData.description = "The input provided is not of the type JSON.".concat(JSON.stringify(request.body));
  displayErrorData(response);
}
```
