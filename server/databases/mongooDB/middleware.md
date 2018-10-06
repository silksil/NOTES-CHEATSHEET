Middleware are functions that execute before or after certain distinct events that take place with Mongoose. There are two types of middleware:
- **Pre:** before an event takes place
- **Post:** after an event takes place.

### Example cases
After we remove a user, we also want to remove it's associated blog posts. 
