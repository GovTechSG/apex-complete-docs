# Sample Codes (Consumers)

The Sample Codes below are meant for reference and are not for production use.

## Generate Code Challenge and Code Verifier

The follow code is a sample code to generate the Code Challenge and Code Verifier.

```
// DEPENDENCIES
const jose = require('node-jose');
/**
  * @module crypto
  * @description Node.js built-in crypto module
  * @link https://nodejs.org/api/crypto.html#crypto_crypto
  */
const crypto = require("crypto");
/**
  * @module crypto
  * @description JSON Object Signing and Encryption (JOSE) module via NPM
  * @link https://www.npmjs.com/package/node-jose
  */
const jose = require('node-jose');
// CREATE KEYSET
async function key(){
  try{
  let key = crypto.generateKeyPairSync('ec', {
    namedCurve: 'prime256v1',
    publicKeyEncoding: {
      type: 'spki',
      format: 'pem'
    },
    privateKeyEncoding: {
    type: 'pkcs8',
    format: 'pem'
    }
  })
  let cryptoKey = await jose.JWK.asKey(key.privateKey, "pem");
   console.log(cryptoKey.toPEM()) // X.509 public key
   console.log(cryptoKey.toPEM(true)) // X.509 private key
  console.log(cryptoKey.toJSON()) // JSON format of public key
 // This can be used for the JWKS endpoint
  console.log(cryptoKey.toJSON(true)) // JSON format of private key

} catch(e) {console.log(e);}}

key();
```

Source: Auth0

## Generate JWKS Endpoint

The follow code is a sample code to generate the JWK used in JWKS endpoint.

```
'use strict';

// DEPENDENCIES
const jose = require('node-jose');
const crypto = require('crypto');

// CREATE JWKS
async function jwks() {

    /*
        CREATE JWK
    */

    let key = crypto.generateKeyPairSync('ec', {
      namedCurve: 'prime256v1',
      publicKeyEncoding: {
        type: 'spki',
        format: 'pem',
      },
      privateKeyEncoding: {
        type: 'pkcs8',
        format: 'pem',
      },
    });

    let cryptoKey = await jose.JWK.asKey(key.privateKey, 'pem');

    let publicKeyJSON = cryptoKey.toJSON();
    let privateKeyJSON = cryptoKey.toJSON(true);

    let jwksEndpoint = {
      keys: [{...publicKeyJSON,
              ...{use: "sig"},
              ...{crv: "P-256"},
              ...{alg: "ES256"},
    }]};

    console.log("Public keys:");
    console.log(publicKeyJSON);

    console.log("JWKS Endpoint:");
    console.log(jwksEndpoint);

    // console.log("Private keys:");
    // console.log(privateKeyJSON);

    // console.log("Private keys(PEM):");
    // console.log(cryptoKey.toPEM(true));
}
jwks();

```

```
% node create-keys.js
Public keys:
{
  kty: 'EC',
  kid: 'vyaeBP-ohJ5RF_cEEU1v0h3IEIoWCa24YKZK4ddb4-U',
  crv: 'P-256',
  x: 'RYvFyXBZLa7TWEDlk3sqqyhxXkTYGkEEl5AuANZ70Ps',
  y: 'JKobI-7Z4xBCGDKDubtBF-sHvC69F6sKGbRX3RIaGhY'
}
JWKS Endpoint:
{
  keys: [
    {
      kty: 'EC',
      kid: 'vyaeBP-ohJ5RF_cEEU1v0h3IEIoWCa24YKZK4ddb4-U',
      crv: 'P-256',
      x: 'RYvFyXBZLa7TWEDlk3sqqyhxXkTYGkEEl5AuANZ70Ps',
      y: 'JKobI-7Z4xBCGDKDubtBF-sHvC69F6sKGbRX3RIaGhY',
      use: 'sig',
      alg: 'ES256'
    }
  ]
}
```

## Generate Client Assertion

The follow code is a sample code to generate the Client Assertion.

```
/**
  * @module jsonwebtoken
  * @description JSON Web Tokens module via NPM
  * @link https://www.npmjs.com/package/jsonwebtoken
*/
const jwt = require('jsonwebtoken');
/**
  * @module fs
  * @description Node.js built-in File System module
  * @link
  */
const fs = require('fs');
const epoch = new Date().getTime();
let payload = {};
let privateKey = fs.readFileSync('./private.key', 'utf8'); // your private key generated in Initial
Setup in PEM format
let signOptions = {
  issuer: 'STG-xxxx', // your Client ID issued during Initial Setup
  subject: 'STG-xxxx', // your Client ID issued during Initial Setup
  audience: 'https://public.api.gov.sg/oauth/cp/v2/token',
  jwtid: 'session-" + epoch, // your Session ID – used for troubleshooting and preventing replay attacks
  keyid: 'key-id1", // your key ID in JWKS endpoint from Initial Setup
  expiresIn: '5m',
  algorithm: 'ES256'
}
let clientAssertion = jwt.sign(payload, privateKey, signOptions);
// Signing Client Assertion will prove client is the issuer of client assertion
```
