See also https://developer.ibm.com/clouddataservices/docs/cloudant/indexes/

Different type of indexes you can use to query your database, and user cases for different types of indexes. 

### 1. Document lookup
If you need to find a document by id, use the direct document lookup. Built-in. 
```http://ablanks.cloudant.com/animaldb/id```

### 2. Primary Index 
Document id is used as the primary key.  To get a list of of documents by their id. Built-in.
```ablanks.cloudant.com/animaldb/_all_docs```

### 3. Secondary Index (or View)
Is used if it requires more than lookups that uses the primary key, Is build by the map-reduce paradigm. Includes indexes that require mathematical calcaltions. Stored in design documents.


### 4. Search Index
For ad hoc queries or searches that require large blocks of text, use a search index. Stored in design documents.

### 5. Geospatial Index
Is used in the most efficient way to allow 4D quering. Stored in design documents.

### 6. Cloudant Query
Can be build in the Cloudant Dashboard, or by posting JSON data. 

