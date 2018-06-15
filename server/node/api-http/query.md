### URL
```javascript
url = "localhost:8080/api/test?name=" + name + "&id=" + id + "&key=" + key;
```

### Route
```javascript
app.get('/api/test', controller.handleQuery)
```

### Controller
``` javascript
module.exports.handleQuery = function(req, res) {
  let name = req.query.name;
  let id = req.query.id;
  let key = req.query.key;
}
```

