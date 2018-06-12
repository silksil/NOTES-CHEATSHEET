### Introduction
Passport is Express-compatible authentication middleware for Node.js. Passport includes 2 library components:
#### 1. Passport 
General helpers for handling auth in Express aps

#### 2. Passport Strategy
Helpers for authentication with one very specific method (email/password, Google, Facebook etc.)

### Installation
#### Install passport 
`npm install --save passport`

#### Install strategy
1. Google: `npm install --save passport-google-oauth20`

Go to https://console.developers.google.com and: `create project > enable API > search for Google+ API > should state OAuth 2.0 somewhere > enable > create Credentials > OAuth client id > configure consent screen > fill in data > select application type >> fill in data > create >` Get back:
1. clientId = public key
2. clientSecret = private key


