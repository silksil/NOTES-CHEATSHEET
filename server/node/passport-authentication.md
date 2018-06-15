# Introduction
### Passport
Passport is Express-compatible authentication middleware for Node.js. Passport includes 2 library components:
##### 1. Passport 
General helpers for handling auth in Express apps.

##### 2. Passport Strategy
Helpers for authentication with one very specific method (e.g Email, Google, Facebook, Linkedin)

### Cookies
#### Difference cookie-session and express-session
The key difference is how the data is being stored in the cookie; cookie-session stores all info in the cookie, and with express-session you store all the data outside of the cookie. With express-session you can store more data. 

##### cookie-session
If we use the cookie-session library we say that the cookie is the session. The cookie contains all the data concerning the current session; it contains the user_id. To find out which user it is, we look to the cookie, decode the value and that is the exact value that is stored in req.session. 

##### express-session
Express session stores a reference to a session. It will store inside the cookie an id to a session. Express session takes the id, then looks up all relevant session data, or 'session 'store' in a remote db. This will then retrieve us the user info. 

## Example flow
Below you can find an example using Google OAuth and the cookie-sessions library. 

#### Connecting with Google OAuth
In the example below, passport is used to make connection to an strategy, get an profile, create new user/or identify existing user and the creation of an cookie. 
![Passport flow](../../images/googleOauth-passport-cookies-flow.png?raw=true "Passport flow") </br>

#### Serialize and Deserialize
![Passport flow](../../images/googleOauth-passport-cookies-flow-1.png?raw=true "Passport flow") </br>

#### The flow when a request comes in 
When the request comes in, the cookie-session middleware extract cookie data, passport than extract id from the cookie data and than queries the user by id. 

![Session to user flow](../../images/session-to-user.png?raw=true "Session to user flow") </br>


# Installation
#### Install passport 
`npm install --save passport`

#### Install cookie-session
`npm install --save cookie-session` 

#### Install strategy
##### Google: 
`npm install --save passport-google-oauth20`

Go to https://console.developers.google.com and: `create project > enable API > search for Google+ API > should state OAuth 2.0 somewhere > enable > create Credentials > OAuth client id > configure consent screen > fill in data > select application type >> fill in data > create >` Get back:
1. clientId = public key
2. clientSecret = private key
