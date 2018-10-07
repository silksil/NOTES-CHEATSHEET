Based on a collection of artists that has albums as their sub-documents.

### Single Record Update
```js
const Artist = require('../models/artist');

/**
 * Edits a single artist in the Artists collection
 * @param {string} _id - The ID of the artist to edit.
 * @param {object} artistProps - An object with a name, age, yearsActive, and genre
 * @return {promise} A promise that resolves when the record is edited
 */
module.exports = (_id, artistProps) => {
  return Artist.update({ _id }, artistProps);
};
```

### Bulk Update
```js
const Artist = require('../models/artist');

/**
 * Sets a group of Artists as retired
 * @param {array} _ids - An array of the _id's of of artists to update
 * @return {promise} A promise that resolves after the update
 */
module.exports = (_ids) => {
  return Artist.update(
    // look at id's and if the id is in the list of arguments
    { _id: { $in: _ids } },
    // update the status to true  
    { retired: true },
    // To update in bulk, you have to pass `multi` as true. By default it is false
    { multi: true }
  );
};
```
