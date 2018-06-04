### General
The -X parameter defines the HTTP method. If omitted it is assumed to be GET, but PUT, POST and DELETE (and others) are allowed.

### Adding data
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
    
### Retrieving a document
We can read the document back again with a GET request:
```
$ curl "$URL/newdb/2ded8ec775b6728227143ac575613060"
```
Response:
```
{"_id":"2ded8ec775b6728227143ac575613060","_rev":"1-0785e9eb543380151003dc452c3a001a","x":1}
```

### Modifying a document
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
