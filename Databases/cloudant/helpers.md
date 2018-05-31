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
