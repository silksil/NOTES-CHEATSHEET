### URL
```javascript
url = "localhost:8080/api/test?name=" + name + "&id=" + id + "&key=" + key;
```

### Route
```javascript
app.get('/api/test', jsonParser, controller.handleQuery)
```

### Controller
``` javascript
let name = request.query.name;
let id = request.query.id;
let key = request.query.key;
```

