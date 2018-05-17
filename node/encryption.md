
##Crypto
#### Require
```javascript
const crypto = require('crypto');
const algorithm = 'aes-256-cbc';
let config = require('config');
const password = config.get('encrypt-string');
```

#### Encrypt
```javascript
module.exports.encrypt = function(encValues){
let cipher = crypto.createCipher(algorithm, password)
let crypted = cipher.update(encValues,'utf8','hex')
    crypted += cipher.final('hex');
    return crypted;
}

let encryptionKey = encrypt(encValues);
```

let decrypt = function(decValues){
let decipher = crypto.createDecipher(algorithm, password)
let deccrypted = decipher.update(decValues,'hex','utf8')
    deccrypted += decipher.final('utf8');
    return deccrypted;
  }
  
let decryptedKey = decrypt(decValues);

```javascript



              
              
