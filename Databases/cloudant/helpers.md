### Check if the data send to the DB is a jsonObject

```javascript
/**
  * @function isJsonString - receives an JSON object, convert it into a string an try to parse it again into a json object
  * @param {object} JSON - Test if object is a JSON object
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
  * @param {object} responseData - Empty object to be populated with (informational) error messages
  */
let docList = [];
let responseData = {};
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
