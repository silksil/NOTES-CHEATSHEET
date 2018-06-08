
# One Document
### Creating a document
``` request = {"name": "Sil"}
```
```javascript
function insertDoc(request, response) {
    db.insert(request.body, function(error, result) {
        if(error){
        } else {
        }
    })
}
```
Response:
```
{"ok":true,"id":"2ded8ec775b6728227143ac575613060","rev":"1-0785e9eb543380151003dc452c3a001a"}
```
    
### Reading a document
```javascript
function getDocument(request, response) {
  db.find({
    'selector': {
      '_id': {request.body.id}
    },
    'fields': ['_id', 'name',] //optional to specify which data you want to get back
  }, function (error, data) {
    if(!err) {
    res.status(200).send(JSON.stringify(data));
  })
})

}

```
Response:
```
```

### Updating a document

```
function here
```
Response:
```
```


### Deleting a document

```
function here
```
Response: 
```
```

# Bulk

### Creating documents bulk
```
function here
```

Response: 
``` 
```

### Reading documents bulk
```
function here
```
Response:
```
```

### Updating documents bulk
```
function here

```
Response:
```

```

### Deleting documents bulk
```
function here
```
Response:
```
```
