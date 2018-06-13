## Introduction
Passport is Express-compatible authentication middleware for Node.js. Passport includes 2 library components:
#### 1. Passport 
General helpers for handling auth in Express apps.

#### 2. Passport Strategy
Helpers for authentication with one very specific method (e.g Email, Google, Facebook, Linkedin)


## Example flow
Below you can find an example using Google oAuth. 

In the example below, passport is used to make connection to an strategy, get an profile, create new user/or identify existing user and create a cookie to identify a user. 

![Passport flow](../images/googleOauth-passport-cookies-flow.png?raw=true "Passport flow") </br>

Users info is used to create a cookie. If we request info, the same cookie is deserialized to identify the user. 

![Passport flow](../images/googleOauth-passport-cookies-flow-1.png?raw=true "Passport flow") </br>

Session to user flow: 

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


