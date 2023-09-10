# Creating JWKS Endpoint

> This is a **prerequisite** for creating an OAuth 2.1 Client.

The JWKS is a set of keys containing the public keys used to verify any JSON Web Token
(JWT). Generate a set of asymmetric private and public keys for signing of claims for
connecting to Authorization Server using EC (P-256).

An example of an online key generator is at https://mkjwk.org, and sample code can be found [here](sections/oauth/sample-codes?id=generate-jwks-endpoint). Save the private key. You will need it
for [JWT](https://datatracker.ietf.org/doc/html/rfc7523) signing in later steps.

Host the JWKS generated in the previous step on a publicly accessible URL. You will use the
key-id ('kid') in later requests. Information on JWK Set format and JWK formats can be
found in [RFC7517](https://datatracker.ietf.org/doc/html/rfc7517). The keys 'kid' and 'use' may have to be manually added.

**Sample JWKS Endpoint (eg. https://{SWD_Domain}/.well-known/keys.json)**

```
{
 "keys": [
  {
   kty: "EC",
   kid: "key-id1",
   crv: "P-256",
   alg: "ES256",
   use: "sig",
   x: "SXUBSK5y9HO2Sjrac-N9VbSQaLAtePjtWGab9_Yoo1c",
   y: "l-aB05rylpiFcTNx-3H9JFUXtxoWXmLWOWO_
  }
 ]
}
```
