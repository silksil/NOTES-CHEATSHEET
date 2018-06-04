## Intro
- The database contains a collection of json formatted documents. 
- When you create a document it automatically creatas a unique _id identifier, and must be unqiue in each database.
- Cloudant allows you to use a flexible schema: same or unqiue
- It can contain: numbers, string, nested objects or boolean data. 
- Fields starting with the underscore character _ are reserved for Cloudant-specific purposes. 
- You access Cloudant via HTTP/S. Typically you perform a GET request, but it could also be a PUT, POST, DELETE and COPY request.

## The Document
### Multiple object types in the same database
One widely used pattern that is odd to folks coming from a relational background is storing different object types in the same database. You could have blog posts and authors in the same Cloudant database using their own separate schema. One convention is to use the type field to indicate which flavour of object it is. 

```javascript
{
  "type": "post",
  "name": "Thought Leadership for Beginners",
  "description": "How to think yourself to success.",
  "url": "https://myblog.com/2018-04-02-thougt-leadership.html",
  "data": "2018-04-02",
  "author": "John Doe"
}
```

```javascript
{
  "type": "author",
  "name": "John Doe",
  "url": "https://myblog.com/",
  "registered": "2005-09-18",
  "email": "john.doe@myspace.com",
  "confirmed": true
}
```

Source: https://medium.com/ibm-watson-data-lab/cloudant-fundamentals-the-document-855c5ab92051

### Write-only document patterns
Cloudant allows documents to be updated, but not on a field-by-field basis. Your app would have to fetch the whole document and write the whole, modified document back to the database.

## The _id
Every Cloudant document has an _id - if you don't supply one when you write a new document then Cloudant will generate one for you. Letting Cloudant make an _id for you is the easiest solution, but there are some cases where you might want to keep control of the _id field for yourself.  Cloudant can retrieve a document given its _idvery quickly by consulting the index - without having to page through all the documents in the collection to find the right one.

If we know something unique about our user, such as their email address, we could modify the document to look like this:
```javascript
{
  "_id": "user:abe.froman@aol.com",
  "type": "user",
  "name": "Abe Froman",
  "email": "abe.froman@aol.com",
  "registered": "2018-03-09T11:48:11.491Z",
  "profile": "Sausage maker",
  "city": "Chicago"
}
```

Source: https://medium.com/ibm-watson-data-lab/cloudant-fundamentals-the-id-f6c7c88fbc75

## The _rev
The _rev token consists of two parts separated by a hyphen character -:
- a number that increments with each version of the document
- a 32-character string that is a cryptographic hash of the document’s body.

The _rev should be used if we want to update or delete a document. The _rev token keeps track of the revisions that a document goes through in its life. 

    First revision 1–25f9b97d75a648d1fcd23f0a73d2776e
    Second revision 2–524e981baaeec9bbecf92c4c01242308
    Third revision 3-e95ca5ca4dc5407fd09b8e0e0acf25fd
    
    
Source: https://medium.com/ibm-watson-data-lab/cloudant-fundamentals-the-rev-token-fb0fc19a3145




