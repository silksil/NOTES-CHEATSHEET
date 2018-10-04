#### Have change being made up Mongo
Case: If you want to update multiple record , you could fetch the records, than make a change and the send the data back to Mongo. Nonetheless, this is not the most efficient way in terms of performance. 

Instead, MongoDB provides you with modifiers/operators.See Link with update modifiers: https://docs.mongodb.com/manual/reference/operator/update/
Examples of modifiers:
- $inc: increment value.
- $mul: multiply value.
- $rename: rename a field. 
- $set: set value of a field => change value of a field to a new value. 
- $unset: unset value of a field. 

If you update one record, it is not necessary required. If you update multiple records, it is advised to use modifiers/operators.


