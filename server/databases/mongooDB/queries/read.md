Context: Based on a collection of artists that has albums as their sub-documents.
### Read min or max of property assigned to a collection of records
```js
const Artist = require('../models/artist');

/**
 * Finds the lowest and highest age of artists in the Artist collection
 * @return {promise} A promise that resolves with an object
 * containing the min and max ages, like { min: 16, max: 45 }.
 */
module.exports = () => {
  const minQuery = Artist
    // find all artists
    .find({})
    /*
      sort by age
      by including 1, you go from low to high
    */
    .sort({ age: 1 })
    // only give me the first one
    .limit(1)
    .then(artists => artists[0].age);

  const maxQuery = Artist
    .find({})
    .sort({ age: -1 })
    .limit(1)
    .then(artists => artists[0].age);

  return Promise.all([minQuery, maxQuery])
    .then((result) => {
      return { min: result[0], max: result[1] };
    });
};
```

### Get instances of collection + sort it by a property + limit amount per page + count amount of results
```js
/**
 * Searches through the Artist collection
 * @param {string} sortProperty The property to sort the results by
 * @param {integer} offset How many records to SKIP in the result set
 * @param {integer} limit How many records to return in the result set
 * @return {promise} A promise that resolves with the artists, count, offset, and limit
 * like this: { all: [artists], count: count, offset: offset, limit: limit }
 */

 // the values of offset and limit are default values
module.exports = (criteria, sortProperty, offset = 0, limit = 20) => {

  const query = Artist.find({})
  /*
  We add a property to an object, not an array. This is es6, same as:
  - const sortOrderandProperty = {};
  - sortOrderandProperty[sortProperty] = 1;
  .- sort(sortOrder)
  */
  .sort({ [sortProperty]: 1 })
  // offset default value is 0 but can change
  .skip(offset)
  .limit(limit);

// count refers to the amount of artists
return Promise.all([query, Artist.count()])
  .then((results) => {
    return {
      all: results[0],
      count: results[1],
      offset: offset,
      limit: limit
    };
  });
};
```

### Get instances of collection + filter by single property + sort it by a property + limit amount per page + count amount of results, 
```js
const Artist = require('../models/artist');

/**
 * Searches through the Artist collection
 * @param {object} criteria An object with a name, age, and yearsActive
 * @param {string} sortProperty The property to sort the results by
 * @param {integer} offset How many records to SKIP in the result set
 * @param {integer} limit How many records to return in the result set
 * @return {promise} A promise that resolves with the artists, count, offset, and limit
 * like this: { all: [artists], count: count, offset: offset, limit: limit }
 */

 // the values of offset and limit are default values
module.exports = (criteria, sortProperty, offset = 0, limit = 20) => {

  const query = Artist.find(buildQuery(criteria))
  /*
  We add a property to an object, not an array. This is es6, same as:
  - const sortOrderandProperty = {};
  - sortOrderandProperty[sortProperty] = 1;
  .- sort(sortOrder)
  */
  .sort({ [sortProperty]: 1 })
  // offset default value is 0 but can change
  .skip(offset)
  .limit(limit);

// count refers to the amount of artists
return Promise.all([query, Artist.count()])
  .then((results) => {
    return {
      all: results[0],
      count: results[1],
      offset: offset,
      limit: limit
    };
  });
};


const buildQuery = (criteria) => {
  const query = {};

  if (criteria.age) {
    query.age = {
      $gte: criteria.age.min,
      $lte: criteria.age.max
    };
  }

  return query;
};
```

