CLOUDANT / DASHDB

Views
Views are the primary tool used for querying and reporting on CouchDB databases. They are defined in JavaScript (although there are other query servers available).
Views can be created in the CouchDB/Cloudant database.

[-]_rev id should be used if we want to update or delete a document. > if you update it can generate a new rev_id. Multiple locations may use same id but different rev. 


Configuration

let Cloudant = require('cloudant'); // Load the Cloudant library
let cloudantHost = config.get('cloudant.dev'); //should include a link to the account and database
let cloudant;
let db;






let Cloudant = require('cloudant');

cloudant = Cloudant({ url: cloudantHost});
db = cloudant.db.use("alert_dev");
