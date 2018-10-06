### Updating a record 
Cases:  mainly for increment & decrement. For example, if you want to update multiple record , you could fetch the records, than make a change and the send the data back to Mongo. Nonetheless, this is not the most efficient way in terms of performance. 

Instead, MongoDB provides you with modifiers/operators.See Link with update modifiers: https://docs.mongodb.com/manual/reference/operator/update/
Examples of modifiers:
- `$inc`: increment value.
- `$mul`: multiply value.
- `$rename`: rename a field. 
- `$set`: set value of a field => change value of a field to a new value. 
- `$unset`: unset value of a field. 

If you update one record, it is not necessary required. If you update multiple records, it is advised to use modifiers/operators.

### Query: let mongo do the filtering
You have to query a filtered number of records, you could first query the records and then later filter the ones out, you want to leave out. Nonetheless, this is not best practice, as Mongodb may first has to query a large amount of records, before you can actually filter them. 

For example, you have to get the lowest number of a property assigned to a collection of users. You could do the fetch all instances and then take out the first one:
```js
const minQuery = Artist
  // find all artists
  .find({})
  /* 
    sort by age
    by including 1, you go from low to high
  */
  .sort({ age: 1 })
  .then(artists => artists[0].age);
```
If we would have many users in our db, this would significantly reduce our performance. So, instead we do the following:
```js
.find({})
    /* 
      sort by age
      by including 1, you go from low to high
    */
    .sort({ age: 1 })
    // only give me the first one
    .limit(1)
    .then(artists => artists[0].age);
```





