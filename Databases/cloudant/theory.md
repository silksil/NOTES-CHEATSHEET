## Intro
- The database contains a collection of json formatted documents. 
- When you create a document it automatically creatas a unique _id identifier, and must be unqiue in each database.
- Cloudant allows you to use a flexible schema: same or unqiue
- It can contain: numbers, string, nested objects or boolean data. 
- Fields starting with the underscore character _ are reserved for Cloudant-specific purposes. 
- You access Cloudant via HTTP/S. Typically you perform a GET request, but it could also be a PUT, POST, DELETE and COPY request.
- Views are the primary tool used for querying and reporting on CouchDB databases. They are defined in JavaScript (although there are other query servers available). Views can be created in the CouchDB/Cloudant database.

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

## Types of indexes
Different type of indexes you can use to query your database, and user cases for different types of indexes. See also https://developer.ibm.com/clouddataservices/docs/cloudant/indexes/

#### 1. Document lookup
If you need to find a document by id, use the direct document lookup. Built-in. http://ablanks.cloudant.com/animaldb/id

#### 2. Primary Index
Document id is used as the primary key. To get a list of of documents by their id. Built-in. ablanks.cloudant.com/animaldb/_all_docs

#### 3. Secondary Index (or View)
Is used if it requires more than lookups that uses the primary key, Is build by the map-reduce paradigm. Includes indexes that require mathematical calcaltions. Stored in design documents.

#### 4. Search Index
For ad hoc queries or searches that require large blocks of text, use a search index. Stored in design documents.

#### 5. Geospatial Index
Is used in the most efficient way to allow 4D quering. Stored in design documents.

#### 6. Cloudant Query
Can be build in the Cloudant Dashboard, or by posting JSON data.


## Views
A view can selectively filter documents. It can speed up searching for content. It can be used to 'pre-process' the results before they are returned to the client. Views are useful for many purposes:
- Filtering the documents in your database to find those relevant to a particular process.
- Extracting data from your documents and presenting it in a specific order.
- Building efficient indexes to find documents by any value or structure that resides in them.
- Use these indexes to represent relationships among documents.
- Finally, with views you can make all sorts of calculations on the data in your documents. For example, if documents represent your company’s financial transactions, a view can answer the question of what the spending was in the last week, month, or year.

You provide DB with view functions as strings stored inside the views field of a design document. You don’t run it yourself. Instead, when you query your view, the DB takes the source code and runs it for you on every document in the database your view was defined in. You query your view to retrieve the view result. 

If you read carefully over the last few paragraphs, one part stands out: “When you query your view, Cloudant takes the source code and runs it for you on every document in the database.” If you have a lot of documents, that takes quite a bit of time and you might wonder if it is not horribly inefficient to do this. Yes, it would be, but CouchDB is designed to avoid any extra costs: it only runs through all documents once, when you first query your view. If a document is changed, the map function is only run once, to recompute the keys and values for that single document. 

Using views, we can replace all of the queries we were performing on these tables, and the calculations would be performed once, and then stored. Accessing that data would be as simple as issuing a single HTTP request, which would efficiently pull the data from the view’s B-Tree. In other words, it would be fast, and very efficient.

Sources: 1. http://guide.couchdb.org/draft/views.html 2. http://johnpwood.net/2009/07/10/couchdb-views-the-advantages/

