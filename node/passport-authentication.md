## Introduction
Passport is Express-compatible authentication middleware for Node.js. Passport includes 2 library components:
#### 1. Passport 
General helpers for handling auth in Express apps.

#### 2. Passport Strategy
Helpers for authentication with one very specific method (e.g Email, Google, Facebook, Linkedin)

##### Reading material: 
- https://medium.com/@bitshadow/how-basic-http-authentication-and-session-works-d29af9caec31
- https://www.quora.com/What-is-the-difference-advantage-between-of-using-cookie-session-and-token-based-authentication

## Example flow
Below you can find an example using Google OAuth and cookies. 

#### Connecting with Google OAuth
In the example below, passport is used to make connection to an strategy, get an profile, create new user/or identify existing user and the creation of an cookie. 
![Passport flow](../images/googleOauth-passport-cookies-flow.png?raw=true "Passport flow") </br>

#### Serialize and Deserialize
![Passport flow](../images/googleOauth-passport-cookies-flow-1.png?raw=true "Passport flow") </br>

#### The flow when a request comes in 
Once a hte cookie-session middleware  extract cookie data, passport than extract id from the cookie data and than queries the user by id. 
![Session to user flow](../images/session-to-user.png?raw=true "Session to user flow") </br>


## Installation
#### Install passport 
`npm install --save passport`
Cookie
```
npm install --save cookie-session

const cookieSession = require('cookie-session')
```
#### Install strategy
##### Google: 
`npm install --save passport-google-oauth20`

Go to https://console.developers.google.com and: `create project > enable API > search for Google+ API > should state OAuth 2.0 somewhere > enable > create Credentials > OAuth client id > configure consent screen > fill in data > select application type >> fill in data > create >` Get back:
1. clientId = public key
2. clientSecret = private key




