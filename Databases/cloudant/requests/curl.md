## General
The -X parameter defines the HTTP method. If omitted it is assumed to be GET, but PUT, POST and DELETE (and others) are allowed.

## One Document
### Creating a document
We have to specify the data is JSON by specifying a “Content-type” header:
```
$ curl -X POST \
    -H "Content-type: application/json" \
    -d '{"x":1}' \
    "$URL/newdb"
```
Response:
```
{"ok":true,"id":"2ded8ec775b6728227143ac575613060","rev":"1-0785e9eb543380151003dc452c3a001a"}
```
    
### Reading a document
We can read the document back again with a GET request:
```
$ curl "$URL/newdb/2ded8ec775b6728227143ac575613060"
```
Response:
```
{"_id":"2ded8ec775b6728227143ac575613060","_rev":"1-0785e9eb543380151003dc452c3a001a","x":1}
```

### Updating a document
To create another revision we need to do a new POST request, passing in the new document body including the old document’s revision token:
```
$ curl -X POST \
       -H "Content-type: application/json" \
       -d '{"_id":"2ded8ec775b6728227143ac575613060","_rev":"1-0785e9eb543380151003dc452c3a001a","x":2}' \
      "$URL/newdb"
```
Response:
```
{"ok":true,"id":"2ded8ec775b6728227143ac575613060","rev":"2-8c49edca19d786e747fb5bea32c4cb91"}
```


### Deleting a document
The act of deleting a document causes a final revision to the document to be added with a `_deleted: true` flag added. Cloudant needs to know the ID of the document and the revision token that is to be deleted.

```
$ curl -X DELETE \
       "$URL/newdb/2ded8ec775b6728227143ac575613060?rev=2-8c49edca19d786e747fb5bea32c4cb91"
```
Response: 
```
{"ok":true,"id":"2ded8ec775b6728227143ac575613060","rev":"3-fb567087695adb203ba116e130794a84"}
```

Source: https://medium.com/ibm-watson-data-lab/cloudant-fundamentals-using-the-api-with-curl-4c4a4f278104

## Bulk
There are only two API calls you need to know about:
- GET or POST /db/_all_docs - for reads
- POST /db/_bulk_docs - for creates, updates and deletions

### Creating documents bulk
```
bulk.json = {
  "docs": [
    {"name": "Ferris Bueller", "actor": "Matthew Broderick", "dob": "1962-03-21"},
    {"name": "Sloane Peterson", "actor": "Mia Sara", "dob": "1967-06-19"},
    {"name": "Cameron Frye", "actor": "Alan Ruck", "dob": "1956-07-01"}
  ] 
}
```
```
$ curl -X POST \
       -H 'Content-type: application/json' \
       -d@bulk.json \
"$URL/newdb/_bulk_docs"
```
Response: 
``` 
[{"ok":true,"id":"6545abac34ff08ea39aaafb5ca1765c4","rev":"1-974e44505640c47cd31db6d4949aaff5"},{"ok":true,"id":"6545abac34ff08ea39aaafb5ca176920","rev":"1-9bcc8585a4cb3144fcffe8201f3c56d4"},{"ok":true,"id":"6545abac34ff08ea39aaafb5ca177037","rev":"1-8e73b2f79fcf8ef1cafa37d196808ecd"}]
```

### Reading documents bulk
```
$ curl "$URL/newdb/_all_docs"
```
Response:
```
{"total_rows":3,"offset":null,"rows":[
{"id":"6545abac34ff08ea39aaafb5ca1765c4","key":"6545abac34ff08ea39aaafb5ca1765c4","value":{"rev":"1-974e44505640c47cd31db6d4949aaff5"}},
{"id":"6545abac34ff08ea39aaafb5ca176920","key":"6545abac34ff08ea39aaafb5ca176920","value":{"rev":"1-9bcc8585a4cb3144fcffe8201f3c56d4"}},
{"id":"6545abac34ff08ea39aaafb5ca177037","key":"6545abac34ff08ea39aaafb5ca177037","value":{"rev":"1-8e73b2f79fcf8ef1cafa37d196808ecd"}}
]}
```
If you want the document bodies too, you have specify include_docs=true in your request:
```
$ curl "$URL/newdb/_all_docs?include_docs=true"
```
Response:
```
{"total_rows":3,"offset":0,"rows":[
{"id":"6545abac34ff08ea39aaafb5ca1765c4","key":"6545abac34ff08ea39aaafb5ca1765c4","value":{"rev":"1-974e44505640c47cd31db6d4949aaff5"},"doc":{"_id":"6545abac34ff08ea39aaafb5ca1765c4","_rev":"1-974e44505640c47cd31db6d4949aaff5","name":"Ferris Bueller","actor":"Matthew Broderick","dob":"1962-03-21"}},
{"id":"6545abac34ff08ea39aaafb5ca176920","key":"6545abac34ff08ea39aaafb5ca176920","value":{"rev":"1-9bcc8585a4cb3144fcffe8201f3c56d4"},"doc":{"_id":"6545abac34ff08ea39aaafb5ca176920","_rev":"1-9bcc8585a4cb3144fcffe8201f3c56d4","name":"Sloane Peterson","actor":"Mia Sara","dob":"1967-06-19"}},
{"id":"6545abac34ff08ea39aaafb5ca177037","key":"6545abac34ff08ea39aaafb5ca177037","value":{"rev":"1-8e73b2f79fcf8ef1cafa37d196808ecd"},"doc":{"_id":"6545abac34ff08ea39aaafb5ca177037","_rev":"1-8e73b2f79fcf8ef1cafa37d196808ecd","name":"Cameron Frye","actor":"Alan Ruck","dob":"1956-07-01"}}
]}
```

### Updating documents bulk

### Deleting documents bulk





